pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Dependencies') {
            steps {
                sh 'mvn install'
            }
        }

        stage('Execute Tests') {
            steps {
                sh 'mvn verify -Dcucumber.options="--tags=$env.TAGS"'
            }
        }

        stage('Send Report') {
            steps {
                script {
                    def reportDir = 'target/cucumber-html-reports'
                    def reportFile = "${reportDir}/${env.BUILD_ID}/cucumber-report.html"

                    def isBuildSuccess = sh(
                        script: 'return $?',
                        returnStdout: true).trim() == '0'

                    if (isBuildSuccess) {
                        println "Tests successful"
                        emaildata = [
                            to: '35test42@gmail.com',
                            subject: "Cucumber Test Report - Successful (${env.BUILD_NUMBER})",
                            body: "Hello,\n\nCucumber tests successful completed. Click the report link: \nhttps://cucumber.io/docs/cucumber/reporting/(${reportFile})\n\nSaygılarımızla,\nJenkins",
                            charset: 'UTF-8'
                        ]
                        sendMail emaildata
                    } else {
                        println "Tests Failed"
                        emaildata = [
                            to: '335test42.gmail.com',
                            subject: "Cucumber Test Report - Failed (${env.BUILD_NUMBER})",
                            body: "Hello,\n\nCucumber tests ended in failure. Click the report link: \nhttps://cucumber.io/docs/cucumber/reporting/(${reportFile})\n\nSaygılarımızla,\nJenkins",
                            charset: 'UTF-8'
                        ]
                        sendMail emaildata
                    }
                }
            }
        }
    }
}