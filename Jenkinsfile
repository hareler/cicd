node {
    def server = Artifactory.server('http://mill.jfrog.team:12553/artifactory,'admin','Jfrogtest1!')
    def rtMaven = Artifactory.newMavenBuild()
    def buildInfo

    stage ('Clone') {
        git url: 'https://github.com/JFrog/project-examples.git'
    }

    stage ('Artifactory configuration') {
        rtMaven.tool = 'mvn381' 
        rtMaven.deployer releaseRepo: 'libs-local', snapshotRepo: 'libs-local', server: server
        rtMaven.resolver releaseRepo: 'libs-virtual', snapshotRepo: 'libs-virtual', server: server
        buildInfo = Artifactory.newBuildInfo()
    }

    stage ('Exec Maven') {
        rtMaven.run pom: 'maven-example/pom.xml', goals: 'clean install', buildInfo: buildInfo
    }

    stage ('Publish build info') {
        server.publishBuildInfo buildInfo
    }
}

