pipeline {
    agent  any;
    stages {
	stage('Comprobaciones') {
	    steps {
		sh 'whoami'
	    }
	}
        stage('Preparing the environment') {
	    steps {
		sh 'python -m pip install -r requirements.text'
	    }
        stage('Calidad de código') {
            steps {
                sh 'python -m pylint app.py'
            }
        }
        stage('Test Unitario') {
            steps {
                sh 'python -m pytest'
            }
        }
        stage('Build') {
            agent {
		node {
		   label "DockerServer";
		}
                sh 'echo creando paquete de aplicación'
            }
	    steps {
		sh 'docker build https://github.com/Atari96/MisCodigos.git -t MisCodigos:latest'
	    }
        }
     
      stage('Deploy') {
   	  agent {
		node{
                   label "DockerServer";
		}
	  }       
          steps {
              sh 'docker run -tdi -p 5000:5000 MisCodigos:latest'
          }
      }
    
	}
}
