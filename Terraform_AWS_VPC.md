<h1>IaC_Terraform_Orchestration with our VPC</h1>

![image](https://github.com/Lost-Crow23/IaC_Terraform_Orchestration/assets/126012715/aac128a1-bc37-4b29-813b-04dd452cf0f8)

![IaC vpc diagramm](https://github.com/Lost-Crow23/IaC_Terraform_Orchestration/assets/126012715/5ee6e9e9-9f1d-422e-b7f4-47c9bd41a8e2)

<h2>Create a Hybrid Infrastructure with a VPC</h2>

<h3>Create a VPC</h3>

<h3>Step 1</h3>

```
# Create a VPC on AWS

resource "aws_vpc" "tech221_ruhal_terraform_vpc" {
  cidr_block = var.cidr_block (10.0.9.0/24)
  instance_tenancy = "default"

  tags = {
    Name = "tech221_ruhal_terraform_vpc"
  }
}

```

<h3>Step 2: Add a internet gateaway</h3>

```
#create a Gateway on AWS
resource "aws_internet_gateway" "tech221_ruhal_terraform_gw" {
  vpc_id = aws_vpc.tech221_ruhal_terraform_vpc.id

  tags = {
    Name = "tech221_ruhal_terraform_gw"
  }
}
```

h3>Step 3: Add a Route Table</h3>

```
# Create a public route table on AWS
resource "aws_route_table" "tech221_ruhal_publicRT" {
  vpc_id = aws_vpc.tech221_ruhal_terraform_vpc.id

  route {
    cidr_block = "0.0.0.0/0"
    gateway_id = aws_internet_gateway.tech221_ruhal_terraform_gw.id
  }

  tags = {
    Name = "ruhal_public_RT"
  }
}
```
<h3>Create a Private Route Table</h3>

```
# Create a private route table on AWS
resource "aws_route_table" "tech221_ruhal_privateRT" {
  vpc_id = aws_vpc.tech221_ruhal_terraform_vpc.id

  tags = {
    Name = "ruhal_private_RT"
  }
}
```
<h3>Step 4: Create a public subnet</h3>

```
# Create a Public Subnet in VPC

resource "aws_subnet" "tech221_ruhal_publicSubnet" {
	vpc_id = aws_vpc.tech221_ruhal_terraform_vpc.id
	cidr_block = var.public_subnet_cidr_block
	map_public_ip_on_launch = "true"
	availability_zone = var.availability_zone

	tags = {
		Name = "tech221_ruhal_publicSubnet"
	}

}
```

<h3>Step 5: Create a private subnet</h3>

```
# Creating Route association private Subnet

resource "aws_route_table_association" "ruhal_private_association" {
	subnet_id = aws_subnet.tech221_ruhal_privateSubnet.id
	route_table_id = aws_route_table.tech221_ruhal_privateRT.id
}
```
<h3>Step 6: Associate the public subnet to the public route table</h3>

```
# Creating Route association

resource "aws_route_table_association" "ruhal_public_association" {
	subnet_id = aws_subnet.tech221_ruhal_publicSubnet.id
	route_table_id = aws_route_table.tech221_ruhal_publicRT.id
}

```
<h3>Step 7: Associate the Private subnet to the Private RT</h3>

```
# Creating Route association 

resource "aws_route_table_association" "ruhal_private_association" {
	subnet_id = aws_subnet.tech221_ruhal_privateSubnet.id
	route_table_id = aws_route_table.tech221_ruhal_privateRT.id
}
```
<h3>Step 8: Creating the Security groups for the app</h3>
```
# Creating a security group EC2 app

resource "aws_security_group" "tech221_ruhal_AppSG" {
  name        = "security group for App"
  description = "security group for App"
  vpc_id      = aws_vpc.tech221_ruhal_terraform_vpc.id

  ingress {
    description      = "HTTP"
    from_port        = 80
    to_port          = 80
    protocol         = "tcp"
    cidr_blocks      = ["0.0.0.0/0"]
    ipv6_cidr_blocks = ["::/0"]
  }

  ingress {
    description      = "port3000"
    from_port        = 3000
    to_port          = 3000
    protocol         = "tcp"
    cidr_blocks      = ["0.0.0.0/0"]
    ipv6_cidr_blocks = ["::/0"]
  }

  egress {
    from_port        = 0
    to_port          = 0
    protocol         = "-1"
    cidr_blocks      = ["0.0.0.0/0"]
    ipv6_cidr_blocks = ["::/0"]
  }

  tags = {
    Name = "tech221_ruhal_AppSG"
  }
}

```

<h3>Step 9: Launching the instance</h3>

```
#within our public subnet

resource "aws_instance" "app_instance"{
	# which ami to use
	ami = var.aim_id
	
	#type of instance
	instance_type = "t2.micro"

	# do you need the public IP
	associate_public_ip_address = true

	# Which subnet

	subnet_id = "${aws_subnet.tech221_ruhal_publicSubnet.id}"

        # Add security groups

	security_groups = ["${aws_security_group.tech221_ruhal_AppSG.id}"]

	# what would you like to name it
	tags = {
	
	  Name = "tech221_ruhal_terraform_app"

	} 

}

```
