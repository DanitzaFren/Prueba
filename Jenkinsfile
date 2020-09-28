pipeline {
        // El parámetro Ninguno en la sección del agente significa que no se asignará ningún agente global para todo el Pipeline
        // ejecución y que cada directiva de etapa debe especificar su propia sección de agente.
    agent none
    stages {
        stage('Build') {
            agent {
                docker {
                    // Este parámetro de imagen (del parámetro de ventana acoplable de la sección del agente) descarga python: 2-alpine
                    // Imagen de Docker y ejecuta esta imagen como un contenedor separado. El contenedor de Python se convierte en
                    // el agente que Jenkins usa para ejecutar la etapa de compilación de su proyecto de canalización.
                    image 'python:2-alpine'
                }
            }
            steps {
                // Este paso sh ejecuta el comando Python para compilar su aplicación y
                // su biblioteca calc en archivos de código de bytes, que se colocan en el directorio del espacio de trabajo de fuentes
                sh 'python -m py_compile app/admin.py app/apps.py app/models.py app/tests.py app/views.py sitio/settings.py sitio/urls.py sitio/wsgi.py'
                // Este paso de almacenamiento guarda el código fuente de Python y los archivos de código de bytes compilados de las fuentes
                // directorio del espacio de trabajo para usar en etapas posteriores.
                stash(name: 'compiled-results', includes: 'app/*.py*')
            }
        }
     
    }
}
