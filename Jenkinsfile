pipeline{
    //Directives
    agent any
    tools {
        maven 'maven'
    }
	
	 environment{
       ArtifactId = readMavenPom().getArtifactId()
       Version = readMavenPom().getVersion()
       Name = readMavenPom().getName()
       GroupId = readMavenPom().getGroupId()
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
                script {

               def NexusRepo = Version.endsWith("SNAPSHOT") ? "KuruDevOpsLab-SNAPSHOT" : "KuruDevOpsLab-RELEASE"

			   nexusArtifactUploader artifacts: 
               [[artifactId: "${ArtifactId}", 
               classifier: '', 
               file: "target/${ArtifactId}-${Version}.war", 
               type: 'war']], 
               credentialsId: '066798d5-428d-4ea8-94d3-9365f1a18938',
               groupId: "${GroupId}", 
               nexusUrl: '172.20.10.43:8081', 
               nexusVersion: 'nexus3', 
               protocol: 'http', 
               repository: "${NexusRepo}", 
               version: "${Version}"
			}

          }
		
		
		}
		
		// Stage 4 : Print some information
        stage ('Print Environment variables'){
            steps {
                   echo "Artifact ID is '${ArtifactId}'"
                   echo "Version is '${Version}'"
                   echo "GroupID is '${GroupId}'"
                   echo "Name is '${Name}'"
            }
        }

        // Stage 5 :Deploy
        stage ('Deploy'){
            steps {
                   echo 'deploying.........................'
                  
            }
        }


		
		
		Stage 5 :Deploy the build artifact to tomcar
        stage ('Deploy to Tomcat'){
            steps {
                echo ' Deploying to tomcat node......'
                sshPublisher(publishers: 
                [sshPublisherDesc(configName: 'Ansible_controller_jenkins', 
                transfers: 
                [sshTransfer(cleanRemote: false, excludes: '', 
                execCommand: 'ansible-playbook /opt/playbooks/downloadanddeploy.yaml -i /opt/playbooks/hosts', 
                execTimeout: 120000, 
                flatten: false, 
                makeEmptyDirs: false, 
                noDefaultExcludes: false, 
                patternSeparator: '[, ]+', 
                remoteDirectory: '', 
                remoteDirectorySDF: false, 
                removePrefix: '', sourceFiles: '')], 
                usePromotionTimestamp: false, 
                useWorkspaceInPromotion: false, 
                verbose: true)])

            }
        }

        // // Stage 6 : Deploy the build artifact to docker
        // stage ('Deploy to Docker'){
        //     steps {
        //         echo ' Deploying......'
        //         sshPublisher(publishers: 
        //         [sshPublisherDesc(configName: 'Ansible_controller_jenkins', 
        //         transfers: 
        //         [sshTransfer(cleanRemote: false, excludes: '', 
        //         execCommand: 'ansible-playbook /opt/playbooks/downloadanddeploy_docker.yaml -i /opt/playbooks/hosts', 
        //         execTimeout: 120000, 
        //         flatten: false, 
        //         makeEmptyDirs: false, 
        //         noDefaultExcludes: false, 
        //         patternSeparator: '[, ]+', 
        //         remoteDirectory: '', 
        //         remoteDirectorySDF: false, 
        //         removePrefix: '', sourceFiles: '')], 
        //         usePromotionTimestamp: false, 
        //         useWorkspaceInPromotion: false, 
        //         verbose: true)])

        //     }
        // }

        



        
        
    }

}