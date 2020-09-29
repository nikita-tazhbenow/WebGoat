pipeline{
    agent any
    
    tools{
        maven "maven"
    }
    
    stages{
	stage("Initialize"){
            steps{
                sh '''

			echo "PATH=${PATH}"
			echo "M2_HOME = ${M2_HOME}"
		     '''
		}
	}
        stage("Checkout code"){
            steps{
                echo "Checkout code in progress..."
                checkout([$class: 'GitSCM', branches: [[name: '*/develop']], 
                doGenerateSubmoduleConfigurations: false, 
                extensions: [], 
                submoduleCfg: [], 
                userRemoteConfigs: [[url: 'https://github.com/WebGoat/WebGoat']]])
                echo "Finished!"
            }
        } 
	stage("Dependency analysis"){
            steps{
                echo "Performing OSS analysis from the commit..."
                sh "rm owasp* || true"
                sh "wget https://raw.githubusercontent.com/nikita-tazhbenow/WebGoat/develop/owasp-dependency-check.sh"
                sh "chmod +x owasp-dependency-check.sh"
                sh "bash owasp-dependency-check.sh"
		sh "cat /var/lib/jenkins/OWASP-Dependency-Check/reports/dependency-check-report.xml"
                echo "Finished!"
            }
        }
        stage("Build code"){
            steps{
                echo "Building code in progress..."
                sh "mvn -B -DskipTests clean package"
                echo "WebGoat was successfully built!"
            }
        } 
        
    }
}    