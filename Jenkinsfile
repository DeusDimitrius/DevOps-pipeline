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
            sh '''
               cd /home/Ansible
               ansible-playbook playbook.yml
               '''
        }
    }

    stage ('Health check') {
        steps {
            sleep 240

            httpRequest responseHandle: 'NONE', timeout: 2500, url: 'http://172.20.128.13:8080/sample.web.service-0.0.1-SNAPSHOT/users', validResponseCodes: '200', consoleLogResponseBody: true
        }
    }
   }

   post {
    failure {
     mail bcc: '', body: 'ERROR CD: Project name -> sample.web.service', cc: '', from: '', replyTo: '', subject: "ERROR CD: Project name -> sample.web.service", to: 'fferide.celik@gmail.com, admin.admin@gmail.com';
    }
   }

}
