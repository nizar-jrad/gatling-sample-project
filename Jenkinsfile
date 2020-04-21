//def pom, isMultiModuleBuild, pomModule
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
                    extensions: [],
                    submoduleCfg: [],
                    userRemoteConfigs: [[url: 'https://github.com/nizar-jrad/gatling-sample-project.git']]
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
                script{
                    try {
                        sh 'mvn gatling:test'
                    }catch (Exception e) {
                                echo("Le build a échoué à cause de gatling")
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