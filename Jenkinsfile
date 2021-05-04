pipeline {
  agent any
  environment {
    APPSYSID = '85fc655bdb3be410ad2b2ab74b961931'
    BRANCH = "${Jenkinsfile}"
    CREDENTIALS = 'ServiceNow'
    DEVENV = 'https://highmetricllcdemo10.service-now.com/'
    TESTENV = 'https://highmetricllcslsdemo16.service-now.com/navpage.do'
    PRODENV = 'https://highmetricllcdemo13.service-now.com/'
    TESTSUITEID = 'b1ae55eedb541410874fccd8139619fb'
  }
  stages {
    stage('Build') {
      when {
        not {
          branch 'master'
        }
      }
      steps {
        snApplyChanges(appSysId: "${APPSYSID}", branchName: "${BRANCH}", url: "${DEVENV}", credentialsId: "${CREDENTIALS}")
        snPublishApp(credentialsId: "${CREDENTIALS}", appSysId: "${APPSYSID}", obtainVersionAutomatically: true, url: "${DEVENV}")
      }
    }
    stage('Test') {
      when {
        not {
          branch 'master'
        }
      }
      steps {
        snInstallApp(credentialsId: "${CREDENTIALS}", url: "${TESTENV}", appSysId: "${APPSYSID}")
        snRunTestSuite(credentialsId: "${CREDENTIALS}", url: "${TESTENV}", testSuiteSysId: "${TESTSUITEID}", withResults: true)
      }
    }
    stage('Deploy to Prod') {
      when {
        branch 'master'
      }
      steps {
        snInstallApp(credentialsId: "${CREDENTIALS}", url: "${PRODENV}", appSysId: "${APPSYSID}")
      }
    }
  }
}
