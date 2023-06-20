pipeline {
    
	agent any
    tools {
        maven "MAVEN3"
        jdk "JAVA_HOME"
    }
	
    environment {
        NEXUS_VERSION = "nexus3"
        NEXUSPORT = "8081"
        NEXUSIP = "172.31.7.0"
        SNAP_REPO = 'vprofile-maven-snap'
        NEXUS_USER = 'admin'
        NEXUS_PASS = 'admin'
        RELEASE_REPO = "vprofile-maven-central"
	    NEXUS_GRP_REPO = "vprofile-maven-group"
        NEXUS_LOGIN = "nexuslogin"
        SONARSERVER = 'sonarserver'
        SONARSCANNER = 'sonarscanner'
        
    }
	
    stages {
        stage('build'){
            steps {
                sh 'mvn -s settings.xml -DskipTests install'
            }
        
            post {
                success {
                    echo "now archieved"
                    archiveArtifacts artifacts: '**/*.war'
                }
            }
        }            

        stage('Test'){
            steps {
                sh 'mvn -s settings.xml test'
            }
        }

        stage('checkStyle'){
            steps {
                sh 'mvn -s settings.xml checkstyle:checkstyle'
            }
        }

     
        stage('Sonar Analysis'){
            environment {
                scannerHome = tool "${SONARSCANNER}"
            }
            steps {
                withSonarQubeEnv("${SONARSERVER}") {
                   sh '''${scannerHome}/bin/sonarscanner -Dsonar.projectKey=vprofile \
                   -Dsonar.projectName=vprofile \
                   -Dsonar.projectVersion=1.0 \
                   -Dsonar.sources=src/ \
                   -Dsonar.java.binaries=target/test-classes/com/visualpathit/account/controllerTest/ \
                   -Dsonar.junit.reportsPath=target/surefire-reports/ \
                   -Dsonar.jacoco.reportsPath=target/jacoco.exec \
                   -Dsonar.java.checkstyle.reportPaths=target/checkstyle-result.xml'''

            }
        }
    }
    }
}




