#!groovy
@Library('gini-shared-library@master') _

pipeline {
    agent {
        label 'python3-slave'
    }

    options {
        gitLabConnection('git.i.gini.net')
    }
    environment {
      image = null
    }
    stages {
        stage('Push package to devpi') {
            steps {
                script {
                  sh 'python setup.py bdist_wheel'
                  sh 'twine upload -r nexus dist/*'
                }
            }
            post {
                failure {
                    updateGitlabCommitStatus name: 'Upload python module', state: 'failed'
                }
                success {
                    updateGitlabCommitStatus name: 'Upload python module', state: 'success'
                }
            }
        }
    }
    post {
        failure {
            updateGitlabCommitStatus name: 'pipeline', state: 'failed'
        }
        success {
            updateGitlabCommitStatus name: 'pipeline', state: 'success'
        }
    }
}
