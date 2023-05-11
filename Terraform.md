terraform

'''
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

'''
