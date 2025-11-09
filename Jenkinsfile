pipeline {
  agent any

  environment {
    VIRTUAL_ENV = 'venv'
  }

  stages {
    stage('Setup') {
      steps {
        bat """
          if not exist %WORKSPACE%\\%VIRTUAL_ENV% (
            python -m venv %VIRTUAL_ENV%
          )
          call %VIRTUAL_ENV%\\Scripts\\activate
          pip install --upgrade pip
          pip install -r requirements.txt
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

    stage('Deploy') {
      steps {
        echo "Deploying application..."
        // put real deploy steps here later
      }
    }
  }

  post {
    always {
      cleanWs()
    }
  }
}
