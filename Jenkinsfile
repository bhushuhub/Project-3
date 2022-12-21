pipeline{
    agent{
        node{
            label 'qa'
            customWorkspace '/mnt/project'
        }
    }
    tools{
        maven 'apache-maven-3.8.6'
        jdk 'jdk'
    }
    stages{
        stage ('cleaning local maven repo of slave-1'){
            steps{
                sh "sudo rm -rf /root/.m2/repository"
            }
        }
        stage ('clone repo on slave-1'){
            steps{
                sh "sudo rm -rf /mnt/project/webapp"
                sh "sudo git clone 'https://github.com/bhushuhub/webapp.git'"
            }
        }
        stage ('make build on slave-1'){
            agent{
                node{
                    label 'qa'
                    customWorkspace '/mnt/project/webapp'
                }
            }
            steps{
                sh "mvn clean install"
            }
        }
        stage ('deploy on slave-1 tomcat'){
            agent{
                node{
                    label 'qa'
                    customWorkspace '/mnt/project/webapp/target'
                }
            }
            steps{
                sh "sudo chmod 777 -R WebApp.war"
                sh "sudo cp -r WebApp.war /mnt/server/apache-tomcat-9.0.70/webapps"
            }
        }
        stage ('cleaning local maven repo on slave-2'){
            agent{
                node{
                    label 'dev'
                    customWorkspace '/mnt/project'
                }
            }
            steps{
                sh "sudo rm -rf /root/.m2/repository"
            }
        }
        stage ('clone repo on slave-2'){
            agent{
                node{
                    label 'dev'
                    customWorkspace '/mnt/project'
                }
            }
            steps{
                sh "sudo rm -rf /mnt/project/webapp"
                sh "sudo git clone 'https://github.com/bhushuhub/webapp.git'"
            }
        }
        stage ('make build on slave-2'){
            agent{
                node{
                    label 'dev'
                    customWorkspace '/mnt/project/webapp'
                }
            }
            steps{
                sh "mvn clean install"
            }
        }
        stage ('deploy on slave-2 tomcat'){
            agent{
                node{
                    label 'dev'
                    customWorkspace '/mnt/project/webapp/target'
                }
            }
            steps{
                sh "sudo chmod 777 -R WebApp.war"
                sh "sudo cp -r WebApp.war /mnt/server/apache-tomcat-9.0.70/webapps"
            }
        }
    }
}
