pipeline {
     // 전역 에이전트를 사용하지 않음으로써 컨테이너 중첩 방지
    agent none 

    environment {
        REACT_APP_VERSION = "1.0.$BUILD_ID"
        APP_NAME = 'myjenkinsapp'
        AWS_DEFAULT_REGION = 'ap-northeast-2'
        AWS_ECS_CLUSTER = 'productive-bee-99erhn'
        AWS_ECS_SERVICE_PROD = 'LearnJenkinsApp-Service-Prod'
        AWS_ECS_TD_PROD = 'learn-jenkins-app-prod'
    }

    stages {
        stage('Build') {
            agent {
                docker { 
                    image 'mcr.microsoft.com/playwright:v1.39.0-jammy' 
                    reuseNode true
                }
            }
            steps {
                sh '''
                    echo '빌드 시작..'
                    node --version
                    npm --version
                    npm ci
                    npm run build
                '''
            }
        }

        // 빌드 이후에 수행
        stage('Build Docker image') {
            agent {
                docker { 
                    image 'amazon/aws-cli'
                    reuseNode true
                    args "-u root --entrypoint='' -v /var/run/docker.sock:/var/run/docker.sock"
                }
            }
            steps {
                sh '''
                    yum install -y docker
                    docker build -t $APP_NAME:$REACT_APP_VERSION .
                '''
            }
        }
    }
  
}
