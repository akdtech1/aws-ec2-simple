pipeline {
  agent any

  environment {
    AWS_ACCESS_KEY_ID = credentials('AWS_ACCESS_KEY_ID')
    AWS_SECRET_ACCESS_KEY = credentials('AWS_SECRET_ACCESS_KEY')
  }

  stages {
    stage('Terraform Init'){
      steps {
        sh '/usr/bin/terraform init'

      }
    }

    stage('Terraform Plan'){
      steps {
        sh '/usr/bin/terraform plan -out testplan'
        sh '/usr/bin/terraform show -json testplan > testplan.json'
        
      }
    }

   
    stage('Checkov Scan'){
      steps {
        catchError(buildResult: 'SUCCESS', message: 'IAC Misconfiguratios found', stageResult: 'UNSTABLE')
                   {
                     sh 'sudo /home/ubuntu/.local/bin/checkov -f testplan.json'
                     
                   }
        
      }
    }


      stage('Terraform Apply'){
      steps {
        sh '/usr/bin/terraform apply -auto-approve'
        
      }
    }

      stage('Terraform Destroy'){
      steps {
        sh '/usr/bin/terraform plan -out destroyplan -destroy'
        sh '/usr/bin/terraform apply destroyplan'
        
      }
    }
                   }
                   }
