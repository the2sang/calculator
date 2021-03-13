pipeline {
    agent any
    stages {
        stage("Compile") {
            steps {
                sh "./gradlew compileJava"
            }
        }
        stage("Unit test") {
            steps {
                sh "./gradlew test"
            }
        }
        stage("Code coverage") {
            steps {
                sh "./gradlew jacocoTestReport"
                publishHTML (target: [
                    reportDir: 'build/reports/tests/test',
                    reportFiles: 'index.html',
                    reportName: "Jacoco Report"
                ])
                sh "./gradlew jacocoTestCoverageVerification"
            }
        }
        stage("Package") {
            steps {
                sh "./gradlew build"
            }
        }
        stage("Docker build") {
            steps {
                sh "docker build -t the2sang/calculator ."
            }
        }
        stage("Docker push") {
            sh "docker push the2sang/calculator"
        }
        stage("Deploy to staging") {
            sh "docker run -d --rm -p 8765:8080 --name calculator the2sang/calculator"
        }
    }
}