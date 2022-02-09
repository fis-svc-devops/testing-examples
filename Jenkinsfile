pipeline {

    options {
        buildDiscarder logRotator(artifactDaysToKeepStr: '30', artifactNumToKeepStr: '5', daysToKeepStr: '30', numToKeepStr: '5')
        disableConcurrentBuilds()
    }

    
    //Test US517587: [This is a tracked rally record] 28/7/22
    agent any
    
    stages {
        stage('Checkout') {
            steps {
                script {
                    sleep 5
                }
            }
        }
        stage('Build with unit tests') {
            steps {
                script {
                    stageName = 'Build with unit tests'
                }
            }
        }
        stage('Publish artifact to ServiceNow') {
    
            steps {
                script {
                    stageName = 'Publish artifact to ServiceNow'
                    def applicationJar = "application-springboot-1.0.0.jar";
                    def repoPath = "access-to-apm/application";
                    def packageName = "applicationpackage";
                    snDevOpsArtifact(artifactsPayload: """{"artifacts": [{"name": "${applicationJar}", "version": "2.${env.BUILD_NUMBER}.0","semanticVersion": "1.0.0","repositoryName": "${repoPath}"}], "branchName": "test"}""")
                    snDevOpsPackage(name: "${packageName}", artifactsPayload: """{"artifacts": [{"name": "${applicationJar}", "version": "2.${env.BUILD_NUMBER}.0","semanticVersion": "1.0.0","repositoryName": "${repoPath}"}], "branchName": "test"}""")
                }
            }
        } 
        stage('Verify code formatting') {
            steps {
                script {
                    stageName = 'Verify code formatting';
                }
            }
        }

        stage("Acceptance and other test phases") {
           
            steps {
                script {
                        stageName = 'Acceptance and other test phases'
                }
            }
        }

        stage('Publish PIT report') {
            steps {
                script {
                    stageName = 'PIT report'
                }   
            }
        }
        stage('APP: SonarQube') {
            steps {
                script {
                    stageName = 'SonarQube analysis'
                }
            }
        }
        stage('SonarQube Report Upload to ServiceNow'){
           
            steps {
                script {
                    try {
                        stageName = 'SonarQube Report Upload to ServiceNow'
                        //application name in sonar
                        // def applicationName = 'gppaas-app';
                        // //get report
                        // sh """curl -X GET --header 'Accept: application/json' 'https://sonarqube.luigi.worldpay.io/api/issues/search?projects=com.worldpay.gpp%3A${applicationName}&resolved=false' > sonarqube.json;"""
                        // //send report base url

                        // // escape all spaces
                        // def stageNameQ = stageName.replaceAll(' ', '%20')
                        // // escape the branch name
                        // def branchNameQ= env.BRANCH_NAME.replace('/','%2F')
                        // // pipeline name
                        // def pipelineNameQ= 'gppaas%2Fgppaas-app'
                        // //send the report
                        // sh """curl "${baseUrl}/api/x_radi_code_qual/devops_code_quality/tool/event/sonarqube?&pipelineName=${pipelineNameQ}&buildNumber=${env.BUILD_NUMBER}&stageName=${stageNameQ}&branchName=${branchNameQ}" -H "Content-Type: application/json" --data @sonarqube.json;"""
                    } catch (e) {
                    }
                }
            }
        }
        stage('APP: Checkmarx') {
            steps {
                script {
                        stageName = 'CheckMarx'
                }
            }
        }
        stage('Checkmarx Report Upload to ServiceNow'){
 
            steps {
                script {
                     try {
                            // get bearer tokenp
                            // sh "echo Upload checkmarx report"
                            // sh "curl -X POST https://worldpay.checkmarx.net/cxrestapi/auth/identity/connect/token -H 'Content-type: application/x-www-form-urlencoded' --data-urlencode 'client_secret=014DF517-39D1-4453-B7B3-9930C563627C' --data-urlencode 'username=svc_ServiceNow-DevOps_reviewer' --data-urlencode 'password=Asnlh@sDh\$32GGH' --data-urlencode 'grant_type=password' --data-urlencode 'scope=sast_rest_api' --data-urlencode 'client_id=resource_owner_client' > token.json"
                            // def props = readJSON file: 'token.json'
                            // def token = props.access_token
                            // // get report
                            // sh """curl -X GET --header 'Accept: application/json' --header 'Authorization: Bearer $token' 'https://worldpay.checkmarx.net/cxrestapi/sast/scans?last=1&scanStatus=Finished&projectId=4612' > checkmarx.json;"""
                            // //s end report
                        } catch (e) {
                        }
                }
            }
        }
        stage('OWASP') {
            steps {
                script {
                    stageName = 'OWASP'
                }
            }
        }
        stage('Dependency Check') {
            steps {
                script {
                    stageName = 'Dependency check'
                }
            }
        }

        stage('Deployment to test environment') {
            steps {
                script{
                    stageName = 'Deployment to test environment'
                }
            }
        }

        stage('Run Pact Provider Tests') {
            steps {
                script {
                    stageName = 'Run Pact Provider Tests'
                }
            }
        }

        stage("Acceptance and other tests in GKOP") {
            steps {
                script {
                       stageName = 'Acceptance and other tests in GKOP'
                }
            }
        }
      
        stage('Deployment to production'){
            steps {
                script{
                    stageName = 'Deployment to production'
                    snDevOpsChange()
                }
            }
        }

        stage('APP: Publish artifact to Nexus') {
            when {
                branch 'master'
            }
            steps {
                script {
                    stageName = 'APP: Publish artifact to Nexus'
                    echo "Run maven deploy"
                }
            }
        }

        stage('Tag release version') {
            steps {
                script {
                    stageName = 'Tag release version'
                   
                }
            }
        }
        stage('Increment version') {
            when {
                branch 'master'
            }
            steps {
                script {
                    stageName = 'Increment version'
                }
            }
        }
        stage('Push changes to GIT') {
            when {
                branch 'master'
            }
            steps {
                script {
                    stageName = 'Push changes to GIT'
                }
            }
        }
    }
    post {
        failure {
            echo 'Build failure!!!'
            script {
                sleep 3
            }
        }
    }
}
