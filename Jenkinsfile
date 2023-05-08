@Library('Jenkins_Shared_Library') _

pipeline{
    
    agent any
    /*
    tools{

        jdk 'JDK8'
        jdk 'JDK11'
        maven 'maven'
    }
    */
    
    environment{

        JAVA8_HOME = "${tool 'JAVA8'}"
        JAVA11_HOME = "${tool 'JAVA11'}"
        MAVEN_HOME = "${tool 'MAVEN'}"
    }
    
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
            defaultValue: 'latest'
        )        

        string(

            name: 'DockerHub',
            description: 'Docker Hub user Account',
            defaultValue: 'zohera27'
        )
        
    }
    

    stages{

        
        stage('Git Checkout') {

         when { expression { params.action == 'Create' } }    

            steps{

                script{

                    git.gitcheckout(

                        branch: 'main',
                        url: 'https://github.com/zohera27/kubernetes_java_deployment.git'

                    )

                }



            }

    


        }
        
        /*
        stage('JDK 8 stage') {

            steps {

                script {

                    sh '$JAVA8_HOME/bin/java -version'
                    sh 'JAVA_HOME=$JAVA8_HOME ${MAVEN_HOME}/bin/mvn --version'
                }            
            }
        }
        */
               
        stage('Unit Test maven') {

         when { expression { params.action == 'Create' } }    

            steps{

                dir('productcatalogue') {

                    script{

                        mvn.mvntest(JAVA8_HOME)        

                    }
                }
                
                dir('shopfront') {

                    script{

                        mvn.mvntest(JAVA8_HOME)        

                    }
                }

                dir('stockmanager') {

                    script{

                        mvn.mvntest(JAVA8_HOME)        

                    }
                }
                
            }
        }
        
        stage('Integration Test maven') {

         when { expression { params.action == 'Create' } }    

            steps{

                dir('productcatalogue') {

                    script{

                        mvn.mvnverify(JAVA8_HOME)        

                    }
                }

                dir('shopfront') {

                    script{

                        mvn.mvnverify(JAVA8_HOME)        

                    }
                }

                dir('stockmanager') {

                    script{

                        mvn.mvnverify(JAVA8_HOME)        

                    }
                }
                
            }
        }
        
        /*        
        stage('Static Code Analysis: Sonarqube'){

         // when { expression { params.action == 'Create' }  }

            steps{

                dir('productcatalogue') {

                    script{

                        def SonarQubecredentialId = 'sonarqube'
                        sonar.codeanalysis(SonarQubecredentialId, JAVA11_HOME)
                    }
                }

                dir('shopfront') {

                    script{

                        def SonarQubecredentialId = 'sonarqube'
                        sonar.codeanalysis(SonarQubecredentialId, JAVA11_HOME)
                    }
                }

                dir('stockmanager') {

                    script{

                        def SonarQubecredentialId = 'sonarqube'
                        sonar.codeanalysis(SonarQubecredentialId, JAVA11_HOME)
                    }
                }

            }
        }
        
        stage('QG Satus Check: Sonarqube'){

         // when { expression { params.action == 'Create' }  }

            steps{

                dir('productcatalogue') {

                    script{

                        def SonarQubecredentialId = 'sonarqube'
                        sonar.qualitygate(SonarQubecredentialId)
                    }
                }

                dir('shopfront') {

                    script{

                        def SonarQubecredentialId = 'sonarqube'
                        sonar.qualitygate(SonarQubecredentialId)
                    }
                }

                dir('stockmanager') {

                    script{

                        def SonarQubecredentialId = 'sonarqube'
                        sonar.qualitygate(SonarQubecredentialId)
                    }
                }

            }
        }
        */
        
        stage('Maven Build : maven'){

         when { expression { params.action == 'Create' }  }

            steps{

                dir('productcatalogue') {

                    script{

                        mvn.mvnbuild(JAVA8_HOME)
                    }
                }

                dir('shopfront') {

                    script{

                        mvn.mvnbuild(JAVA8_HOME)
                    }
                }

                dir('stockmanager') {

                    script{

                        mvn.mvnbuild(JAVA8_HOME)
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
        
        /*
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
        */
        
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
        
        /*
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
              
        stage('Deploying App to kubernetes')
        {
            steps {

                dir('kubernetes'){

                    script{

                        kubernetesDeploy(configs: "shopfront-service.yaml", kubeconfigId: "jenkins")
                        kubernetesDeploy(configs: "productcatalogue-service.yaml", kubeconfigId: "jenkins")
                        kubernetesDeploy(configs: "stockmanager-service.yaml", kubeconfigId: "jenkins")
                    }
                }


            }
        }
        
    }   
}    