pipeline {
    agent any

    // Environment Variables
    environment {
        MAJOR = '1'
        MINOR = '0'
        //Orchestrator Services
        UIPATH_ORCH_URL = "https://cloud.uipath.com/"
        UIPATH_ORCH_LOGICAL_NAME = "vectoritc"
        UIPATH_ORCH_TENANT_NAME = "Prueba"
        UIPATH_ORCH_FOLDER_NAME = "Shared"

        UIPATH_INSTALLATION_PATH = "C:\\Users\\jvelazquez\\AppData\\Local\\Programs\\UiPath\\Studio"
    }

    stages {
        // Printing Basic Information
        stage('Preparing'){
            steps {
                echo "Jenkins Home ${env.JENKINS_HOME}"
                echo "Jenkins URL ${env.JENKINS_URL}"
                echo "Jenkins JOB Number ${env.BUILD_NUMBER}"
                echo "Jenkins JOB Name ${env.JOB_NAME}"
                echo "GitHub BranchName ${env.BRANCH_NAME}"
                checkout scm
            }
        }

        // stage('Analyze'){
        //   steps {
        //     echo "Analyzing..."
        //     bat  "\"${UIPATH_INSTALLATION_PATH}\\UiPath.Studio.CommandLine.exe\" analyze-file -p \"%cd%\\Main.xaml\""
        //   }

        // }

         // Build Stages
        stage('Build') {
            steps {
                echo "Building..with ${WORKSPACE}"
                bat  "\"${UIPATH_INSTALLATION_PATH}\\UiPath.Studio.CommandLine.exe\" publish -p \"%cd%\\project.json\" --target Custom -f \"%cd%\\result\""
            }
        }
        /*
         // Test Stages
        stage('Test') {
            steps {
                echo 'Testing..the workflow...'
            }
        }
        */

         // Deploy Stages
        stage('Deploy to UAT') {
            steps {
                echo "Deploying ${BRANCH_NAME} to UAT "
                // UiPathDeploy (
                //         packagePath: "result\\UiPath.Jenkins.CICD.Demo.1.0.1.nupkg",//"Output\\${env.BUILD_NUMBER}",
                //         orchestratorAddress: "${UIPATH_ORCH_URL}",
                //         orchestratorTenant: "${UIPATH_ORCH_TENANT_NAME}",
                //         folderName: "${UIPATH_ORCH_FOLDER_NAME}",
                //         environments: 'DEV',
                //         //credentials: [$class: 'UserPassAuthenticationEntry', credentialsId: 'APIUserKey']
                //         credentials: Token(accountName: "${UIPATH_ORCH_LOGICAL_NAME}", credentialsId: 'appSecretCloudCommunity'),


                // )


                UiPathDeploy (
                  credentials: 
                    ExternalApp(
                      accountForApp: 'vectoritc', 
                      applicationId: '9771851e-53b2-47b0-8708-e0e4ae370075', 
                      applicationScope: 'OR.Assets OR.BackgroundTasks OR.Execution OR.Folders OR.Jobs OR.Machines.Read OR.Robots.Read OR.Settings.Read OR.TestSetExecutions OR.TestSets OR.TestSetSchedules OR.Users.Read', 
                      applicationSecret: 'appSecretCloudCommunity', 
                      identityUrl: 'https://cloud.uipath.com/'), 
                  entryPointPaths: 'Main.xaml', 
                  environments: '', 
                  folderName: "${UIPATH_ORCH_FOLDER_NAME}", 
                  orchestratorAddress: "${UIPATH_ORCH_URL}", 
                  orchestratorTenant: "${UIPATH_ORCH_TENANT_NAME}", 
                  packagePath: "result\\UiPath.Jenkins.CICD.Demo.1.0.1.nupkg",//"Output\\${env.BUILD_NUMBER}",
                  traceLevel: 'None'
                )


            }
        }




         // Deploy to Production Step
        // stage('Deploy to Production') {
        //     steps {
        //         echo 'Deploy to Production'
        //         }
        //     }
                
    }


    // Options
    options {
        // Timeout for pipeline
        timeout(time:80, unit:'MINUTES')
        skipDefaultCheckout()
    }




    //

    post {
        success {
            echo 'Deployment has been completed!'
        }
        failure {
          echo "FAILED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.JOB_DISPLAY_URL})"
        }
        always {
            /* Clean workspace if success */
            cleanWs()
        }
    }


}