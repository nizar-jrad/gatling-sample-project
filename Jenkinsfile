def pom, isMultiModuleBuild, pomModule
pipeline {
    agent none

    stages {
        stage('Checkout') {
            agent { label 'linux-performance-testing-1' }
            steps {
                checkout([$class: 'GitSCM',
                  branches: scm.branches,
                  extensions: scm.extensions + [[$class: 'WipeWorkspace']],
                  userRemoteConfigs: scm.userRemoteConfigs])
            }
        }

        stage('Run gatling tests') {
            agent { label 'linux-performance-testing-1' }
            steps {
                script {
                    sh 'mvn gatling:test -Dgatling.simulationClass=computerdatabase.BasicSimulation -Dgatling.useOldJenkinsJUnitSupport=true'
                    }
                }
            }
            post {
                always {
                    junit 'target/gatling/assertions-*.xml'
                }
            }
        }

        stage ('Archive') {
            gatlingArchive()
        }

    }
}