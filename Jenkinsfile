pipeline {
    agent {
        label 'windows-agent'
    }    
    stages {
        stage('Build') {
            steps('Build Class library') {	
                echo 'Build Starts!'
                bat 'C:/NuGet/nuget.exe restore AspNetCoreWebApplication.sln -Verbosity Detailed -NonInteractive'
                bat 'msbuild build AspNetCoreWebApplication.sln /t:Rebuild /p:Configuration=Release'  
                echo 'Build Ends'
            }
        }
         stage('UnitTests') {
            steps {                
              	bat 'dotnet test AspNetCoreWebApplication.sln --logger "trx;LogFileName=${WORKSPACE}/unit_tests.xml"' 
            }
        }
    }
}
