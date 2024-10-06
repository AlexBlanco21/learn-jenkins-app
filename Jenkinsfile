pipeline {
    agent any

    environment {

        NETLIFY_SITE_ID = '11b9a2ea-fb77-4de6-8a6b-40ef5f9021c1' //Variable should not change name
        NETLIFY_AUTH_TOKEN = credentials('netlify-token')

    }

    stages {
        stage('Build') {
            agent{
                docker{
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    echo 'Building the Node Js application'
                    ls -la
                    node --version
                    npm --version
                    npm ci
                    npm run build
                    ls -la
                '''
            }
        }

        stage('Test') {
            agent{
                docker{
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    echo 'Testing the app'
                    test -f build/index.html
                    npm test
                '''
            }
        }

        stage('Deploy') {
            agent{
                docker{
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                //Deployment is using the variables declared at the beginning
                sh '''
                    npm install netlify-cli 
                    node_modules/.bin/netlify --version
                    echo "Deploying to production. Site ID: $NETLIFY_SITE_ID"
                    node_modules/.bin/netlify status
                    node_modules/.bin/netlify deploy --dir=build --prod

                '''
            }
        }



    }

    post {
        always {
            junit 'test-results/junit.xml' //Installing netlify
        }
    }


}