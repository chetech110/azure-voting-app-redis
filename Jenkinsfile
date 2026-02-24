pipeline {
   agent any

   stages {
      stage('Verify Branch') {
         steps {
            echo "$GIT_BRANCH"
         }
      }
      stage('Docker Build') {
         steps {
            sh(script: 'docker compose build')
         }
      }
      stage('Start App') {
         steps {
            sh(script: 'docker compose up -d')
         }
      }
      stage('Run Tests') {
         steps {
            sh '''
                    # Install Python tools & create venv
                    sudo apt-get update
                    sudo apt-get install -y python3-full python3-pip python3-venv
                    
                    # Setup virtual environment
                    python3 -m venv venv
                    . venv/bin/activate
                    pip install pytest
                    
                    # Run tests
                    pytest ./tests/test_sample.py -v --junitxml=test-results.xml
                '''
         }
         post {
            success {
               echo "Tests passed! :)"
            }
            failure {
               echo "Tests failed :("
            }
         }
      }
   }
   post {
      always {
         sh(script: 'docker compose down')
      }
   }
}
