// pipeline {
//     agent any
    
//     environment {
//         // Set the Maven home path
//         MAVEN_HOME = tool 'Maven 3.x'
//         AWS_ACCESS_KEY_ID     = credentials('jenkins-aws-secret-key-id')
//         AWS_SECRET_ACCESS_KEY = credentials('jenkins-aws-secret-access-key')
//     }

//     stages {
//         stage('Checkout') {
//             steps {
                
//             }
//         }

//         stage('Build') {
//             steps {
//                 // Build Maven project
//                 sh "${MAVEN_HOME}/bin/mvn clean install"
//             }
//         }

//         stage('Deploy to Tomcat') {
//             steps {
//                 // Deploy the WAR file to Tomcat
//                 deploy adapters: [
//                     tomcat(credentialsId: 'your-tomcat-credentials-id', url: 'http://your-tomcat-server:8080', path: '/manager/text', war: 'target/your-application.war')
//                 ]
//             }
//         }
//     }
// }

pipeline {
    agent any
    
    environment {
        AWS_ACCESS_KEY_ID     = credentials('jenkins-aws-secret-key-id')
        AWS_SECRET_ACCESS_KEY = credentials('jenkins-aws-secret-access-key')

    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout source code from Git repository
                git 'https://github.com/PhiThai1309/Simple-Webapp-COSC2767.git'
                // Installing dependencies
                yum install git -y

                // Git configuration
                git config --global user.email "s3878070@rmit.edu.vn"
                git config --global user.name "Phi Thai"
                git config --global init.defaultBranch main
            }
        }

        stage('Build') {
            steps {
                // Build Maven project
                sh ‘mvn clean package’
            }
        }

        stage(‘Deploy’) {
            environment {
                TOMCAT_URL = ‘http://3.87.112.240:8080'
                TOMCAT_USER = ‘admin’
                TOMCAT_PASSWORD = ‘s3cret’
                CONTEXT_PATH = ‘/Simple-Webapp-COSC2767’ // e.g., /myapp
            }
            steps {
                // Deploy the built war file to Tomcat
                sh “curl — upload-file target/Simple-Webapp-COSC2767.war ${TOMCAT_URL}/manager/text/deploy?path=${CONTEXT_PATH} -u ${TOMCAT_USER}:${TOMCAT_PASSWORD}”
            }
        }
        
    }
}