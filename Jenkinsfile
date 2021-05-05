pipeline {
    agent any

    stages {
        stage('preparation') {
            steps {
                git 'https://github.com/AyaHamedd/Booster_CI_CD_Project.git'
            }
        }
        stage('CI') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker', passwordVariable: 'mypass', usernameVariable: 'myname')]) {
                sh """
                docker build . -t ayahamedd/myimage:1.0
                docker login --username ${myname} --password ${mypass}
                docker push ayahamedd/myimage:1.0
                """
                }
            }
        }
        stage('CD') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker', passwordVariable: 'password', usernameVariable: 'username')]) {
                sh """
                    docker run -d -p 3001:8000 ayahamedd/myimage:1.0
                """
                }
            post {
            	success {
            	slackSend (color: "#008000",message: "pipeline is done")
            	}
            	}
            }
        }
    }
}

