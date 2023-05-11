AWS Terraform

Infrastructure as code (IaC) tools allow you to manage infrastructure with configuration files rather than through a graphical user interface. IaC allows you to build, change, and manage your infrastructure in a safe, consistent, and repeatable way by defining resource configurations that you can version, reuse, and share.

Terraform is HashiCorp's infrastructure as code tool. It lets you define resources and infrastructure in human-readable, declarative configuration files, and manages your infrastructure's lifecycle. Using Terraform has several advantages over manually managing your infrastructure:

To deploy your infrastructure with terraform we use these steps:

Scope - Identify the infrastructure for your project.

Author - Write the configuration for your infrastructure.

Initialize - Install the plugins Terraform needs to manage the infrastructure.

Plan - Preview the changes Terraform will make to match your configuration.

Apply - Make the planned changes.

Pre-requisites (On mac)

- Have Terraform installed ( use homebrew to do this)
- Have AWS CLI (command line interface) installed
- Have your AWS Access keys and secrey keys (Supplied by the company or can be created on AWS cloud)
- Have an Account on for AWS credentials to log in and see your instance
- Have an `ami` running already on the AWS server cloud instance, we had `app` ami already there.

Building our infrastructure

Step 1

`On MacOs`

- Have your homebrew installed already within the git bash terminal, this is where we are going install our terraform application and use the official forumula for it.

- Install the HashiCorp tap, a repository of all our Homebrew packages, `$ brew tap hashicorp/tap`

- Install Terraform, `brew install hashicorp/tap/terraform`

- update to the latest version of Terraform `brew update`

- Run the upgrade command to download and use the latest Terraform version, `brew upgrade hashicorp/tap/terraform`

Step 2 

Now you should have Terraform installed, to check this we open a new terminal:

- `terraform -help` displays all available sub commands
- `terraform --version` displays the downloaded version of the terraform
```
Terraform v1.4.6
on darwin_amd64
```
Step 3 

With Terraform installed, you are ready to create your first infrastructure.

- You will provision an EC2 instance on Amazon Web Services (AWS).

Make sure you have `AWS CLI`, installed. To do this follow this link or alternatively follow the steps as below. 

- If on `MacOs` download this package `https://awscli.amazonaws.com/AWSCLIV2.pkg`
- Run your downloaded file and follow the on-screen instructions. You can choose to install the AWS CLI
- You may pick your desired location but we kept it within our 'local` but you may also have `usr` available also
- Use `which aws` then `aws --version` to check if the version has been installed.

Step 4 

Creating our AWS CLI

```

#Terraform script to create a service on the cloud
#Let's set up our cloud provider with terraform

# Who is the provider - AWS

# How to codify with terraform - syntax - name of the recourse/task{key = value
# Most commonly used commands - terraform init - terraform plan - terraform apply - terraform destroy
provider "aws" {
	region = "eu-west-1"
}
# let's create a service on AWS
# which service -EC2
resource "aws_instance" "app_instance" {
	# which AMI to use
	ami = "ami-0fb7109a40e90112d"
	instance_type = "t2.micro"
	# do you need the public IP
	associate_public_ip_address = true
	tags = { 
	  Name = "tech221_ruhal_terraform_app"

	}
}

```
