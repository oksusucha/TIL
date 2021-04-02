### docker build 사용법
### Docker pipeline 플러그인을 미리 설치 
``` 
pipeline {
    agent any
    stages {
        stage('다른 작업') {
          // 다른작업 내용
        }
        stage('Build dockerfile') {
            when {
                expression {
                    return env.cloneResult ==~ /(?i)(Y|YES|T|TRUE|ON|RUN)/
                }                
            }
            steps{
                script{
                    try {
                        docker.build "${ECR_URL}/${SERVICE_NAME}:latest"
                        env.buildResult=true
                    } catch (error) {
                        print(error)
                        env.buildResult=false
                        currentBuild.result = 'FAILURE'
                    }
                }
            }
        }   
    }
}
```
