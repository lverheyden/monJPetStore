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
  }
}