pipeline {
  agent {
    docker {
      image 'python:3.11-slim'
}
}
  stages {

    stage('set up python environment') {
      steps {
        sh '''
        cd message
        python -m venv venv
        . venv/bin/activate
        pip install --upgrade pip
        pip install -r requirements.txt
        '''
}
}
    stage('run tests') {
      steps {
        sh '''
        cd message
        . venv/bin/activate
        pytest --html=report.html --self-contained-html
        '''
}
}
}
  post {
    always {
      archiveArtifacts artifacts: 'message/report.html', fingerprint: true
}
}
}
