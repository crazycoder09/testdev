pipeline {
    agent any

    environment {
        MASTER_BRANCH = 'master'
        TEST_BRANCH = 'master1'
    }

    stages {
        stage('Checkout') {
            steps {
                script {
                    // Checkout the master branch
                    checkout([$class: 'GitSCM', branches: [[name: MASTER_BRANCH]], userRemoteConfigs: [[url: 'https://github.com/crazycoder09/testdev.git']]])
                }
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    // Run your tests
                    sh 'php vendor/bin/codecept run --steps'
                }
            }
        }

        stage('Merge to master1') {
            steps {
                script {
                    // Merge the changes to master1
                    sh "git checkout ${TEST_BRANCH}"
                    sh "git merge ${MASTER_BRANCH}"
                    sh "git push origin ${TEST_BRANCH}"
                }
            }
        }
    }
}
