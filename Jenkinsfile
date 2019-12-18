pipeline {
    agent any
    tools {
        maven 'maven3'
    }
    stages {
        stage('Build') {
            steps {
                sh "mvn -DskipTests clean package"
            }
            post {
                success {
                    archiveArtifacts 'target/*.jar'
                }
            }
        }
        stage('Check&Test') {
            parallel {
                stage('Test') {
                    steps {
                        sh "mvn test -Dmaven.test.failure.ignore=true"
                    }
                    post {
                        success {
                            junit '**/target/surefire-reports/TEST-*.xml'
                        }
                    }
                }
                stage('PMD') {
                    steps {
                        sh "mvn pmd:pmd"
                    }
                    post {
                        always {
                            recordIssues(tools: [pmdParser()])
                        }
                    }
                }
            }
        }
        stage('Deliver') {
            steps {
                sh 'echo here is deliver' 
            }
        }
    }
}
