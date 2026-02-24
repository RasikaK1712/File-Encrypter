g/maven2/org/junit/platform/junit-platform-
console-standalone/1.10.0/junit-platform-console-standalone-1.10.0.jar
fi
# Compile test files (test folder beside src)
mkdir -p test-build
javac -cp junit-platform-console-standalone.jar:build -d test-build
test/*.java
# Run JUnit tests
java -jar junit-platform-console-standalone.jar \
--class-path build:test-build \
--scan-class-path
echo "JUnit tests executed successfully"
'''
}
stage('Deploy') {
sh '''
echo "Deploying (Packaging) File-Encrypter Application..."
cd "Password Protection"
# Create executable artifact (JAR)
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
