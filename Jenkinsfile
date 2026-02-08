pipeline {
agent any
// test
stages {
    stage('Test') {
        steps {

            junit 'target/surefire-reports/*.xml'
        }
    }
    stage('Build') {
        steps {
            bat './mvnw clean install'
            archiveArtifacts artifacts: 'target/*.jar'
        }
    }

    stage('documentation') {
            steps {
                bat './mvnw javadoc:javadoc'

                bat '''
                mkdir -p target/site/apidocs
                cp -r target/site/apidocs/* docs/
                zip -r target/site/apidocs.zip docs
                '''
                archiveArtifacts artifacts: 'docs.zip'
            }
        }

}
}