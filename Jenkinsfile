pipeline {
    agent any
    tools { 
        maven 'CWK_MVN' 
        jdk 'CWK_SENSE' 
    }
    stages {
        stage('Prepare') {
            steps {
                bat 'git bisect start'
                bat 'git bisect bad 198644632661c67b6c32f59e9047c11a70685e15'
                bat 'git bisect good 98ac319c0cff47b4d39a1a7b61b4e195cfa231e5'
            }
        }
        stage('Git Bisect') {
            steps {
                script {
                    // Define a method that runs mvn clean test and checks if it succeeds
                    def runMavenTest = {
                        try {
                            bat 'mvn clean test'
                            return true // Return true if mvn test succeeds
                        } catch (any) {
                            return false // Return false if mvn test fails
                        }
                    }
                    // Use the method in git bisect run
                    bat 'git bisect run powershell -Command "if(runMavenTest) { exit 0 } else { exit 1 }"'
                }
            }
        }
        stage('Complete') {
            steps {
                bat 'git bisect reset' // Always clean up bisect state afterwards
            }
        }
    }
}
