pipeline{
    agent any

    tools {
        maven 'local-maven'
    }

    parameters {
        string(name: 'tomcat_dev', defaultValue: '3.87.210.247', description: 'Staging Server')
        string(name: 'tomcat_prod', defaultValue: '3.85.193.45', description: 'Production Server')
    }
    stages{
        stage("Build"){
            steps{
                bat "mvn clean package"
            }
            post{
                always{
                    echo "========always========"
                }
                success{
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
                failure{
                    echo "========A execution failed========"
                }
            }
        }
        stage('Deployments'){
            parallel{
                stage('Deploy to staging'){
                    steps {
                        bat "winscp -i F:\\Study\\AWS\\myKeyPair.ppk **/target/*.war ubuntu@${params.tomcat_dev}:/var/lib/tomcat8/webapps/ROOT"
                    }
                }
                stage('Deploy to production'){
                    steps {
                        bat "winscp -i F:\\Study\\AWS\\myKeyPair.ppk **/target/*.war ubuntu@${params.tomcat_prod}:/var/lib/tomcat8/webapps/ROOT"
                    }
                }
            }
        }
    }
}
