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
	stage('Publication') {
      steps {
        nexusArtifactUploader {
		nexusVersion('nexus3')
		protocol('http')
		nexusUrl('localhost:8081/')
		groupId('monJPetStore')
         version('1.0')
          repository('maven-snapshots')
          credentialsId('nexus')
          artifact {
              artifactId('jpetstore')
              type('war')
              classifier('debug')
              file('target/jpetstore.war')
          }
      }
	 }
  }
}