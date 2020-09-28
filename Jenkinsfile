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
        stage('Test') {
            agent {
                docker {
                    // Este parámetro de imagen descarga la imagen de Docker qnib: pytest y ejecuta esta imagen como
                    // contenedor separado. El contenedor pytest se convierte en el agente que Jenkins usa para ejecutar la prueba
                    // etapa de su proyecto Pipeline.
                    image 'qnib/pytest'
                }
            }
            steps {
                // Este paso sh ejecuta el comando py.test de pytest en sources / test_calc.py, que ejecuta un conjunto de
                // pruebas unitarias (definidas en test_calc.py) en la función add2 de la biblioteca "calc".
                // La opción --junit-xml test-reports / results.xml hace que py.test genere un informe XML JUnit,
                // que se guarda en test-reports / results.xml
                sh 'py.test --verbose --junit-xml test-reports/results.xml app/test.py'
            }
            post {
                always {
                    // Este paso junit archiva el informe JUnit XML (generado por el comando py.test anterior) y
                    // expone los resultados a través de la interfaz de Jenkins.
                    // La condición de la sección de publicación siempre que contiene este paso junit garantiza que el paso sea
                    // siempre se ejecuta al finalizar la etapa de prueba, independientemente del resultado de la etapa.
                    junit 'test-reports/results.xml'
                }
            }
        }
        stage('Deliver') {
                    agent any

                    // Este bloque de entorno define dos variables que se utilizarán más adelante en la etapa 'Entregar'.
                    environment {
                        VOLUME = '$(pwd)/sources:/src'
                        IMAGE = 'cdrx/pyinstaller-linux:python2'
                    }
                    steps {
                        // Este paso de directorio crea un nuevo subdirectorio nombrado por el número de compilación.
                        // El programa final será creado en ese directorio por pyinstaller.
                        // BUILD_ID es una de las variables de entorno Jenkins predefinidas.
                        // Este paso de desinstalación restaura el código fuente de Python y el byte compilado
                        // archivos de código (con extensión .pyc) del alijo previamente guardado. imagen]
                        // y ejecuta esta imagen como un contenedor separado.
                        dir(path: env.BUILD_ID) {
                            unstash(name: 'compiled-results')
                            // Este paso sh ejecuta el comando pyinstaller (en el contenedor PyInstaller) en su aplicación Python simple.
                            // Esto agrupa su aplicación Python add2vals.py en un solo archivo ejecutable independiente
                            // y envía este archivo al directorio del espacio de trabajo dist (dentro del directorio de inicio de Jenkins).
                            sh "docker run --rm -v ${VOLUME} ${IMAGE} 'pyinstaller -F apps.py'"
                        }
                    }
                    post {
                        success {
                            // Este paso archiveArtifacts archiva el archivo ejecutable independiente y expone este archivo
                            // a través de la interfaz de Jenkins.
                            archiveArtifacts "${env.BUILD_ID}/app/dist/apps"
                            sh "docker run --rm -v ${VOLUME} ${IMAGE} 'rm -rf build dist'"
                        }
                    }
        }
    }
}
