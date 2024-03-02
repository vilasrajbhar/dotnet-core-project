pipeline {
    agent {
        label 'windows-agent'
    }    
    stages {
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
    }
}
