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
                docker build . -t AyaHamedd/myimage:1.0
                docker login --username ${myname} --password ${mypass}
                docker push AyaHamedd/myimage:1.0
                """
                }
            }
        }
        stage('CD') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker', passwordVariable: 'mypass', usernameVariable: 'myname')]) {
                sh """
                    docker run -d -p 3001:8000 AyaHamedd/myimage:1.0
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

