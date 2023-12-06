String buildShell = "${params.buildCommand}"

def jarName

node("master"){
    stage("checkout"){
        checkout scm
    }
    
    stage("build"){
        def mvnHome = tool 'Maven'
        sh " ${mvnHome}/bin/mvn ${buildCommand}"
        
        jarName = sh returnStdout: true, script: "cd target && ls *.jar"
    }

}
