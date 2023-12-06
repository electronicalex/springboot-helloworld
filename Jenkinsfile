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
        jarName = jarName - "\n"
        sh "mkdir -p /srv/salt/${serviceName} && mv service.sh target/${jarName} /srv/salt/${serviceName} "
    }
    
    stage("deploy"){
        sh " salt ${targetHosts} cmd.run ' rm -fr  ${targetDir}/*.jar '"
        sh " salt ${targetHosts} cp.get_file salt://${serviceName}/${jarName}  ${targetDir}/${jarName} mkdirs=True"
        sh " salt ${targetHosts} cp.get_file salt://${serviceName}/service.sh  ${targetDir}/service.sh mkdirs=True"
        sh " salt ${targetHosts} cmd.run 'chown ${user}:${user} ${targetDir} -R '"
        sh " salt ${targetHosts} cmd.run 'su - ${user} -c \"cd ${targetDir} &&  sh service.sh stop\" ' "
        sh " salt ${targetHosts} cmd.run 'su - ${user} -c \"cd ${targetDir} &&  sh service.sh start ${jarName} ${port} ${targetDir}\" ' "
    }

}
