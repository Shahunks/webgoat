pipeline {
    agent any
    stages {
        stage('depedency-check-Analysis'){
            steps{
          dependencycheck additionalArguments: '--format XML --format HTML', odcInstallation: 'OSWAP-dependency-check'
            }
        }
        stage('Dependency-Check-xml-report') {
            steps{
            dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
            /* groovylint-disable-next-line DuplicateStringLiteral, LineLength */
            archiveArtifacts allowEmptyArchive: true, artifacts: '**/dependency-check-report.html', onlyIfSuccessful: true
            }
        }
        stage('SAST'){
            steps{
                catchError(buildResult: 'SUCCESS', message: 'FAILURE', stageResult: 'FAILURE') {
                 bat "docker run -v ${WORKSPACE}:/src --workdir /src returntocorp/semgrep-agent:v1 semgrep-agent --config p/ci --config p/security-audit --config p/secrets"

                }
                
            }
        }
         stage('DAST'){
            steps{
                catchError(buildResult: 'SUCCESS', message: 'FAILURE', stageResult: 'FAILURE') {
                 bat "docker run -t owasp/zap2docker-stable zap-fullscan.py -t http://www.vulnweb.com"
                }
            }
        }
    }
}
