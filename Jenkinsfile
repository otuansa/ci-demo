pipeline{
    agent any
	tools {
        maven 'Devops maven 3.9.10'
        jdk 'Learner JDK'
    }
    stages{
        stage('init'){
            steps{
                script{
                    println("Hello world and Welcome to DEVOPS")
                }
            }
        }
        stage('Build') {
            steps {
		script{
		    sh 'rm -rf maven_project/target'
	            dir('maven_project'){
			sh 'mvn -B -DskipTests clean package' 
		    }
		}
            }
        }
        stage('Test') {
            steps {
		script{
	            dir('maven_project'){
			sh 'mvn test' 
		    }
		}
            }
        }
        stage('upload jar to AWS'){
            steps{
                script{                    
                    withAWS(credentials: 'Learner', region: 'eu-west-2') {
                        sh '''echo "Uploading the tested jar file to s3 for later deployments" '''
                        s3Upload(pathStyleAccessEnabled: true, payloadSigningEnabled: true, file:'maven_project/target/my-app-1.0-SNAPSHOT.jar', bucket:'learning-rest-api', path:'ci-demo/javaapp/myapp.jar')
                    }
                }
            }
        }
    }
   post{
        always{
                script{  
                    echo  '''this is always executed '''
		    cleanWs()
            }
        }
    }
}
