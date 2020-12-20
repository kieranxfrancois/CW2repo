node {
    def app

    stage('Clone repository') {
        checkout scm
    }
    
        steps {
        withSonarQubeEnv('sonarqube') {
            sh "${scannerHome}/bin/sonar-scanner"
        }
        timeout(time: 10, unit: 'MINUTES') {
            waitForQualityGate abortPipeline: true
        }
    }
}
    
    stage('Build image') {
        app = docker.build("kieranxfrancois/cw2repo:1.0")
    }
    
        stage('Sonarqube') {
    environment {
        scannerHome = tool 'SonarQube'
    }

    stage('Push image') {
        docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials') {
            app.push("${env.BUILD_NUMBER}")
            app.push("latest")
        }
    }
}
