<h1>IaC_Terraform_Orchestration with our VPC</h1>

![image](https://github.com/Lost-Crow23/IaC_Terraform_Orchestration/assets/126012715/aac128a1-bc37-4b29-813b-04dd452cf0f8)

An private IP was assigned to me for this task as 10.0.9.0/24 so anywhere you see this, replace it with your private IP.

- For consistency we'll also be using,
10.0.0.0/16 as a reference to the public subnet.

10.0.9.0/24 as a reference to the private subnet. Later changed to 10.0.91.0/24 to make the private subnet work.

<h2>Creation of VPC </h2>

We have used "public" and "private" as a name for the prefixes, however you may use alternative names, but for easier tracking we use these names which is recommended.

<h3>Step 1</h3>

- Create VPC 

<h3>Step 2</h3>

- Choose `vpc only` and name the desired vpc e.g `ruhal-tech221-terraform-vpc`
  
- Apply a `CIDR` block `10.0.0.0/16`

<h3>Step 3</h3>

Create Internet Gateaway

- Name the IG e.g `ruhal-tech221-IG-terraform-vpc`
- Attach the IG into the vpc created prior 

<h3>Step 4</h3>

Create a public subnet. > Personal VPC > Edit name > No AZ preffence.

- Choose the vpc just created as the vpc ID
- `ruhal-tech221-public-terraform-subnet` make a subnet name
- Select IPv4 CIDR as your private IP so `10.0.9.0/24` was inputted.
- Create subnet

<h3>Step 5</h3>

Create the route table. > Name edit > Select personal VPC > create

- Name the table e.g `ruhal-tech221-public-terraform-RT`
- Select the prior VPC you created

<h3>Step 6</h3>

Edit our subnet associations. > Edit > Select subnet > Click routes > Click edit route.

- Edit our routes as below and add our internet gateaway ID `igw-047202aedb1d756ee` with the destination being `0.0.0.0/0
- Save changes

<h3>Step 7</h3>

Create a private subnet

- Select your terraform VPC created and the name your subnet `ruhal-tech221-private-terraform-subnet`
- No AZ preffered
- And `Private: 10.0.91.0/24`
- create subnet

<h3>Final iteration</h3>

Create the route table. > Name edit > Select personal VPC > create

- Name the table e.g `ruhal-tech221-private-terraform-RT`
- Select the prior VPC you created

Edit our subnet associations. > Edit > Select subnet > Click routes > Click edit route.

- Edit our routes as below and add our internet gateaway ID `igw-047202aedb1d756ee` with the destination being `0.0.0.0/0
- Save changes
