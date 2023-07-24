// 所有命令都放入pipeline
pipeline {
    agent any
    environment {
        harborUser = 'harbor100'
        harborAddr = '207.148.19.166:80'
        harborPasswd = 'Admin321'
        harborRepo = 'repo'
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
                sh '''docker login -u ${harborUser} -p ${harborPasswd} ${harborAddr}
                    docker tag ${JOB_NAME}:v1.1 ${harborAddr}/${harborRepo}/${JOB_NAME}:v1.1
                    docker push ${harborAddr}/${harborRepo}/${JOB_NAME}:v1.1'''
            }
        }
        stage('服务器运行镜像') {
            steps {
                echo '服务器运行镜像 -- SUCCESS'
            }
        }
    }
}
