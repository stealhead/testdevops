// 所有命令都放入pipeline
pipeline {
    agent any
    environment {
        key = 'value'
    }
    stages {
        stage('拉取git仓库代码') {
            steps {
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/stealhead/testdevops.git']])
            }
        }
        stage('构建项目') {
            steps {
                sh '/var/jenkins_home/maven/bin/mvn clean package -DskipTests'
            }
        }
        stage('制作镜像') {
            steps {
                sh '''mv ./target/*.jar ./docker/
                        docker build -t ${JOB_NAME}:v1.1 ./docker/'''
            }
        }
        stage('自定义镜像推送到harbor') {
            steps {
                echo '自定义镜像推送到harbor -- SUCCESS'
            }
        }
        stage('服务器运行镜像') {
            steps {
                echo '服务器运行镜像 -- SUCCESS'
            }
        }
    }
}
