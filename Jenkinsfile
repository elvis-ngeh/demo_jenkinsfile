node{
    def mavenHome = tool name: 'maven3.8.6'
    stage('1clonecode'){
        git "https://github.com/elvis-ngeh/maven-web-application"
    }
    stage('2test&build'){
        sh "${mavenHome}/bin/mvn clean package "
    }
    stage('3codeQuality'){
        sh "echo 'running code Quality now'"
        sh "${mavenHome}/bin/mvn clean sonar:sonar"
    }
    stage('4uploadArtifacts'){
        sh "echo 'uploading arifacts now '"
        sh "${mavenHome}/bin/mvn deploy"
    }
    stage('5UATtestonTomcat'){
        sh "echo 'Deploying for UAT test'"
        deploy adapters: [tomcat9(credentialsId: 'tomcat-credentials', path: '', url: 'http://10.0.1.148:8080')], contextPath: null, war: 'target/*war'
    }
    stage('6approvalGate'){
        sh "echo 'review and approve if satisfactory'"
        timeout(time: 5 ,unit: 'DAYS'){
        input message: 'application ready for deployment, pls review & approve'
        }
    }
    stage('7deploy2Prod'){
        sh "echo 'Deploying code to prod now '"
        deploy adapters: [tomcat9(credentialsId: 'tomcat-credentials', path: '', url: 'http://10.0.1.148:8080')], contextPath: null, war: 'target/*war'
    }
    stage('8notification'){
        emailext body: 'Thanks for the sacrifices team ', subject: 'code deployed ', to: 'elvis.bantar@gmail.com'
    }
}
