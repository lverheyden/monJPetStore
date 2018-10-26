pipeline {
  agent any
  stages {
    stage('Recupération des sources') {
      steps {
        git(url: 'https://github.com/lverheyden/monJPetStore.git', branch: 'master', credentialsId: 'lverheyden')
      }
    }
    stage('build Maven') {
      steps {
        bat(script: 'runmaven.bat', encoding: 'utf-8')
      }
    }
    stage('Qualimetrie') {
      steps {
        withSonarQubeEnv('Sonar') {
            bat(script: 'runquality.bat', encoding: 'utf-8')}
      }
    }
    stage("Quality Gate") {
        steps {
		sleep 10
            timeout(time: 4, unit: 'MINUTES') {
                // Parameter indicates whether to set pipeline to UNSTABLE if Quality Gate fails
                // true = set pipeline to UNSTABLE, false = don't
                // Requires SonarQube Scanner for Jenkins 2.7+
                waitForQualityGate(abortPipeline: true)
            }
        }
    }
    stage('Publication') {
      steps {
        nexusArtifactUploader(artifacts: [
          			[artifactId: 'jpetstore', classifier: 'debug', file: 'target/jpetstore.war', type: 'war']
                        
                   ], credentialsId: 'nexus', groupId: 'monJPetStore', nexusUrl: 'localhost:8081/', nexusVersion: 'nexus3', protocol: 'http', repository: 'maven-snapshots', version: '1.0-SNAPSHOT')
        }
      }
    }
  }