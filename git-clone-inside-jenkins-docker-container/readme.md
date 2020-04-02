docker run -t --name jenkins -p 8080:8080 -v /var/jenkins_home:/var/jenkins_home jenkins/jenkins:lts

copy my id_rsa to /var/jenkins_home/.ssh/

ssh-keyscan github.com >> $HOME/.ssh/known_hosts

restart jenkins container

and create a pipeline job:

node('master') { 

    git params.git_url
    sh "ls -la"

}
