def tests_failed = false

pipeline {
    agent any
    tools {
        maven 'mvn3.6.3'
        jdk 'jdk8'
    }

    stages {
        stage('init') {
            steps {
                sh '''
                echo "PATH=${PATH}"
                echo "M2_HOME=${M2_HOME}"
                '''
            }
        }
        stage('build') {
            steps {
                sh 'mvn clean install -Dmaven.test.skip=true'
            }
        }
        stage('test') {
            steps {
                script {
                    try {
                        throw new Exception("Arbitrary test step failure")
                    } catch (ex) {
                        tests_failed = true
                        throw ex
                    }
                }
            }
        }
        stage('optional retest') {
            steps {
                script {
                    if (tests_failed) {
                        sh 'mvn test'
                    }
                }
            }

        }
    }
}