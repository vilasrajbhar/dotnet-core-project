pipeline {
    agent {
        label 'windows-agent'
    }    
    stages {
        stage('Clean') {
            steps {
                echo 'Cleaning up the workspace started.'
                deleteDir()
                echo 'Workspace clean successfully.'
            }
        }
        stage('Checkout Again') {
            steps {
                // Checkout the source code again after cleaning the workspace
                git 'https://github.com/vilasrajbhar/dotnet-core-project.git'
            }
        }
        stage('Build') {
            steps {  
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
	
	// stage('Approval') {
        //     steps {
        //         input "Deploy to wwwroot folder?"
        //     }
        // }
		
        stage('Deploy') {
            steps {  
                echo 'Deploying to wwwroot folder Starts!'
		bat 'powershell if (Test-Path "C:\\inetpub\\wwwroot\\ContosoWebApp") { Remove-Item -Path "C:\\inetpub\\wwwroot\\ContosoWebApp" -Recurse -Force }'
                bat 'powershell Expand-Archive -Path .\\artifacts.zip -DestinationPath C:\\inetpub\\wwwroot\\artifacts' 
		bat 'powershell Rename-Item -Path "C:\\inetpub\\wwwroot\\artifacts" -NewName "ContosoWebApp"'
                echo 'Deploying to wwwroot folder Completed.'
            }
        }
        stage('Convert to Application') {
            steps {
                echo 'Converting deployed folder to application Starts!'
                echo 'Creating New Virtual Directory!'
                bat 'powershell -Command "New-WebVirtualDirectory -Site \'Default Web Site\' -Name \'ContosoWebApp\' -PhysicalPath \'C:\\inetpub\\wwwroot\\ContosoWebApp\'"'
                echo 'Convert To IIS WebApplication!'
                bat 'powershell -Command "ConvertTo-WebApplication -PSPath \'IIS:\\Sites\\Default Web Site\\ContosoWebApp\'"'
                echo 'Converting deployed folder to application Completed.'
            }
        }
    }
}
