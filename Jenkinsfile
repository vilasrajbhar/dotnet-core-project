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
                dotnet publish -c Release -o output
            }
        }
        stage('Publish') {
            steps {  
                echo 'Dotnet Publish Starts!'
              	bat 'dotnet publish -c Release -r win-x64 --output artifacts AspNetCoreWebApplication.sln' 
                echo 'Dotnet Publish Completed.'
            }
        }
    }
}
