pipeline {
  agent any
  stages {
    stage('checkout') {
      steps {
        git(url: 'git@github.com:suchsatdevops/spring-framework-petclinic.git', branch: 'master')
      }
    }

    stage('build') {
      steps {
        sh '/opt/maven38/bin/mvn clean package -P MySQL'
      }
    }

    stage('upload') {
      steps {
        sh 'aws s3 cp target/petclinic.war s3://suchsatbucket/petclinic-$BUILD_NUMBER.war'
      }
    }

    stage('deploy') {
      steps {
        sh '''
            aws s3 cp s3://suchsatbucket/petclinic-$BUILD_NUMBER.war .
            scp petclinic-$BUILD_NUMBER.war root@10.0.59.137:/opt/tomcat/webapps/petclinic.war
        '''
      }
    }

  }
}