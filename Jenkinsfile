pipeline {
    agent {
        label 'windows-agent'
    }    
    stages {
        stage('Build') {
            steps('Build Class library') {	
               bat 'dotnet restore AspNetCoreWebApplication.sln'
               bat 'dotnet build AspNetCoreWebApplication.sln /t:Build /p:Configuration=Release'                             
            }
        }
         stage('UnitTests') {
            steps {                
              	bat 'dotnet test AspNetCoreWebApplication.sln --logger "trx;LogFileName=${WORKSPACE}/unit_tests.xml"' 
            }
        }
    }
}