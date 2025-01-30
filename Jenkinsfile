pipeline {
    agent any
    stages {
        stage('Deploy') {
            steps {
                sshagent(credentials: ['ec2-ssh-key']) {
                    sh '''
                        ssh -o StrictHostKeyChecking=no ec2-user@your-instance-private-ip "
                            mkdir -p ~/app
                            rm -rf ~/app/*
                        "
                        scp -r ./* ec2-user@your-instance-private-ip:~/app
                        ssh ec2-user@your-instance-private-ip "
                            cd ~/app
                            python3 -m venv venv
                            source venv/bin/activate
                            pip install -r requirements.txt
                            nohup python app.py > output.log 2>&1 &
                        "
                    '''
                }
            }
        }
    }
}

