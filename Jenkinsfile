pipeline {
    agent any
    stages {
        stage('depedency-check-Analysis'){
            steps{
          dependencycheck additionalArguments: '--format XML', odcInstallation: 'OSWAP-dependency-check'
            }
        }
        stage('Dependency-Check-xml-report') {
            steps{
            dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
            /* groovylint-disable-next-line DuplicateStringLiteral, LineLength */
            archiveArtifacts allowEmptyArchive: true, artifacts: '**/dependency-check-report.xml', onlyIfSuccessful: true
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
                 bat "docker run -t owasp/zap2docker-stable zap-baseline.py -t http://www.vulnweb.com"
                }
            }
        }
    }
}
