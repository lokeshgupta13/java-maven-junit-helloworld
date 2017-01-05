node {
    
    def mvnHome = tool 'M3'
    
    stage ("Check Out") {
        checkout scm
    }
        
    stage ("Build") {
        // we want to pick up the version from the pom
        def pom = readMavenPom file: 'pom.xml'
        def version = pom.version.replace("-SNAPSHOT", ".${currentBuild.number}")
        echo "${version}"
        sh "${mvnHome}/bin/mvn versions:set versions:update-child-modules -DnewVersion=${version}" 
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
        sh "${mvnHome}/bin/mvn deploy -Dartifactoryurl=http://54.200.248.158:8081/artifactory -Dusername=admin -Dpassword=AP85ANZPE3J1D2stR4k6ZHKQMvE"
    }
    
    //stage ("Release") {
    //    sh "${mvnHome}/bin/mvn release:clean -X release:prepare release:perform"        
    //}
}
