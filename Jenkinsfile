library identifier: 'pipeline-library@master', retriever: modernSCM(
    [$class: 'GitSCMSource', 
    credentialsId: '', 
    id: 'pipeline-library', 
    remote: 'https://github.com/devopspatterns/pipeline-library.git', 
    traits: [[$class: 'jenkins.plugins.git.traits.BranchDiscoveryTrait']]
]) 

pipeline {
    agent {
        docker {
            image 'maven:3-alpine'
        }
    }
    
    options {
        // General Jenkins job properties
        buildDiscarder(logRotator(numToKeepStr:'6'))
    }

    stages {
        stage('Build') {
            steps {
                sh 'mvn -B -DskipTests clean package'
                echo 'Build done'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
                echo 'Test done'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        stage('Publish') {
            steps {
                echo 'Publishing to Artifactory Now'
                publishMavenEvent()
            }
        }
    }
}
