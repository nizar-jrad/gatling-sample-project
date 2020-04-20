node('jenkins_slave') {
        stage ('SCM_Checkout') {
           git branch: 'master', credentialsId: 'user', url: ''
            }

        stage ('build') {
            sh '/opt/maven/bin/mvn gatling:test -Dgatling.simuationClass=ee.ee_globale' 
                        }

        stage ('Archive') {
         gatlingArchive()
         }  

}