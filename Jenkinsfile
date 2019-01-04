peline {
    agent { label 'ldap'}
	
	stages {
		stage('Permission') {
			steps {
				echo '\'path "auth/approle/role/java-example/secret-id" {   capabilities = ["read","create","update"] }\' > ./config/vault/policies/jenkins_policy.hcl'
				sh 'vault policy-write jenkins ./config/vault/policies/jenkins_policy.hcl'
			}
		}
		
		stage('Create a Role') {
			steps {
				echo 'echo \'path "secret/hello" {   capabilities = ["read", "list"] }\' > ./config/vault/policies/java-example_policy.hcl'
				sh 'vault policy-write java-example ./config/vault/policies/java-example_policy.hcl'
				sh 'vault auth-enable approle'
				sh '''vault write auth/approle/role/java-example \\
					> secret_id_ttl=60m \\
					> token_ttl=60m \\
					> token_max_tll=120m \\
					> policies="java-example"'''
				sh 'vault read auth/approle/role/java-example'
			}
		}
		
		stage('Generate Role ID') {
			steps {
				sh 'vault read auth/approle/role/java-example/role-id'
			}
		}
		
		stage('Generate Jenkins Token ID') {
			steps {
				sh 'vault token-create -policy=jenkins'
			}
		}
		
		stage('Write a Secret') {
			steps {
				sh 'vault write secret/hello value="You\'ve Successfully retrieved a secret from Hashicorp Vault"'
			}
		}
	}
}
