pipeline {
    agent {
        label 'windows-agent'
    }    
    stages {
        stage('Clean') {
            steps {
                echo 'Cleaning up the workspace started.'
                deleteDir()
                echo 'Workspace clean succesfully.'
            }
        }
        stage('Checkout Again') {
            steps {
                // Checkout the source code again after cleaning the workspace
                git 'https://github.com/vilasrajbhar/dotnet-core-project.git'
            }
        }
        stage('Build') {
            steps('Build Class library') {	
                echo 'Nuget Restore Starts!'
                bat 'C:/NuGet/nuget.exe restore AspNetCoreWebApplication.sln -Verbosity Detailed -NonInteractive'
                echo 'Nuget Restore Completed.'
                echo 'Solution Build Starts!'
                bat 'msbuild AspNetCoreWebApplication.sln -t:restore,build -p:RestorePackagesConfig=true /p:configuration="Release"'  
                echo 'Solution Build Completed.'
            }
        }
         stage('UnitTests') {
            steps {  
                echo 'Dotnet Test Starts!'
              	bat 'dotnet test AspNetCoreWebApplication.sln --logger "trx;LogFileName=${WORKSPACE}/unit_tests.xml"' 
                echo 'Dotnet Test Completed.'
            }
        }
        stage('Publish') {
            steps {  
                echo 'Dotnet Publish Starts!'
              	bat 'dotnet publish -c Release --output artifacts AspNetCoreWebApplication.sln' 
                echo 'Dotnet Publish Completed.'
            }
        }
        stage('Archive Artifacts') {
            steps { 
                echo 'Zipping Artifacts Starts!'
                bat 'powershell Compress-Archive -Path .\\artifacts -DestinationPath .\\artifacts.zip' 
                echo 'Zipping Artifacts Completed.'
            }
        }
    }
}
