pipeline {
    agent {
        docker {
            image 'python:3.10'
            args '-u root'
        }
    }

    stages {
        stage('Install Dependencies') {
            steps {
                sh 'pip install -r requirements.txt'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'pytest test_app.py'
            }
        }

        stage('Deploy') {
            when {
                anyOf {
                    branch 'main'
                    branch pattern: "release/.*", comparator: "REGEXP"
                }
            }
            steps {
                echo "Simulating deploy from branch ${env.BRANCH_NAME}"
            }
        }
    }

    post {
        success {
            script {
                def payload = [
                    content: "✅ Build SUCCESS on `${env.BRANCH_NAME}`\n🔗URL: ${env.BUILD_URL}"
                ]
                httpRequest(
                    httpMode: 'POST',
                    contentType: 'APPLICATION_JSON',
                    requestBody: groovy.json.JsonOutput.toJson(payload),
                    url: 'https://discord.com/api/webhooks/1425105466324680786/K5A5sf0fG771qXEGSpKRAqrH2CiRMBLJOgwOCkOhE2UE_dkxWvHsfJnvCmDXRG2TlM7r'
                )
            }
        }

        failure {
            script {
                def payload = [
                    content: "❌ Build FAILED on `${env.BRANCH_NAME}`\n🔗URL: ${env.BUILD_URL}"
                ]
                httpRequest(
                    httpMode: 'POST',
                    contentType: 'APPLICATION_JSON',
                    requestBody: groovy.json.JsonOutput.toJson(payload),
                    url: 'https://discord.com/api/webhooks/1425105466324680786/K5A5sf0fG771qXEGSpKRAqrH2CiRMBLJOgwOCkOhE2UE_dkxWvHsfJnvCmDXRG2TlM7r'
                )
            }
        }
    }
}
