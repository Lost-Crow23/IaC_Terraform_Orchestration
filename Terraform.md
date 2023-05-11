<h1>AWS IaC Terraform</h1>

![image](https://github.com/Lost-Crow23/IaC_Terraform_Orchestration/assets/126012715/75aee4ac-373f-45e2-b85f-8da0d37f4578)

Infrastructure as code (IaC) tools allow you to manage infrastructure with configuration files rather than through a graphical user interface. IaC allows you to build, change, and manage your infrastructure in a safe, consistent, and repeatable way by defining resource configurations that you can version, reuse, and share.

Terraform is HashiCorp's infrastructure as code tool. It lets you define resources and infrastructure in human-readable, declarative configuration files, and manages your infrastructure's lifecycle. Using Terraform has several advantages over manually managing your infrastructure:

To deploy your infrastructure with terraform we use these steps:

- Scope - Identify the infrastructure for your project.

- Author - Write the configuration for your infrastructure.

- Initialize - Install the plugins Terraform needs to manage the infrastructure.

- Plan - Preview the changes Terraform will make to match your configuration.

- Apply - Make the planned changes.

<h2>Pre-requisites (On mac)</h2>

- Have Terraform installed ( use homebrew to do this)
- Have AWS CLI (command line interface) installed
- Have your AWS Access keys and secrey keys (Supplied by the company or can be created on AWS cloud)
- Have an Account on for AWS credentials to log in and see your instance
- Have an `ami` running already on the AWS server cloud instance, we had `app` ami already there.

<h2>Building our infrastructure</h2>

<h3>Step 1</h3>

`On MacOs`

- Have your homebrew installed already within the git bash terminal, this is where we are going install our terraform application and use the official forumula for it.

- Install the HashiCorp tap, a repository of all our Homebrew packages, `$ brew tap hashicorp/tap`

- Install Terraform, `brew install hashicorp/tap/terraform`

- update to the latest version of Terraform `brew update`

- Run the upgrade command to download and use the latest Terraform version, `brew upgrade hashicorp/tap/terraform`

<h3>Step 2</h3>

Now you should have Terraform installed, to check this we open a new terminal:

- `terraform -help` displays all available sub commands
- `terraform --version` displays the downloaded version of the terraform
```
Terraform v1.4.6
on darwin_amd64
```
<h3>Step 3 </h3>

With Terraform installed, you are ready to create your first infrastructure.

- You will provision an EC2 instance on Amazon Web Services (AWS).

Make sure you have `AWS CLI`, installed. To do this follow this link or alternatively follow the steps as below. 

- If on `MacOs` download this package `https://awscli.amazonaws.com/AWSCLIV2.pkg`
- Run your downloaded file and follow the on-screen instructions. You can choose to install the AWS CLI
- You may pick your desired location but we kept it within our 'local` but you may also have `usr` available also
- Use `which aws` then `aws --version` to check if the version has been installed.
 
<h3>Step 4</h3>

Creating our AWS CLI

- We need to `cd` onto our folder to where we want it to be created within the environment folder e.g. `IaC_Terraform_Orchestration`
- Then use the following commands to export the AWS access keys and secret keys:

`export AWS_ACCESS_KEY_ID= (yourprivateID)

`export AWS_SECRET_ACCESS_KEY= (yoursecretkey)

- print `env` or `printenv` to display the keys, as it should be all within the folder

<h3>Step 5</h3>

- `mkdir` but as we already have a directory e.g. `IaC_Terraform_Orchestration`

This is terraform file to execute and define the main configuration of your infrastructure. This file typically includes the definition of resources, providers, data sources, variables, and outputs.

- `touch main.tf`

<h3>Step 6</h3>

We enter the following commands as below to our main.tf file.

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
	# which AMI to use, this is from our AWS instance
	ami = "ami-0fb7109a40e90112d"
	instance_type = "t2.micro"
	# do you need the public IP
	associate_public_ip_address = true
	tags = { 
	  Name = "tech221_ruhal_terraform_app"

	}
}

```
<h3>Final Iteration</h3>

- Use `terraform init` to initialise and then do `terraform plan` to show what actions terraform will take to achieve the desired infrastructure state declared in the conifg files.
- `terraform apply` to apply the changes made within terraform and execute the file and enter the `value: yes` when prompted.
- Once applied an instance from the ami should be running

<h2>Defining Input Variables</h2>

Input variables are used to make it easier to use and make your configuration more flexible.

<h3>Pre-requisites</h3>

- Have a previously made main.tf file with all the conifigurations as above.

<h3>Step 1</h3>

- In `vs code` make a `.gitignore` file and put the `variables.tf` so that we can ignore the variable folder.
- Create a new file called `variables.tf` so you can script your hard code variables

<h3>Step 2</h3>

- Make a block defining the instance name with a variable as below:
```
# variables.tf

# name the variable and the ami ID is from the AMI instance.
variable "webapp_ami_id" {
    default = "ami-0fb7109a40e90112d"
}
```

<h3>Step 3</h3>
 
- Edit the main.tf folder as below:

```
provider "aws" {
	region = "eu-west-1"
}
# let's create a service on AWS
# which service -EC2
resource "aws_vpc_endpoint_id" "main" {
	# which AMI to use
	#ami = "ami-0fb7109a40e90112d"
	ami = var.webapp_ami_id
	instance_type = "t2.micro"
	# do you need the public IP
	associate_public_ip_address = true
	tags = { 
	  Name = "tech221_ruhal_terraform_app"

	}
}
```

<h3>Step 4</h3>

- Save the updated main.tf file and Apply the configuration on your gitbash terminal. 
`terraform apply`
- Should execute and run the instance

You can also set the variables without having to avoid to enter them repeatedly as you execute commands.




