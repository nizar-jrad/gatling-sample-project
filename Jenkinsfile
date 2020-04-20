def pom, isMultiModuleBuild, pomModule
pipeline {
    agent none

    stages {
        stage('Checkout') {
            agent { label 'linux-performance-testing-1' }
            steps {
                checkout([
                    $class: 'GitSCM',
                    branches: [[name: 'refs/heads/master']],
                    doGenerateSubmoduleConfigurations: false,
                    extensions: [[$class: 'RelativeTargetDirectory', relativeTargetDir: baseline.dir]],
                    submoduleCfg: [],
                    userRemoteConfigs: [[url: baseline.project]]
                ])
            }
        }

        stage("Build Maven") {
            agent { label 'linux-performance-testing-1' }
            steps {
                sh 'mvn -B clean package'
            }
        }

        stage('Run gatling tests') {
            agent { label 'linux-performance-testing-1' }
            steps {
                script {
                    sh 'mvn gatling:test -Dgatling.simulationClass=computerdatabase.BasicSimulation'
                    }
                }
            }
            post {
                always {
                    gatlingArchive()
                }
            }
        }
    }
}