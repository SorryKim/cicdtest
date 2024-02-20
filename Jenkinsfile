pipeline {
  agent any
  stages {
    stage('git scm update') {
      steps {
        git url: 'https://github.com/SorryKim/cicdtest.git', branch: 'main'
      }
    }
    stage('docker build and push') {
      steps {
        sh '''
        sudo docker build -t sorrykim/cicdtest:green .
        sudo docker push sorrykim/cicdtest:green
        '''
      }
    }
    stage('deploy kubernetes') {
      steps {
        sh '''
        ansible master -m shell -a 'export KUBECONFIG=/etc/kubernetes/admin.conf && kubectl create deployment pl-bulk-prod --image=sorrykim/cicdtest:green'
        ansible master -m shell -a 'export KUBECONFIG=/etc/kubernetes/admin.conf && kubectl expose deployment pl-bulk-prod --type=LoadBalancer --port=80 --target-port=80 --name=pl-bulk-prod1'
        '''
      }
    }
  }
}
