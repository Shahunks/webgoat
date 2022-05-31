/* groovylint-disable-next-line CompileStatic */
pipeline {
    agent any
    stages {
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
                git branch: 'main', url: 'https://github.com/Shahunks/webgoat.git'
                sh 'ls'
                //sh "docker run -v /var/jenkins_home/workspace/Devsecops:/src --workdir /src returntocorp/semgrep-agent:v1 semgrep-agent --config p/ci --config p/security-audit --config p/secrets"
            }
        }
    }
}

