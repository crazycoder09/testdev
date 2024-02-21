pipeline {
    agent any

    tools {
        git 'Default Git'
    }
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

                    // Check if the TEST_BRANCH exists, if not, create it
                    def branchExists = sh(script: "git show-ref refs/heads/${TEST_BRANCH}", returnStatus: true) == 0
                    if (!branchExists) {
                        echo "Branch ${TEST_BRANCH} doesn't exist. Creating it..."
                        sh "git checkout -b ${TEST_BRANCH}"
                        sh "git push origin ${TEST_BRANCH}"
                    } else {
                        echo "Branch ${TEST_BRANCH} already exists."
                    }
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
