pipeline {
    agent any

    stages {

        stage("run frontend") {
            steps {
                echo 'building frontend for the application ...'
                nodejs('Node-18.3.0') {
                    sh 'npm install'
                    sh 'npm build'
                }
            }
        }
        stage("test") {

            steps {
                echo 'testing frontend ...'
            }
        }
        stage("deploy") {
            steps {
                echo 'looks awesome ! Deploying ...'
            }
        }
    }   
}
