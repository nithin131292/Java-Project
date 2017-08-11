pipeline{

  agent none


  stages{
    stage('Unit Tests')
    {
      agent {
        label 'apache'
      }
      steps{
        sh 'ant -f test.xml -v'
        junit 'reports/result.xml'
      }
    }
    stage('build'){
      agent {
        label 'apache'
      }

      steps {
        sh 'ant -f build.xml -v'
      }

    }

    stage('deploy'){
      agent {
        label 'apache'
      }
      steps{
        sh "cp dist/rectangle_${env.BUILD_NUMBER}.jar /var/www/html/rectangles/all"
      }
    }

   stage("Running on centos")
   {
     agent {
       label 'CentOs'
     }
     steps{
       sh "wget http://nithingudala1.mylabserver.com/rectangles/all/rectangle_${env.BUILD_NUMBER}.jar"
       sh "java -jar rectangle_${env.BUILD_NUMBER}.jar 3 3"
     }
   }
   stage("Promote to green"){
     agent {
       label 'apache'
     }
     when{
       branch 'development'
     }
     steps{
       sh "cp /var/www/html/rectangles/all/rectangle_${env.BUILD_NUMBER}.jar /var/www/html/rectangles/green/ "
     }
   }
 }
}
