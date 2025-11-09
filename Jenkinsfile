pipeline {
  agent any

  environment {
    VIRTUAL_ENV = 'venv'
  }

  stages {

    stage('Setup') {
      steps {
        bat """
          if not exist %WORKSPACE%\\%VIRTUAL_ENV% (python -m venv %VIRTUAL_ENV%)
          call %VIRTUAL_ENV%\\Scripts\\activate
          python -m pip install --upgrade pip
          python -m pip install -r requirements.txt
          python -m pip install coverage bandit
        """
      }
    }

    stage('Lint') {
      steps {
        bat """
          call %VIRTUAL_ENV%\\Scripts\\activate
          flake8 app.py
        """
      }
    }

    stage('Test') {
      steps {
        bat """
          call %VIRTUAL_ENV%\\Scripts\\activate
          python -m unittest discover -s tests -p "test_*.py"
        """
      }
    }

 
    stage('Coverage') {
      steps {
        bat """
          call %VIRTUAL_ENV%\\Scripts\\activate
          coverage run -m unittest discover -s tests -p "test_*.py"
          coverage report
          coverage html
        """
      }
    }

    
    stage('Security Scan') {
  steps {
    bat """
      chcp 65001 >NUL
      set PYTHONIOENCODING=utf-8
      call %VIRTUAL_ENV%\\Scripts\\activate
      bandit -r . -f json -o bandit.json
    """
  }
}


    stage('Deploy') {
      steps {
        echo "Deploying application..."
      }
    }
  }

  post {
  always {
    archiveArtifacts artifacts: 'bandit.json, htmlcov/**', fingerprint: true
    cleanWs()
  }
}

