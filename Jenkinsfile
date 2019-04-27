pipeline {
  agent any

   stages {

    stage('Clone project') {
        steps {
            sh '''
               cd /tmp
               rm -r sample-web-service
               git clone https://github.com/fiskra/sample-web-service.git
               '''
        }
    }

    stage('Build') {
        steps {
            echo "Compiling..."
            sh '''
               cd /tmp/sample-web-service
               mvn -B -DskipTests clean package
               '''
        }
    }

    stage ('Run') {
        steps {
            sleep 360
            sh "ansible-playbook -i hosts playbook.yml"
        }
    }

    stage ('Health check') {
        steps {
            sleep 240

            httpRequest responseHandle: 'NONE', timeout: 2500, url: 'http://127.0.0.1:5001/sample.web.service-0.0.1-SNAPSHOT/users', validResponseCodes: '200', consoleLogResponseBody: true
        }
    }
   }
}
