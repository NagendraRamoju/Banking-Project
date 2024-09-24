pipeline {
    agent any

    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "M2_HOME"
    }

    stages {
        stage('Build') {
            steps {
                // Get some code from a GitHub repository
                git 'https://github.com/NagendraRamoju/banking-finance.git'

                // Run Maven on a Unix agent.
                sh "mvn -Dmaven.test.failure.ignore=true clean package"

            }
        }
        stage ('Genarate Test Reports') {
             steps {
                 publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, keepAll: false, reportDir: '/var/lib/jenkins/workspace/project1/target/surefire-reports', reportFiles: 'index.html', reportName: 'HTML Report', reportTitles: '', useWrapperFileDirectly: true])
      }
}
        stage ('Create Docker Image') {
             steps {
                sh 'docker build -t nagendraramoju/banking-project-demo .'
               }
         }
          stage ('Docker-Login') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'Docker-login', passwordVariable: 'dockerpassword', usernameVariable: 'dockerlogin')]) {
                   sh 'docker login -u ${dockerlogin} -p ${dockerpassword}'
                 }
             }
  }
          stage('push-Image') {
             steps {
             sh 'docker push nagendraramoju/banking-project-demo'
 }

}     

        stage('Config & Deployment') {
           steps {
              withCredentials([aws(accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: 'AwsAccessKey', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY')]) {
               dir('terraform-files') {
               sh 'sudo chmod 600 slavekey.pem'
               sh 'terraform init'
               sh 'terraform validate'
               sh 'terraform apply --auto-approve'
}
}
}
}


}




      }
