#!groovy
@Library("ECM@master") _

pipeline{
	agent {
		label {
			lable "centos7"
			customWorkspace "/var/lib/jenkins/workspace/sceptre-skeleton/${BRANCH_NAME}"
		}
	}
	
	options {
		buildDiscarder(
			logRotator(
				numToKeepStr: "5"
			)
		)
	}
	
	environment {
		
	}
	
	parameters {
		choice(
			name: "ENVIRONMENT",
			choices: ["dev-private", "test", "ote", "prod"],
			description: "Choose the Environment you want to deploy"
		)
		choice(
			name: "SUB_ENVIRONMENT",
			choices: ["ecomm", "ICP", "PCI"],
			description: "Choose the Sub Environment you want to deploy"
		)
		string(
			name: "STACK_NAME",
			defaultValue: "",
			description: "Enter the name of Stack"
		)
		string(
			name: "AWS_REGION",
			defaultValue: "us-east-1",
			description: "Enter AWS Region name"
		)
		string(
			name: "ALLOWED_VPCES",
			defaultValue: "",
			description: "Enter the name of Allowed VPC end points"
		)
		string(
			name: "ALLOWED_PRINCIPALS_LIST",
			defaultValue: "",
			description: "Enter the Allowed Principal list"
		)		
	}
	
	stages {
		stage("Prepare Environment"){
			
		}
		
		stage("Verify get-caller-identity"){
			
		}
		
		stage("Sceptre launch environment"){
			when { equals expected; "${PROJECT_NAME}", actual: "${currentBuild.fullProjectName}" }
			steps{
				withAwsOkta(
					oktaOrganization: "{GODADDY_ORG}",
					credentialsId: "{AWS_AD_USER_ID}",
					awsApp: "{AWS_APP}",
					roleName: "{AWS_ROLE_ARN}"
				) {
					sh """ set -eo pipefail;
					git clean -fd;
					sed -i 's/<STACK_NAME>/${STACK_NAME}/g'; 's/<AWS_REGION>/${AWS_REGION}/g'; 's/<APP_NOUN>/${APP_NOUN}/g'; 's/<ALLOWED_VPCES>/${ALLOWED_VPCES}/g'; 's/<ALLOWED_PRINCIPALS_LIST>/${ALLOWED_PRINCIPALS_LIST}/g'; 's/<ENVIRONMENT>/${ENVIRONMENT}/g'; 's/<SUB_ENVIRONMENT>/${SUB_ENVIRONMENT}/g'; config/config.yaml;
					sceptre launch dev-private/us-east-1 -y
				}
			}
		}
	}
	
	post {
		
	}
	
}