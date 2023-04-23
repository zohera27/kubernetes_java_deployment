@Library('Jenkins_Shared_Library') _

pipeline{
    
    agent any

    tools{

        jdk 'jdk8'
        maven 'maven'
    }
    /*
    parameters{
        
        choice(

            name: 'action',
            choices: ['Create', 'Delete'],
            description: 'Choose the Action Create/Delete'
        )

        string(

            name: 'ProductImage',
            description: 'Docker Image Build',
            defaultValue: 'productcatalogue'
        )

        string(

            name: 'ShopImage',
            description: 'Docker Image Build',
            defaultValue: 'shopfront'
        )

        string(

            name: 'StockImage',
            description: 'Docker Image Build',
            defaultValue: 'stockmanager'
        )

        string(

            name: 'ImageTag',
            description: 'Image Tag for Docker Build',
            defaultValue: 'v1'
        )        

        string(

            name: 'DockerHub',
            description: 'Docker Hub user Account',
            defaultValue: 'zohera27'
        )
        
    }
    */
    environment{

        JAVA8_HOME = "${tool 'JDK8'}"
    }

    stages{


        stage('Git Checkout') {

         when { expression { params.action == 'Create' } }    

            steps{

                script{

                    gitcheckout(

                        branch: 'main',
                        url: 'https://github.com/zohera27/kubernetes_java_deployment.git'

                    )

                }



            }

    


        }

        stage('JDK 8 stage') {

            sh '$JAVA8_HOME/bin/java -version'
            sh 'JAVA_HOME=$JAVA8_HOME ${MAVEN_HOME}/bin/mvn --version'
        }
        /*
        stage('Unit Test maven') {

         when { expression { params.action == 'Create' } }    

            steps{

                dir('productcatalogue') {

                    script{

                        maven.mvntest(mavenhome, toolchain)        

                    }
                }

                dir('shopfront') {

                    script{

                        maven.mvntest(mavenhome, toolchain)        

                    }
                }

                dir('stockmanager') {

                    script{

                        maven.mvntest(mavenhome, toolchain)        

                    }
                }
                
            }
        }

        stage('Integration Test maven') {

         when { expression { params.action == 'Create' } }    

            steps{

                dir('productcatalogue') {

                    script{

                        maven.mvnverify(mavenhome, toolchain)        

                    }
                }

                dir('shopfront') {

                    script{

                        maven.mvnverify(mavenhome, toolchain)        

                    }
                }

                dir('stockmanager') {

                    script{

                        maven.mvnverify(mavenhome, toolchain)        

                    }
                }
                
            }
        }

        stage('Static Code Analysis: Sonarqube'){

         when { expression { params.action == 'Create' }  }

            steps{

                dir('productcatalogue') {

                    script{

                        def SonarQubecredentialId = 'sonar-api'
                        sonarqube.staticcode(SonarQubecredentialId, mavenhome, toolchain)
                    }
                }

                dir('shopfront') {

                    script{

                        def SonarQubecredentialId = 'sonar-api'
                        sonarqube.staticcode(SonarQubecredentialId, mavenhome, toolchain)
                    }
                }

                dir('stockmanager') {

                    script{

                        def SonarQubecredentialId = 'sonar-api'
                        sonarqube.staticcode(SonarQubecredentialId, mavenhome, toolchain)
                    }
                }

            }
        }

        stage('QG Satus Check: Sonarqube'){

         when { expression { params.action == 'Create' }  }

            steps{

                dir('productcatalogue') {

                    script{

                        def SonarQubecredentialId = 'sonar-api'
                        sonarqube.qualitygate(SonarQubecredentialId)
                    }
                }

                dir('shopfront') {

                    script{

                        def SonarQubecredentialId = 'sonar-api'
                        sonarqube.qualitygate(SonarQubecredentialId)
                    }
                }

                dir('stockmanager') {

                    script{

                        def SonarQubecredentialId = 'sonar-api'
                        sonarqube.qualitygate(SonarQubecredentialId)
                    }
                }

            }
        }

        stage('Maven Build : maven'){

         when { expression { params.action == 'Create' }  }

            steps{

                dir('productcatalogue') {

                    script{

                        maven.mvnbuild(mavenhome, toolchain)
                    }
                }

                dir('shopfront') {

                    script{

                        maven.mvnbuild(mavenhome, toolchain)
                    }
                }

                dir('stockmanager') {

                    script{

                        maven.mvnbuild(mavenhome, toolchain)
                    }
                }

            }
        }

        stage('Docker Image Build') {

         when { expression { params.action == 'Create' }  }    

            steps{

                dir('productcatalogue') {

                    script {

                        docker.build("${params.ProductImage}", "${params.ImageTag}", "${params.DockerHub}")
                    }
                }

                dir('shopfront') {

                    script {

                        docker.build("${params.ShopImage}", "${params.ImageTag}", "${params.DockerHub}")

                    }
                }

                dir('stockmanager') {

                    script {

                        docker.build("${params.StockImage}", "${params.ImageTag}", "${params.DockerHub}")

                    }
                }
            }
        }

        stage('Docker Image Scan: trivy') {

         when { expression { params.action == 'Create' }  }

            steps {

                dir('productcatalogue'){

                    script {

                        docker.ImageScan("${params.ProductImage}", "${params.ImageTag}", "${params.DockerHub}")
                    }
                }

                dir('shopfront') {

                    script {

                        docker.ImageScan("${params.ShopImage}", "${params.ImageTag}", "${params.DockerHub}")
                    }
                }

                dir ('stockmanager') {

                    script {

                        docker.ImageScan("${params.StockImage}", "${params.ImageTag}", "${params.DockerHub}")
                    }
                }
            }    

        }

        stage('Docker Image Push : DockerHub') {

         when { expression { params.action == 'Create' }  }    

            steps {

                dir('productcatalogue') {

                    script {

                        docker.PushImage("${params.ProductImage}", "${params.ImageTag}", "${params.DockerHub}")
                    }
                }

                dir('shopfront') {

                    script {

                        docker.PushImage("${params.ShopImage}", "${params.ImageTag}", "${params.DockerHub}")
                    }
                }

                dir('stockmanager') {

                    script {

                        docker.PushImage("${params.StockImage}", "${params.ImageTag}", "${params.DockerHub}")
                    }
                }
            }


        }

        stage('Docker Image Cleanup') {

         when { expression { params.action == 'Create' } }

            steps {

                script{

                    docker.clean("${params.ProductImage}", "${params.ImageTag}", "${params.DockerHub}")
                    docker.clean("${params.ShopImage}", "${params.ImageTag}", "${params.DockerHub}")
                    docker.clean("${params.StockImage}", "${params.ImageTag}", "${params.DockerHub}")                    
                }
            }

        }
        */
    }   
}    