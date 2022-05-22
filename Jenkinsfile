pipeline {
    agent { label 'JDK11' }
    options { 
        timeout(time: 1, unit: 'HOURS')
    }
    triggers {
        cron('0 * * * *')
    }
    stages {
        stage('Source Code') {
            steps {
                git url: 'https://github.com/GitPracticeRepo/myspringpetclinic.git', 
                branch: 'sprint_relase_1'
            }
            
        }
        stage('Artifactory-Configuration') {
            steps {
                rtMavenDeployer (
                    id: 'spc-deployer',
                    serverId: 'JFROG_INSTANCE',
                    releaseRepo: 'qtecomm-libs-release-local',
                    snapshotRepo: 'qtecomm-libs-snapshot-local',
                    
                )
            }
        }
        stage('Build the Code and sonarqube-analysis') {
            steps {
                rtMavenRun (
                    // Tool name from Jenkins configuration.
                    tool: 'MVN_DEFAULT',
                    pom: 'pom.xml',
                    goals: 'clean install',
                    // Maven options.
                    deployerId: 'spc-deployer',
                )
                
            }
        }
        stage('reporting') {
            steps {
                junit testResults: 'target/surefire-reports/*.xml'
            }
        }
       
        
    }
   
}