pipeline { 

  // This pipeline requires the following plugins: 

  // * Git: https://plugins.jenkins.io/git/ 

  // * Workflow Aggregator: https://plugins.jenkins.io/workflow-aggregator/ 

  // * MSTest: https://plugins.jenkins.io/mstest/ 

  agent 'any' 

  stages { 

    stage('Environment') { 

      steps { 

          echo "PATH = ${PATH}" 

      } 

    } 

    stage('Checkout') { 

      steps { 

  

        script { 

            checkout([$class: 'GitSCM', branches: [[name: '*/envia']], userRemoteConfigs: [[url: 'https://github.com/leviczne/API-CRUD.git']]]) 

        } 

      } 

    } 

    stage('Dependencies') { 

      steps { 

          script{ 

            if (isUnix()){  

                sh(script: 'dotnet restore') 

                 

            } 

            else { 

                bat (script: 'dotnet restore') 

                 

            } 

          } 

      } 

    } 

    stage('Build') { 

      steps { 

          script{ 

              if (isUnix()){ 

                sh(script: 'dotnet build --configuration Release', returnStdout: true) 

              } 

              else{ 

                  bat(script: 'dotnet build --configuration Release', returnStdout: true) 

              } 

          } 

      } 

    } 

    stage('Test') { 

      steps { 

          script{ 

               if (isUnix()){ 

                sh(script: 'dotnet test -l:trx')   

               } 

               else { 

                   bat (script: 'dotnet test -l:trx')   

               } 

          } 

      } 

    } 

  } 

  post { 

    always { 

      mstest(testResultsFile: '**/*.trx', failOnError: false, keepLongStdio: true) 

    } 

  } 

} 