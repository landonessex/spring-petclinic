 pipeline {
    agent any
    
    triggers {
        cron('H/10 * * * 1') 
    }

    tools {
        jdk 'JDK17'
        maven 'Maven3'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/landonessex/spring-petclinic.git'
            }
        }

        stage('Build and Test') {
            steps {
                bat 'mvn clean package'
            }
        }

        stage('Code Coverage') {
            steps {
                bat 'mvn verify org.jacoco:jacoco-maven-plugin:prepare-agent'
            }
            post {
                success {
                    jacoco(
                        execPattern: 'target/*.exec',
                        classPattern: 'target/classes',
                        sourcePattern: 'src/main/java',
                        exclusionPattern: 'src/test*'
                    )
                }
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}
