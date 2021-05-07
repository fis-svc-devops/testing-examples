pipeline {
    agent any
    stages {
        
        stage('CI-Test') {
             stages {
                stage("Staging-Deploy1") {
                    steps {
                        snDevOpsStep()
                        snDevOpsChange()
                        echo 'test'
                    }
                }
                stage("Send-Report1") {
                    stages {
                        stage("Alert-If-Issues1") {
                            steps {
                                //withSonarQubeEnv('LocalSonar') {
				//	sh '/Applications/SonarScanner/bin/sonar-scanner -Dproject.settings=/Applications/sonar-scanner.properties'
					//sh 'env'
              			//}
                                snDevOpsStep()
                                echo 'test'
                            }
                        }
                        stage("Conclude1") {
                            steps {
                                snDevOpsStep()
                                echo 'test'
                            }
                        }
                    }
                }
            }
        }
        
        stage('CI') {
            steps {
                //snDevOpsStep()
                echo 'test'
            }
        }
        stage('UAT deploy') {
            stages {
                stage("Staging-Deploy") {
                    steps {
                        snDevOpsStep()
                        echo 'test'
                    }
                }
                stage("Send-Report") {
                    stages {
                        stage("Alert-If-Issues") {
                            steps {
                                snDevOpsStep()
                                snDevOpsChange()
                                echo 'test'
                            }
                        }
                        stage("Conclude") {
                            steps {
                                snDevOpsStep()
                                echo 'test'
                            }
                        }
                    }
                }
            }
        }
        stage('UAT test') {
            parallel {
                stage('UAT test test1') {
                    steps {
                         echo 'test'
                    }
                    post {
                        success {
                            echo 'test'
                        }
                    }
                }
                stage('UAT static code test') {
                    steps {
                        snDevOpsStep()
                        snDevOpsChange()
                            echo 'test'
                    }
                }
            }
        }
        
        stage('Pre Prod') {
           stages {
            stage('Pre_Prod_Sub1') {
                steps {
                    echo 'test'
                }
             }
             stage('Pre_Prod_Sub2') {
                steps{
                    echo 'test'
                }
             }
            }
        }
         
        stage('PROD') {
           stages{
             stage('sub-prod') {
               steps {
                   snDevOpsStep()
                    snDevOpsChange()
                }
             }
           }
        }
	    
	    
	stage('PROD1') {
           stages{
             stage('sub-prod') {
               steps {
                   echo 'test'
                }
             }
           }
        }
    }
}
