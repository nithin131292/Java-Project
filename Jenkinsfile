pipeline{

  agent none

 environment {
   MAJOR_VERSION = 1
 }
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
        sh "cp dist/rectangle_${env.MAJOR_VERSION}.${env.BUILD_NUMBER}.jar /var/www/html/rectangles/all/${env.BRANCH_NAME}/"
      }
    }

   stage("Running on centos")
   {
     agent {
       label 'CentOs'
     }
     steps{
       sh "wget http://nithingudala1.mylabserver.com/rectangles/all/${env.BRANCH_NAME}/rectangle_${env.MAJOR_VERSION}.${env.BUILD_NUMBER}.jar"
       sh "java -jar rectangle_${env.MAJOR_VERSION}.${env.BUILD_NUMBER}.jar 3 3"
     }
   }
   stage("Promote to green"){
     agent {
       label 'apache'
     }
     when{
       branch 'master'
     }
     steps{
       sh "cp /var/www/html/rectangles/all/${env.BRANCH_NAME}/rectangle_${env.MAJOR_VERSION}.${env.BUILD_NUMBER}.jar /var/www/html/rectangles/green/rectangle_${env.MAJOR_VERSION}.${env.BUILD_NUMBER}.jar "
     }
   }
   stage("Promote Development Branch to master"){

     agent {
       label 'apache'
     }
     when{
       branch 'development'
     }
     steps{
       echo "Stashing any local changes"
       sh 'git stash'
       echo "checking out development branch"
       sh 'git checkout development'
       sh 'git pull origin development'
       sh 'git checkout master'
       sh 'git pull origin master'
       echo "Merging development into master branch"
       sh 'git merge development'
       echo 'pushing to origin master'
       sh 'git push origin master'
       echo 'Tagging the Release'
       sh "git tag rectangle-${env.MAJOR_VERSION}.${env.BUILD_NUMBER}"
       sh "git push origin rectangle-${env.MAJOR_VERSION}.${env.BUILD_NUMBER}"
     }
   }
 }
}
