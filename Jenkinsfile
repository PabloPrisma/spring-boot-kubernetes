pipeline {
    agent any

    tools {
        maven 'Maven'
        nodejs 'NodeJS'
    }
  
    stages {
        stage('Iniciando...'){
            steps{
             sh '''
              echo "PATH = ${PATH}"
              echo "M2_HOME = ${M2_HOME}"
              '''
            }
        }
        
        stage('Compilando...'){
            steps{
                sh 'mvn clean compile -e'
            }
        }
        
        stage('Testing...'){
            steps{
                sh 'mvn clean test -e'
            }
        }
        
        stage('Análisis de código estático (SCA)...'){
            steps{
                sh 'mvn org.owasp:dependency-check-maven:check'
                
                archiveArtifacts artifacts: 'target/dependency-check-report.html', followSymlinks: false
            }
        }
        
        stage('Checkeo de seguridad de aplicaciones estáticas (SAST)...'){
            steps{
                script{
                    def scannerHome = tool 'Sonarqube'
                    
                    withSonarQubeEnv('Sonarqube'){
                        sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=devsecops -Dsonar.sources=. -Dsonar.java.binaries=target/classes   -Dsonar.host.url=http://localhost:9001 -Dsonar.login=10dacc506db1dd40457724259d5374fd3a1e1c85"
                    }
                }
            }
        }
        
    }
    
}
