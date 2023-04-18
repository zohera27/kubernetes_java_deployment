@Library('Jenkins_Shared_Library') _

def mavenhome = '/opt/apache-maven-3.9.1/bin'
def toolchain = '/opt/apache-maven-3.9.1/conf/toolchains.xml'

pipeline{

    agent any

    stages{

        stage('GIT CheckOut'){

            steps{

                script{

                    gitcheckout.git([

                        branch:'main',
                        url:'https://github.com/zohera27/kubernetes_java_deployment.git'
                    ])                     
                    
                }
            }
        }

        stage('Maven Test'){

            steps{

                script{

                    maven.mvntest(mavenhome, toolchain)
                }
            }
        }
    }
}