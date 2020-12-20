node {
    def app

    stage('Clone repository') {
        checkout scm
    }

    stage('Build image') {
        /* This builds the actual image; synonymous to
         * docker build on the command line */

        app = docker.build("kieranxfrancois/CW2repo:1.0")
    }

    stage('Push image') {
        /* Finally, we'll push the image with two tags:
         * First, the incremental build number from Jenkins
         * Second, the 'latest' tag.
         * Pushing multiple tags is cheap, as all the layers are reused. */
        docker.withRegistry('https://hub.docker.com/repository/docker/kieranxfrancois/cw2repo', 'bae275df-7114-46d1-9046-87e10770d0fa') {
            app.push("${env.BUILD_NUMBER}")
            app.push("latest")
        }
    }
}
