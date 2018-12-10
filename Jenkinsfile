pipeline {
  agent any
  stages {
    stage('Start') {
      steps {
        echo ' start execution'
        bat 'mvn clean'
      }
    }
    stage('Parallel Execution') {
      parallel {
        stage('UI Test') {
          steps {
            bat 'mvn clean test -Dsurefire.suiteXmlFiles=src/test/resources/testng.xml'
          }
        }
        stage('API Test') {
          steps {
            bat 'ant -buildfile "build.xml"'
          }
        }
      }
    }
    stage('.xml report results') {
      steps {
        junit 'target/surefire-reports/junitreports/TEST-*.xml'
      }
    }
    stage('Convert to HTML') {
      steps {
        bat 'ant -buildfile "build-junit_html_report.xml"'
      }
    }
    stage('Send e-mail with results report') {
      steps {
        emailext(subject: 'Pipeline Test Results', body: '${FILE,path="target/surefire-reports/junitreports/html/finalreport/TESTS-TestSuites.html"} <br> <br> <br> ${FILE,path="results/html/JMeterResults.html"}', to: 'dragana.todorchevska@interworks.com.mk')
      }
    }
    stage('End') {
      steps {
        echo 'Pipeline executed successfully!'
      }
    }
  }
}