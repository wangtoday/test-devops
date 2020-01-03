pipeline {
   agent any
   environment{
      GOOS="linux"
      CGO_ENABLED="0"
      GIN_MODE="release"
   }
   tools{
       go 'go'
   }
   stages {
      stage('Checkout Code') {
         steps {
           git credentialsId: 'b0e35f6a-28ae-4f2d-a04e-cc2d7ded815f', url: 'git@github.com:xyctruth/bookbook.git'
         }
      }

      stage('Set Env') {
         steps {
            script{
                out=sh(script:"ls /lib64",returnStatus:true)
                if(out == 2){
                    sh 'mkdir /lib64  && ln -s /lib/libc.musl-x86_64.so.1 /lib64/ld-linux-x86-64.so.2'
                 }
            }
         }
      }

      stage('Build Code') {
         steps {
           sh 'cd ./services/book && go build .'
         }
      }

      stage('Deploy') {
         steps {
            sh 'ps |grep ./book |awk \'{print $1}\'|xargs kill -9 || true'
            sh 'cd ./services/book  && JENKINS_NODE_COOKIE=dontKillMe  nohup ./book & '
         }
      }
   }
}
