node {
    def app

    stage('Clone repository') {
        checkout scm
    }
    
    stage('Build image') {
        app = docker.build("kieranxfrancois/cw2repo:1.0")
    }
    
    stage('Push image') {
        docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials') {
            app.push("${env.BUILD_NUMBER}")
            app.push("latest")
        }
    }
        stage('Sonarqube') {
    environment {
        scannerHome = tool 'SonarQube'
    }
            steps {
        withSonarQubeEnv('sonarqube') {
            sh "/var/jenkins_home/sonarqube/sonar-scanner-3.3.0.1492-linux/"
        }
        timeout(time: 10, unit: 'MINUTES') {
            waitForQualityGate abortPipeline: true
        }
    }
}
}
