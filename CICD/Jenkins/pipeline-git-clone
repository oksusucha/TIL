### Jenkins pipeline, git 
```
pipeline {
    agent any
    stages {
      stage('Git clone') {
            steps{
                script{
                    try{
                        git url: 'Repogitory URL', branch: '브랜치명', credentialsId: 'Jenkins 환경 설정에 등록해논 id 값'
                        sh "git status"
                        sh "rm -rf ./.git"
                        env.cloneResult=true
                    } catch (error) {
                        print(error)
                        env.cloneResult=false
                        currentBuild.result = 'FAILURE'
                    }
                }
            }
        }
    }
}
```
