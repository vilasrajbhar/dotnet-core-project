pipeline {
    agent {
        label 'windows-agent'
    }    
    stages {
        stage('Build') {
            steps('Build Class library') {	
               bat 'C:\NuGet\nuget.exe restore AspNetCoreWebApplication.sln -Verbosity Detailed -NonInteractive'
               bat 'msbuild build AspNetCoreWebApplication.sln /t:Rebuild /p:Configuration=Release'                 
            }
        }
         stage('UnitTests') {
            steps {                
              	bat 'dotnet test AspNetCoreWebApplication.sln --logger "trx;LogFileName=${WORKSPACE}/unit_tests.xml"' 
            }
        }
    }
}
