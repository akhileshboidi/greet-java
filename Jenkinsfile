pipeline {
    agent any

    stages {

        stage('Compile Java') {
            steps {
                sh '''
                rm -rf build
                mkdir build
                javac -d build src/Greet.java
                jar cfe greet.jar Greet -C build .
                '''
            }
        }

        stage('Prepare Package') {
            steps {
                sh '''
                mkdir -p package/usr/local/bin
                cp greet.jar package/usr/local/bin/
                '''
            }
        }

        stage('Build DEB') {
            steps {
                sh '''
                fpm -s dir -t deb -n greet-java -v 1.0 -C package usr
                '''
            }
        }
    }

    post {
        success {
            archiveArtifacts artifacts: '*.deb'
        }
    }
}
