pipeline {
    agent any

    stages {

        stage('Build') {
            steps {
                sh '''
                    echo "Building Java project..."
                    cd "Password Protection"

                    mkdir -p build

                    # Compile source files
                    javac -d build src/*.java

                    # Copy IntelliJ-generated compiled classes needed by tests
                    cp -r out/production/* build/

                    echo "Build successful"
                '''
            }
        }

        stage('Test') {
            steps {
                sh '''
                    echo "Running JUnit tests..."

                    cd "Password Protection"

                    rm -f junit-platform-console-standalone.jar

                    curl -L -o junit-platform-console-standalone.jar \
                    https://repo1.maven.org/maven2/org/junit/platform/junit-platform-console-standalone/1.10.0/junit-platform-console-standalone-1.10.0.jar

                    echo "JUnit jar size:"
                    ls -lh junit-platform-console-standalone.jar

                    mkdir -p test-build

                    # Add out/production for IntelliJ UI Designer classes
                    javac -cp junit-platform-console-standalone.jar:build:out/production -d test-build test/*.java

                    java -jar junit-platform-console-standalone.jar \
                     --class-path build:test-build:out/production \
                     --scan-class-path

                    echo "JUnit tests completed successfully"
                '''
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                    echo "Packaging application..."
                    cd "Password Protection"
                    jar cf FileEncrypter.jar -C build .
                    echo "Artifact ready"
                '''
            }
        }
    }

    post {
        success { echo "Pipeline executed successfully!" }
        failure { echo "Pipeline failed!" }
    }
}
