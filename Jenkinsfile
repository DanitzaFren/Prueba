#!/usr/bin/groovy

node {
  // If you are having issues with your project not getting updated, 
  // try uncommenting the following lines.
  //stage 'Checkout'
  //checkout scm
  //sh 'git submodule update --init --recursive'

  stage 'Update Python Modules'
  // Create a virtualenv in this folder, and install or upgrade packages
  // specified in requirements.txt; https://pip.readthedocs.io/en/1.1/requirements.html
  sh 'source MYVENV/Scripts/activate && pip install --upgrade -r requirements.txt'
  

  stage 'Test'
  // Invoke Django's tests
  sh 'source MYVENV/Scripts/activate'
}