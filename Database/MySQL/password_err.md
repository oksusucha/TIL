### MySQL 에서 설치 후 caching_sha2_password 에러 발생 시 해결 법

```SQL
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'your mySQL initial password';
```
