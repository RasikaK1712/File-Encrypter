node {
    try {

        stage('Build') {
            sh '''
                echo "Building Java project..."
                echo "Listing workspace contents:"
                ls

                cd "Password Protection"

                mkdir -p build

                # Compile main sources
                javac -d build src/*.java

                # Copy IntelliJ-generated class files into build
                cp -r out/production/* build/

                echo "Build successful"
            '''
        }

        stage('Test') {
            sh '''
                echo "Running JUnit tests for File-Encrypter..."

                cd "Password Protection"

                # Remove any corrupted JAR
                rm -f junit-platform-console-standalone.jar

                # Download correct JUnit JAR (single line ONLY)
                curl -L -o junit-platform-console-standalone.jar https://repo1.maven.org/maven2/org/junit/platform/junit-platform-console-standalone/1.10.0/junit-platform-console-standalone-1.10.0.jar

                echo "Downloaded JAR details:"
                ls -lh junit-platform-console-standalone.jar

                # Compile test files (add out/production for IntelliJ classes)
                mkdir -p test-build
                javac -cp junit-platform-console-standalone.jar:build:out/production -d test-build test/*.java

                # Run JUnit tests
                java -jar junit-platform-console-standalone.jar \
                    --class-path build:test-build:out/production \
                    --scan-class-path

                echo "JUnit tests executed successfully"
            '''
        }

        stage('Deploy') {
            sh '''
                echo "Packaging File-Encrypter application..."

                cd "Password Protection"

                jar cf FileEncrypter.jar -C build .

                echo "Deployment successful - Artifact ready"
            '''
        }

        echo "Pipeline executed successfully!"

    } catch (Exception e) {
        echo "Pipeline failed!"
        throw e
    }
}
