@Library('Jenkins_Shared_Library') _


pipeline{

    agent any

    stages{

        stage('GIT Checkout'){

            steps{

                script{

                    gitcheckout.git(

                        branch:'main',
                        url:'https://github.com/zohera27/kubernetes_java_deployment.git'
                    )
                }
            }
        }
    }
}