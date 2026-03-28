        pipeline{
            tools{
                jdk 'myjava'
                maven 'mymaven'
            }
            agent any
            stages{
                stage('Checkout on Master'){
                    agent any
                    steps{
                echo 'cloning...'
                        git 'https://github.com/charlesadah123/Backup-DevOpscodeDemode-repo.git'
                    }
                }
                stage('Compile with slave1'){
                    agent {label 'slave1'}
                    steps{
                       echo 'compiling on slave1...'
                       sh '''
                           echo "=== Compilation Environment ==="
                           echo "JAVA_HOME: $JAVA_HOME"
                           mvn --version
                           echo "=== Starting Maven Compile ==="
                           mvn clean compile
                       '''
                }
                }
                stage('CodeReview with slave1'){
                    agent {label 'slave1'}
                    steps{
                    
                echo 'codeReview...'
                        sh '''
                        export MAVEN_OPTS="-Xmx1024m"   # Give Maven 1GB of RAM
                        mvn pmd:pmd'''
                    }
                }
                stage('UnitTest with slave2'){
                    agent {label 'slave2'}
                    steps{
                    echo 'Testing'
                        sh 'mvn test'
                    }
                    post {
                    success {
                        junit 'target/surefire-reports/*.xml'
                    }
                }	
                }
                stage('Package on master') {
            agent {
                label 'master'
            }
            steps {
                echo 'Packaging...'
                sh 'mvn package'
            }
        }
    }
}


