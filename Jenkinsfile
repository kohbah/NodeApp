node {
    def app

    stage('Clone repository') {
        /* Cloning the Repository to our Workspace */
	checkout scm
      steps {
        git 'https://github.com/gustavoapolinario/microservices-node-example-todo-frontend.git'      
      }
    }

    stage('Build image') {
        /* This builds the actual image */

        app = docker.build("docker/latest")
    }

    stage('Test image') {
        
        app.inside {
            echo "Tests passed"
        }
    }

    stage('Push image') {
        /* 
			You would need to first register with DockerHub before you can push images to your account
		*/
        docker.withRegistry('http://10.0.1.113:8081/artifactory', 'jfrog') {
            app.push("${env.BUILD_NUMBER}")
            app.push("latest")
            } 
                echo "Trying to Push Docker Build to jfrog artifactory"
    }
    stage('Remove Unused docker image') {
      steps{
        sh "docker rmi $registry:$BUILD_NUMBER"
      }
    }
}
