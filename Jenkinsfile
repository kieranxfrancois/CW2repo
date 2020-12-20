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

    
stage('SonarQube') {
    environment {
        scannerHome = tool 'SonarQube'
    }
        withSonarQubeEnv('SonarQube') {
            sh "${scannerHome}/bin/sonar-scanner"
        }
        timeout(time: 10, unit: 'MINUTES') {
            waitForQualityGate null: true
        }
    }
}
