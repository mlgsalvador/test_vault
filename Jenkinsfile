pipeline {
  agent { label 'ldap'}
stages {  
  stage('Integration Tests') {
      steps {
        withCredentials([string(credentialsId: 'VAULT_ROLE_ID', variable: 'ROLE_ID'),string(credentialsId: 'JENKINS_VAULT_TOKEN', variable: 'VAULT_TOKEN')]) {
        sh '''
  set +x
  export VAULT_ADDR=http://100.26.111.13:8200
  export VAULT_SKIP_VERIFY=true
  export SECRET_ID=$(vault write -field=secret_id -f auth/approle/role/role-example/secret-id)
  export JOB_VAULT_TOKEN=$(vault write -field=token auth/approle/login role_id=${ROLE_ID}   secret_id=${SECRET_ID})
vault read -address=http://100.26.111.13:8200 secret/hello 
        '''
    }
   }
  }
 }
}