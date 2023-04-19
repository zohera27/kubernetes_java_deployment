@Library('Jenkins_Shared_Library') _

def mavenhome = '/opt/apache-maven-3.9.1/bin'
def toolchain = '/opt/apache-maven-3.9.1/conf/toolchains.xml'
def Sonarqubecredentials='sonar-api'

pipeline{

    agent any

    stages{

        stage('GIT CheckOut'){

            steps{

                script{

                    git.gitcheckout(

                        branch:'main',
                        url:'https://github.com/zohera27/kubernetes_java_deployment.git'
                    )
                }
            }
        }
        stage('Maven Test') {

            steps{

                dir('productcatalogue') {

                    script {

                        mvn.mvntest(mavenhome, toolchain)
                    }
                }
                dir('shopfront') {

                    script {

                        mvn.mvntest(mavenhome, toolchain)
                    }
                }
                dir('stockmanager') {

                    script {

                        mvn.mvntest(mavenhome, toolchain)
                    }
                }
            }
        }
        stage('Unit Integration Test') {

            steps{

                dir('productcatalogue') {

                    script {

                        mvn.mvnverify(mavenhome, toolchain)
                    }
                }
                dir('shopfront') {

                    script {

                        mvn.mvnverify(mavenhome, toolchain)
                    }
                }
                dir('stockmanager') {

                    script {

                        mvn.mvnverify(mavenhome, toolchain)
                    }
                }
            }
        }
        stage('SonarQube:Static Code Analysis'){

            steps{

                dir('productcatalogue'){

                    script{

                        sonar.codeanalysis(Sonarqubecredentials, mavenhome, toolchain)
                    }
                }
                dir('shopfront'){

                    script{

                        sonar.codeanalysis(Sonarqubecredentials, mavenhome, toolchain)
                    }
                }
                dir('stockmanager'){

                    script{

                        sonar.codeanalysis(Sonarqubecredentials, mavenhome, toolchain)
                    }
                }
            }
        }
        stage('SonarQube:Quality Gate'){

            steps{

                dir('productcatalogue'){

                    script{

                        sonar.qualitygate(Sonarqubecredentials)
                    }
                }
                dir('shopfront'){

                    script{

                        sonar.qualitygate(Sonarqubecredentials)
                    }
                }
                dir('stockmanager'){

                    script{

                        sonar.qualitygate(Sonarqubecredentials)
                    }
                }
            }
        }
    }
}