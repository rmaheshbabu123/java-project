pipeline {
    agent any
    tools{
        jdk 'jdk'
        maven 'mvn'	
    }
    parameters {
        choice( choices: ['enable' , 'disable'],description: '',name: 'sonarscan')
    }

    environment{
       SONARQUBE_URL = "http://52.66.28.11"
       SONARQUBE_PROJECT = "java-project"
       TEST     = "enable"
       IMAGE_NAME = "java-app"
    }
    stages {
        stage('build'){
            steps{
                script{
                    sh """
                        mvn clean
                        mvn install
                    """
                }
            }
        }
        stage('Test'){
            when {
                 expression { env.TEST == 'enable' }
            }
            steps{
                script{
                    sh """
                        mvn test
                    """ 
                }
            }
        }
        stage('Sonarqube Evalution'){
            when {
                  expression { params.sonarscan == 'enable' }
            }
            steps{
                catchError(buildResult: 'SUCCESS') {
                    script{
                        sh """
                        mvn sonar:sonar -Dsonar.projectKey=${SONARQUBE_PROJECT} -Dsonar.host.url=${SONARQUBE_URL} -Dsonar.login=fb4e8b6c07b1d389514b831963703708d58df298
                        """
                    }
                }
            }
        }   
        stage('Upload artifacts to Jfrog'){
            steps{
                script{
                    def server = Artifactory.server 'jfrog-art'
                    def uploadSpec = """{
                    "files": [{
                       "pattern": "target/*.jar",
                       "target": "artifactory/libs-snapshot/"
                    }]
                    }"""
                    server.upload(uploadSpec) 
                }
            }
        }
        stage('Dockerize Java application'){
            steps{
                script{
                    sh """
                        docker build -t ${IMAGE_NAME} .
                        docker images | grep -i ${IMAGE_NAME}
                    """
                }
            }  
        }
        stage('Upload image to ECR'){
            steps{
                script{
                    sh '$(aws ecr get-login --region ap-south-1 --no-include-email)'
                    sh 'docker tag java-app:latest 558607277863.dkr.ecr.ap-south-1.amazonaws.com/java-app:latest'
                    sh 'docker push 558607277863.dkr.ecr.ap-south-1.amazonaws.com/java-app:latest'
                }
            }
        }
    }
}