pipeline {
    agent any
    
    stages {
        stage('Checkout') {
            steps {
                checkout([$class: 'GitSCM',
                          branches: [[name: '*/master']],
                          userRemoteConfigs: [[url: 'https://github.com/betillox/challenge-cicd-terraform']]])
            }
        }

	stage('Approval') {
		when {
            branch "prod"
        }
    steps {
        script {
            def approver = emailextrecipients([[$class: 'CulpritsRecipientProvider']])
            emailext (
                to: 'eumanachav@ucreativa.com',
                subject: 'Approval needed',
                body: 'Please approve the deployment of the application.',
                recipientProviders: [[$class: 'CulpritsRecipientProvider']],
                replyTo: '$DEFAULT_REPLYTO',
                attachLog: true,
                mimeType: 'text/html'
            )
            timeout(time: 7, unit: 'DAYS') {
                // Wait for approval, timeout after 7 days
                input message: 'Approve or Reject?', ok: 'Approve', submitter: 'eumanachav@ucreativa.com'
            }
        }
    }
}

	stage('Terraform Init') {
            steps {
	
                sh 'terraform init'
		    
            }
        }
        stage ("terraform fmt") {
            steps {
		    
                sh 'terraform fmt'
		
            }
        }
        stage ("terraform validate") {
            steps {
		    
                sh 'terraform validate'
		    
            }
        }
        stage('Terraform Plan') {
            steps {
		    
                sh 'terraform plan'
		    
            }
        }
	
	stage('Terraform Apply') {
		when {
            branch "prod"
        }
	    steps {
		    
	    	sh 'terraform apply --auto-approve'
		    
            }
	}	

 	stage('Terraform Graph') {
		when {
            branch "prod"
        }
	    steps {
		    
	    	sh 'terraform graph'
		    
            }
	}	

     }
  }
