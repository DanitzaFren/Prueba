pipeline {
    agent none 
    stages {
        stage('Build') { 
            agent {
                docker {
                    image 'python:2-alpine' 
                }
            }
            steps {
                sh 'python -m py_compile app/admin.py app/apps.py app/models.py app/tests.py app/views.py sitio/settings.py sitio/urls.py sitio/wsgi.py'
                stash(name: 'compiled-results', includes: 'apps/*.py* sitio/*.py*') 
            }
        }
    }
}