pipeline {
	agent any
	stages {
	
        stage('Build') {
        	steps {
                // sh './gradlew -b build.gradle clean build'  
                // sh './gradlew assemble'   
                sh './gradlew build'

            }
        }
         
        stage('Parallel Stages') {
                parallel {
                    stage('Archival') {
                        steps {
                            script {
                                archiveArtifacts 'build/libs/*.?ar'
                            }   
                        }
                    }
                    // stage('SonarQube analysis') {
                    //     steps {
                    // script {
                    //         withSonarQubeEnv('sonar') {
                    //         sh './gradlew  sonarqube'
                    //     } 
                    //     }
                    //     }
                    // }
                    stage('Test Reports') {
                        steps {
                            script{
                                junit 'build/test-results/test/*.xml'
                            } 
                        }
                    }
                }
            }
               
            stage('Artifact Uploader') {
                steps {
                    script {
                        sh "pwd"
                        sh "ls -la"
                        sh "ls -la build/libs/"
                        // echo "artifactId: $artifactId"
                        // echo "file: $file"
                        // echo "type: $type"
                        // echo "credentialsId: $credentialsId"
                        // echo "groupId: $groupId" 
                        // echo "nexusUrl: $nexusUrl"
                        // echo "nexusVersion: $nexusVersion" 
                        // echo "protocol: $protocol" 
                        // echo "repository: $repository"
                        // echo "version: $version"
                        nexusArtifactUploader artifacts: 
                        [[  artifactId: 'spring-boot-api-example', 
                            file: "build/libs/spring-boot-api-example-${version}.jar", //build/libs/spring-boot-api-example-0.1.0-SNAPSHOT.jar'
                            type: 'jar']], 
                            credentialsId: 'nexusAdminCreds', 
                            groupId: 'com.nisum.mytime', 
                            nexusUrl: '172.16.16.101:8081', 
                            nexusVersion: 'nexus3', 
                            protocol: 'http', 
                            repository: 'spring-boot-api-example-release', 
                            // version: "0.0.1"
                            version: "1.0.${BUILD_NUMBER}"
                    }
                }
            }
            // stage('Deploy to PreProd') {
            //     steps {
            //         sh '''
            //             scp build/libs/myTeam.war tomcat@10.3.45.131:/opt/tomcat/webapps/myTeam.war;
            //             ssh tomcat@10.3.45.131 /opt/tomcat/bin/shutdown.sh;
            //             ssh tomcat@10.3.45.131 /opt/tomcat/bin/startup.sh
            //         '''
            //         //sshPublisher(publishers: [sshPublisherDesc(configName: 'preprod', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: '/opt/tomcat/bin/shutdown.sh;sleep 5;/opt/tomcat/bin/startup.sh', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '/opt/tomcat/webapps/', remoteDirectorySDF: false, removePrefix: '', sourceFiles: 'build/libs/*.war')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
            //     }
            // }
    }
         
    // post {
   	//     always {
   	// 	    notify('started')
   	//     }
   	//     failure {
   	// 	    notify('err')
   	//     }
   	//     success {
   	// 	    notify('success')
   	//     }
    // }
}
 
// def notify(status){
// 	emailext (
// 	to: "sneelam@nisum.com,rtirumalapuram@nisum.com",
// 	subject: "${status}: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
// 	body: """<p>${status}: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]':</p>
//     	<p>Check console output at <a href='${env.BUILD_URL}'>${env.JOB_NAME}  [${env.BUILD_NUMBER}]</a></p>""",
// 	)
// }
