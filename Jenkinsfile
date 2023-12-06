String buildShell = "${params.buildShell}"
String targetHosts = "${params.targetHosts}"
String targetDir = "${params.targetDir}"
String serviceName = "${params.serviceName}"
String user = "${params.user}"
String port = "${params.port}"
def jarName

node("master"){
    stage("checkout"){
        checkout scm
    }
    
    stage("build"){
        def mvnHome = tool 'Maven'
        sh " ${mvnHome}/bin/mvn ${buildShell}"
        
        jarName = sh returnStdout: true, script: "cd target && ls *.jar"
    }

}
