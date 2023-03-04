node {
    def app

    stage('Clone repository') {
      

        checkout scm
    }

    stage('Build image') {
  
       app = docker.build("eshnil/test-jenkins")
    }

    stage('Test image') {
        docker.image('eshnil/test-jenkins').withRun('
                                           ' -p 8888:8080') { c ->
        /* Wait until mysql service is up */
        sh 'wget 0.0.0.0:8888'
        /* Run some tests which require MySQL */
        sh 'make check'
    }

        app.inside {
            sh 'echo "Tests passed"'
        }
    }

    stage('Push image') {
        
        docker.withRegistry('https://registry.hub.docker.com', 'git') {
            app.push("${env.BUILD_NUMBER}")
            app.push("latest")
        }
    }
}
