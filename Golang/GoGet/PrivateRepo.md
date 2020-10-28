### go get -u PRIVATE-REPOSITORY

참고: https://support.atlassian.com/bitbucket-cloud/docs/set-up-an-ssh-key/#SetupanSSHkey-ssh2

* MAC 에서 go get 할때 403 오류
```log
Unauthorized
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.
go: missing Mercurial command. See https://golang.org/s/gogetcmd
# cd .; git ls-remote https://bitbucket.org/USERNAME_OR_ORGANISATION/REPOSITORY
Unauthorized
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.
go: missing Mercurial command. See https://golang.org/s/gogetcmd
go get bitbucket.org/xinapsedev/malmoyidl: reading https://api.bitbucket.org/2.0/repositories/USERNAME_OR_ORGANISATION/REPOSITORY?fields=scm: 403 Forbidden
	server response: Access denied. You must have write or admin access.
```

#### Step 1. Set up your default identity

```bash
$ ssh-keygen 
```

* 참고

|공개키|비밀키|
|---|---|
|KEY_FILE_NAME.pub|KEY_FILE_NAME|

#### Step 2. Add the key to the ssh-agent

```bash
$ eval `ssh-agent`
```
```bash
macOS   $ ssh-add -K ~/.ssh/<private_key_file>       
Linux   $ ssh-add ~/.ssh/<private_key_file>       
```
```bash
# MAC OS only
Host *
UseKeychain yes
```

#### Step 3. (Mercurial only) Enable SSH compression

``` bash
$ vi ~/.hgrc
```

```bash
[ui]
# Name data to appear in commits
username = Emma <emmap1@atlassian.com>
ssh = ssh -C
```

#### Step 4. Add the public key to your Account settings

```bash
# Linux
$ cat ~/.ssh/id_rsa.pub

# macOs
$ pbcopy < ~/.ssh/id_rsa.pub 
```
1. From Bitbucket, click Add key.
2. Enter a Label for your new key, for example, Default public key.
3. Paste the copied public key into the SSH Key field.
You may see an email address on the last line when you paste. It doesn't matter whether or not you include the email address in the Key.

