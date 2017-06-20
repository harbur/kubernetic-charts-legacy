node {
    stage('checkout') {
      deleteDir()
       checkout scm
       if ("${env.BRANCH_NAME}" != 'null') {
           sh "git checkout -b ${env.BRANCH_NAME}"
       }
       gitCommit = sh(returnStdout: true, script: 'git rev-parse HEAD').trim()
    }
    stage('build') {
        build_env = docker.build("local/build_env")
    }
    build_env.inside {
      stage('test') {
        sh '''
          helm init --client-only
          helm lint charts/*
        '''
        charts = sh(returnStdout: true, script: "git diff-tree --no-commit-id --name-only -a -r ${gitCommit} | cut -d\'/\' -f1,2 | uniq | grep \"^charts\" | xargs -n1 -r").trim()
      }
      if (charts) {
        stage('package') {
          // Package Charts
          sh """
            helm init --client-only
            helm package --destination docs ${charts}
            helm repo index --url https://harbur.github.io/kubernetic-charts/ docs
          """

          // Push to Git repository
          withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'github', passwordVariable: 'PASSWORD', usernameVariable: 'USERNAME']]) {
            sh """
              # Configure Git CLI
              git config --global user.email jenkins@cloud
              git config --global user.name jenkins

              # Push Packages to Git
              git commit -am "packaging charts"
              git push -u https://${env.USERNAME}:${env.PASSWORD}@github.com/harbur/kubernetic-charts.git ${env.BRANCH_NAME}
            """
          }
        }
      }
    }
}

