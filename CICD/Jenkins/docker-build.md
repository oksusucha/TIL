### Run Jenkins
```Shell
docker run -d -p 8080:8080 -v /var/run/docker.sock:/var/run/docker.sock -v /jenkins:/var/jenkins_home --name jm_jenkins -u root jenkins/jenkins:lts
```

### Install Docker in Jenkins container
```Shell
apt-get update && \
apt-get -y install apt-transport-https \
     ca-certificates \
     curl \
     gnupg2 \
     software-properties-common && \
curl -fsSL https://download.docker.com/linux/$(. /etc/os-release; echo "$ID")/gpg > /tmp/dkey; apt-key add /tmp/dkey && \
add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/$(. /etc/os-release; echo "$ID") \
   $(lsb_release -cs) \
   stable" && \
apt-get update && \
apt-get -y install docker-ce
```

### In Jenkins web
#### Install plugins
* docker
* docker-build-step

#### Settings
*  jenkins 관리 -> 환경설정 -> Docker Builder
*  unix:///var/run/docker.sock 설정
*  테스트 및 저장
