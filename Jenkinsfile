pipeline{
    //Directives
    agent any
    tools {
        maven 'maven'
    }

    stages {
        // Specify various stage with in stages

        // stage 1. Build
        stage ('Build'){
            steps {
                sh 'mvn clean install package'
            }
        }

        // Stage2 : Testing
        stage ('Test'){
            steps {
                echo ' testing......'

            }
        }
		//Stage3 : publish the artifacts to Nexus
		stage ('Publish to Nexus') {
		    steps {
			   nexusArtifactUploader artifacts: [[artifactId: 'KuruDevOpsLab', classifier: '', file: 'target/KuruDevOpsLab-0.0.3-SNAPSHOT.war', type: 'war']], credentialsId: '12b394cd-4b64-4fce-9e7b-2e9910b86ff4', groupId: 'com.kuru', nexusUrl: '172.20.10.144:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'KuruDevopslab-SNAPSHOT', version: '0.0.3-SNAPSHOT'
			}
		
		
		}

		
		
		// Stage4 : Deploy
        stage ('Deploy'){
            steps {
                echo ' Deploying......'

            }
        }



        
        
    }

}