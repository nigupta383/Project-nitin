pipeline {
   agent any
   environment {
      TF_DIR = "terraform"
      ANSIBLE_DIR = "ansible"
      ARM_CLIENT_ID=credentials('azure-client-id')
      ARM_CLIENT_SECRET=credentials('azure-client-secret')
      ARM_TENANT_ID=credentials('azure-tenant-id')
      ARM_SUBSCRIPTION_ID=credentials('azure-subscription-id')
   }
   
   stages {
      stage('Terraform init'){
        steps {
          sh '''cd ${TF_DIR}
             terraform init'''
         }
      }
       stage('Terraform Plan'){
        steps {
          sh '''cd ${TF_DIR}
             terraform plan -out=tfplan'''
         }
      }
       stage('Terraform Apply'){
        steps {
          sh '''cd ${TF_DIR}
             terraform apply tfplan'''
         }
      }
      stage('Run Ansible Playbook'){
        steps {
          ansiblePlaybook credentialsId: 'ansible', disableHostKeyChecking: true, inventory: 'terraform/inventory', playbook: 'ansible/webserver.yaml', vaultTmpPath: '' 
         }
      }
      stage('Terraform destroy'){
        steps {
          sh 'terraform destroy -auto-approve'
         }
      }
   }
}
