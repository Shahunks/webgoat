/* groovylint-disable-next-line CompileStatic */
pipeline {
    agent any
    stages {
        stage('SCM') {
            steps {
                git branch: 'main', url: 'https://github.com/Shahunks/webgoat.git'
            }
        }
        stage('depedency-check-Analysis'){
            steps{
          dependencycheck additionalArguments: '--format XML', odcInstallation: 'OSWAP-dependency-check'
            }
        }
        stage('Dependency-Check') {
            steps{
            dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
            /* groovylint-disable-next-line DuplicateStringLiteral, LineLength */
            archiveArtifacts allowEmptyArchive: true, artifacts: '**/dependency-check-report.xml', onlyIfSuccessful: true
            }
        }
        stage('SAST'){
            steps{
               sh "docker run -v ${WORKSPACE}:/src --workdir /src returntocorp/semgrep"
            }
        }
    }
}

