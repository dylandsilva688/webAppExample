pipeline {
    agent any

    stages {
        stage('Matrix Build') {
            matrix {
                axes {
                    axis {
                        name 'BROWSER'
                        values 'Chrome', 'Firefox'
                    }
                    axis {
                        name 'OS'
                        values 'Ubuntu', 'Windows'
                    }
                }

                stages {
                    stage('Checkout') {
                        steps {
                            git credentialsId: 'Github_username_password',
                                url: 'https://github.com/dylandsilva688/webAppExample.git'
                        }
                    }

                    stage('Build WAR') {
                        steps {
                            bat '"C:\\Program Files\\apache-maven-3.9.5-bin\\apache-maven-3.9.5\\bin\\mvn.cmd" clean package'
                        }
                    }

                    stage('Deploy to Tomcat') {
                        steps {
                            deploy adapters: [tomcat9(
                              alternativeDeploymentContext: '', 
                              credentialsId: '735a5106-4896-4c24-868a-56aaf0cd291c', 
                              path: '', 
                              url: 'http://localhost:8181'
                            )
                          ], 
                          contextPath: 'webAppExample-${BROWSER}-${OS}', 
                          war: '**/*.war
                        }
                    }
                }
            }
        }
    }

    post {
        success {
            echo '✅ Build and deploy completed successfully!'
        }
        failure {
            echo '❌ Build or deploy failed.'
        }
    }
}

