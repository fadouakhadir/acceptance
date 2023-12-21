pipeline {
    agent any

    stages {
	stage("Package") {
		steps {
			sh "./gradlew build"
		}
	}
	stage("Docker build") {
		steps {
			sh "docker build -t calculator ."
		}
	}
    }
}
