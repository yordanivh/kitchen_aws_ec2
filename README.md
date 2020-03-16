# kitchen_aws_ec2
This repo contains code that will test if terraform creates an aws instance with ubuntu OS

# What is this repo for

This repo contains terraform code that will create an aws ec2 instance and will test if that instance has Ubuntu OS installed on it.

# Why use this repo

This repo is valuable since it shows hands on how to use kitchen test with terraform.

# How to use this repo

### Before using this code there are some prerequisites that need to be fulfilled.

* AWS Account
* AWS Access Key ID
* AWS Secret Key

  Put both AWS Access Key ID and AWS Secret Key as environment variables for the shel you are using like so
  ```
  export AWS_ACCESS_KEY_ID=XXXXXXXXXXXXXXXX
  export AWS_SECRET_ACCESS_KEY=XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
  ```
  check if they are now present by typing in `env` command
  
* An AWS Keypair

 Create a key pair in the portal  or follow this [instruction](https://docs.aws.amazon.com/cli/latest/reference/ec2/create-key-pair.html)
 
* Terraform installed

You can download Terraform from this [link](https://www.terraform.io/downloads.html)

* Ruby >= 2.4, < 2.7

Ruby can be installed with `brew install ruby`

* Bundler installed

Bundler can be installed with "gem install bundler"

### Steps to use this code

* Download this repository to you local workstation

```
git clone git@github.com:yordanivh/kitchen_aws_ec2.git
```

* Change to the downloaded repo

```
cd kitchen_aws_ec2
```

* In the Gemfile you can see that we will use `kitchen-terraform` gem, there we need to download it for this environment with the following command

```
bundle install
```

* In the .kitchen.yml file you will see this config

```
key_files:
        - /Users/yhalachev/Downloads/yordan.pem
```
 you need to change the path with the one of your own key. That is to be used to be able to login via ssh on the EC2 instance.

* create a .tfvars file to put in the values of the variables

```
touch testing.tfvars
```

* put the following infromation there

key_name = "Your key name"
region = "Region the key is located in"
ami = "ami-0fc20dd1da406780b"
instance_type = "t2.micro"

* Now we can perform the test. First step is to create the resource we wil test.

```
bundle exec kitchen converge
```

* You can expect the output of the command to look something like this.

```
❯ bundle exec kitchen converge
-----> Starting Test Kitchen (v2.4.0)
-----> Creating <default-ubuntu>...
$$$$$$ Verifying the Terraform client version is in the supported interval of >= 0.11.4, < 0.13.0...
$$$$$$ Reading the Terraform client version...
       Terraform v0.12.23
       + provider.aws v2.53.0
$$$$$$ Finished reading the Terraform client version.
$$$$$$ Finished verifying the Terraform client version.
$$$$$$ Initializing the Terraform working directory...
       
       Initializing the backend...
       
       Initializing provider plugins...
       - Checking for available provider plugins...
       - Downloading plugin for provider "aws" (hashicorp/aws) 2.53.0...
       
       The following providers do not have any version constraints in configuration,
       so the latest version was installed.
       
       To prevent automatic upgrades to new major versions that may contain breaking
       changes, it is recommended to add version = "..." constraints to the
       corresponding provider blocks in configuration, with the constraint strings
       suggested below.
       
       * provider.aws: version = "~> 2.53"
       
       Terraform has been successfully initialized!
$$$$$$ Finished initializing the Terraform working directory.
$$$$$$ Creating the kitchen-terraform-default-ubuntu Terraform workspace...
       Created and switched to workspace "kitchen-terraform-default-ubuntu"!
       
       You're now on a new, empty workspace. Workspaces isolate their state,
       so if you run "terraform plan" Terraform will not see any existing state
       for this configuration.
$$$$$$ Finished creating the kitchen-terraform-default-ubuntu Terraform workspace.
       Finished creating <default-ubuntu> (0m22.52s).
-----> Converging <default-ubuntu>...
$$$$$$ Verifying the Terraform client version is in the supported interval of >= 0.11.4, < 0.13.0...
$$$$$$ Reading the Terraform client version...
       Terraform v0.12.23
       + provider.aws v2.53.0
$$$$$$ Finished reading the Terraform client version.
$$$$$$ Finished verifying the Terraform client version.
$$$$$$ Selecting the kitchen-terraform-default-ubuntu Terraform workspace...
$$$$$$ Finished selecting the kitchen-terraform-default-ubuntu Terraform workspace.
$$$$$$ Downloading the modules needed for the Terraform configuration...
$$$$$$ Finished downloading the modules needed for the Terraform configuration.
$$$$$$ Validating the Terraform configuration files...
       
       Warning: The -var and -var-file flags are not used in validate. Setting them has no effect.
       
       These flags will be removed in a future version of Terraform.
       
       Success! The configuration is valid, but there were some validation warnings as shown above.
       
$$$$$$ Finished validating the Terraform configuration files.
$$$$$$ Building the infrastructure based on the Terraform configuration...
       aws_security_group.allow_ssh: Creating...
       aws_security_group.allow_ssh: Creation complete after 10s [id=sg-055519d639e31eb11]
       aws_instance.example: Creating...
       aws_instance.example: Still creating... [10s elapsed]
       aws_instance.example: Still creating... [20s elapsed]
       aws_instance.example: Creation complete after 28s [id=i-00848c2df4eb9ebfd]
       
       Apply complete! Resources: 2 added, 0 changed, 0 destroyed.
       
       Outputs:
       
       public_dns = ec2-3-15-45-109.us-east-2.compute.amazonaws.com
$$$$$$ Finished building the infrastructure based on the Terraform configuration.
$$$$$$ Reading the output variables from the Terraform state...
$$$$$$ Finished reading the output variables from the Terraform state.
$$$$$$ Parsing the Terraform output variables as JSON...
$$$$$$ Finished parsing the Terraform output variables as JSON.
$$$$$$ Writing the output variables to the Kitchen instance state...
$$$$$$ Finished writing the output varibales to the Kitchen instance state.
$$$$$$ Writing the input variables to the Kitchen instance state...
$$$$$$ Finished writing the input variables to the Kitchen instance state.
       Finished converging <default-ubuntu> (0m50.38s).
-----> Test Kitchen is finished. (1m13.70s)
```

* Execute the following command to perform the test

```
bundle exec kitchen verify
```

* The outout should look like this

```
-----> Starting Test Kitchen (v2.4.0)
-----> Setting up <default-ubuntu>...
       Finished setting up <default-ubuntu> (0m0.00s).
-----> Verifying <default-ubuntu>...
$$$$$$ Reading the Terraform input variables from the Kitchen instance state...
$$$$$$ Finished reading the Terraform input variables from the Kitchen instance state.
$$$$$$ Reading the Terraform output variables from the Kitchen instance state...
$$$$$$ Finished reading the Terraform output varibales from the Kitchen instance state.
$$$$$$ Verifying the systems...
$$$$$$ Verifying the 'default' system...

Command: `lsb_release -a`
  stdout
    is expected to match /Ubuntu/

Finished in 0.44098 seconds (files took 5.72 seconds to load)
1 example, 0 failures

$$$$$$ Finished verifying the 'default' system.
$$$$$$ Finished verifying the systems.
       Finished verifying <default-ubuntu> (0m6.01s).
-----> Test Kitchen is finished. (0m6.76s)
```

* Next step is to destroy the environment to save resources and to not be charged.

```
bundle exec kitchen destroy
```

* Output

```
-----> Starting Test Kitchen (v2.4.0)
-----> Destroying <default-ubuntu>...
$$$$$$ Verifying the Terraform client version is in the supported interval of >= 0.11.4, < 0.13.0...
$$$$$$ Reading the Terraform client version...
       Terraform v0.12.23
       + provider.aws v2.53.0
$$$$$$ Finished reading the Terraform client version.
$$$$$$ Finished verifying the Terraform client version.
$$$$$$ Initializing the Terraform working directory...
       
       Initializing the backend...
       
       Initializing provider plugins...
       
       The following providers do not have any version constraints in configuration,
       so the latest version was installed.
       
       To prevent automatic upgrades to new major versions that may contain breaking
       changes, it is recommended to add version = "..." constraints to the
       corresponding provider blocks in configuration, with the constraint strings
       suggested below.
       
       * provider.aws: version = "~> 2.53"
       
       Terraform has been successfully initialized!
$$$$$$ Finished initializing the Terraform working directory.
$$$$$$ Selecting the kitchen-terraform-default-ubuntu Terraform workspace...
$$$$$$ Finished selecting the kitchen-terraform-default-ubuntu Terraform workspace.
$$$$$$ Destroying the Terraform-managed infrastructure...
       aws_security_group.allow_ssh: Refreshing state... [id=sg-055519d639e31eb11]
       aws_instance.example: Refreshing state... [id=i-00848c2df4eb9ebfd]
       aws_instance.example: Destroying... [id=i-00848c2df4eb9ebfd]
       aws_instance.example: Still destroying... [id=i-00848c2df4eb9ebfd, 10s elapsed]
       aws_instance.example: Still destroying... [id=i-00848c2df4eb9ebfd, 20s elapsed]
       aws_instance.example: Still destroying... [id=i-00848c2df4eb9ebfd, 30s elapsed]
       aws_instance.example: Still destroying... [id=i-00848c2df4eb9ebfd, 40s elapsed]
       aws_instance.example: Destruction complete after 48s
       aws_security_group.allow_ssh: Destroying... [id=sg-055519d639e31eb11]
       aws_security_group.allow_ssh: Destruction complete after 2s
       
       Destroy complete! Resources: 2 destroyed.
$$$$$$ Finished destroying the Terraform-managed infrastructure.
$$$$$$ Selecting the default Terraform workspace...
       Switched to workspace "default".
$$$$$$ Finished selecting the default Terraform workspace.
$$$$$$ Deleting the kitchen-terraform-default-ubuntu Terraform workspace...
       Deleted workspace "kitchen-terraform-default-ubuntu"!
$$$$$$ Finished deleting the kitchen-terraform-default-ubuntu Terraform workspace.
       Finished destroying <default-ubuntu> (1m5.63s).
-----> Test Kitchen is finished. (1m6.38s)
```



