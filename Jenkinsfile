node {
    
    def mvnHome = tool 'M3'
    
    stage ("Check Out") {
        checkout scm
    }
        
    stage ("Build") {
        sh "${mvnHome}/bin/mvn -B -Dmaven.test.failure.ignore cobertura:cobertura"
    }
        
    stage ("Archieve Artifacts") {
        archiveArtifacts artifacts: '**/target/*.jar', fingerprint: true
    }
        
    stage ("Publish Test Results") {
        step([$class: 'JUnitResultArchiver', testResults: '**/target/surefire-reports/TEST-*.xml'])
    }
    
    stage ("Publish Code Coverage Report") {
        // publish html
        // snippet generator doesn't include "target:"
        // https://issues.jenkins-ci.org/browse/JENKINS-29711.
        publishHTML (target: [
        allowMissing: false,
        alwaysLinkToLastBuild: false,
        keepAll: true,
        reportDir: 'target/site/cobertura',
        reportFiles: 'index.html',
        reportName: "Cobertura Report"
    ])
    }
    
    stage ("Upload to Artifcatory") {
      //  sh "${mvnHome}/bin/mvn artifactory:publish -Dusername=user -Dpassword=APAJedk3QV9SvPdupHQ2B8VrMrq"
    }
    
    //stage ("Release") {
    //    sh "${mvnHome}/bin/mvn release:clean -X release:prepare release:perform"        
    //}
}