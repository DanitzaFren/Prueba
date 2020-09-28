#!/usr/bin/groovy

node {
  // If you are having issues with your project not getting updated, 
  // try uncommenting the following lines.
  //stage 'Checkout'
  //checkout scm
  //sh 'git submodule update --init --recursive'

  stage 'Test'
  // Invoke Django's tests
  sh 'source MYVENV/Scripts/activate && python ./manage.py runtests'
}