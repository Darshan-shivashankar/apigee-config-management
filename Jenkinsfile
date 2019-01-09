#!groovy
pipeline {
    agent any
    tools {
        maven 'Maven 3.6.0'
    }
    stages {
        stage ('Initialize') {
            steps {
                sh '''
                    echo "PATH = ${PATH}"
                    echo "M2_HOME = ${M2_HOME}"
                '''
            }
        }

        stage ('Clean') {
            steps {
                echo 'This is a happy path demo for creating package'
                echo "Path to workspace Directory :::: ${env.WORKSPACE}"
                echo "Print JOB Name :::: ${env.JOB_NAME}"
                sh """
                mvn clean -X -f ${env.WORKSPACE}/pom.xml
                """
            }
        }

        stage ('Deploy-Sandbox') {
            steps {
                echo 'This is a happy path demo for creating package'
                echo "Path to workspace Directory :::: ${env.WORKSPACE}"
                echo "Print JOB Name :::: ${env.JOB_NAME}"
                sh """
                mvn install -X -f ${env.WORKSPACE}/pom.xml -Ptest -Dapigee.config.options=update
                """
            }
        }

        stage ('Deploy-Production') {
            steps {
                echo 'This is a happy path demo for proxy deploy'
                echo "Path to workspace Directory :::: ${env.WORKSPACE}"
                echo "Print JOB Name :::: ${env.JOB_NAME}"
                sh """
                mvn install -X -f ${env.WORKSPACE}/pom.xml -Pprod -Dapigee.config.options=update
                """
            }
        }
    }
}
