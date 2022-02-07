This tutorial is only for learning on how setting up a Terraform config to spin up an Amazon Web Services (AWS) EC2 instance using Inspec and Kitchen-Terraform from scratch.

### Prerequisites
- [X] [VirtualBox](https://www.virtualbox.org/wiki/Downloads)

- [X] [Vagrant](https://www.vagrantup.com/downloads)

- [X] [Terraform](https://www.terraform.io/downloads)


## How to Use this Repo

- Clone this repository:
```shell
git clone git@github.com:dlavric/tf-aws-cluster.git
```

- Go to the directory where the repo is stored:
```shell
cd tf-aws-cluster
```

- Start Vagrant
```shell
vagrant up
```

- While the VM is created, go to the AWS console and create a key pair

Step 1. Search for the keyword `key pair` in the search box of the AWS console

![Step 1](https://github.com/dlavric/tf-aws-cluster/blob/main/pictures/Screenshot1.png)

Step 2. While in the `Key Pair` section click on the orange button `Create Key Pair`

![Step 2](https://github.com/dlavric/tf-aws-cluster/blob/main/pictures/Screenshot2.png)

Step 3. Fill in the name of your key pair and choose the options that are shown in the screenshot and create the `.pem` file

![Step 3](https://github.com/dlavric/tf-aws-cluster/blob/main/pictures/Screenshot3.png)

- Connect to the VM
```shell
vagrant ssh terraform
```

- Go to the following directory
```shell
cd /vagrant
```

- Copy the `.pem` file you have created previously in the /vagrant directory
```shell
vim <your_key_pair.pem>
```

- Make sure that all the dependencies in the Gemfile are available for the application
```shell
bundle install
```

- Add the values for the testing variables
```shell
vim testing.tfvars

key_name = "daniela_key_pair"
region = "us-east-1"
ami = "ami-fce3c696"
instance_type = "m3.medium"
access_key= "your-access-key"
secret_key= "your-secret-key"
aws_session_token= "your-aws-session-token"
```


- Check if the kitchen test is going to work
```shell
bundle exec kitchen converge

-----> Starting Test Kitchen (v2.12.0)
-----> Converging <default-ubuntu>...
$$$$$$ Reading the Terraform client version...
       Terraform v0.12.31
       + provider.aws v3.74.0
       
       Your version of Terraform is out of date! The latest version
       is 1.1.5. You can update by downloading from https://www.terraform.io/downloads.html
$$$$$$ Finished reading the Terraform client version.
$$$$$$ Verifying the Terraform client version is in the supported interval of < 1.1.0, >= 0.11.4...
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
       aws_security_group.allow_ssh: Creation complete after 9s [id=sg-066e7d187feb31a94]
       aws_instance.example: Creating...
       aws_instance.example: Still creating... [10s elapsed]
       aws_instance.example: Still creating... [20s elapsed]
       aws_instance.example: Creation complete after 29s [id=i-08ebcbd2c055927d5]
       
       Apply complete! Resources: 2 added, 0 changed, 0 destroyed.
       
       Outputs:
       
       public_dns = ec2-3-93-232-250.compute-1.amazonaws.com
$$$$$$ Finished building the infrastructure based on the Terraform configuration.
$$$$$$ Reading the output variables from the Terraform state...
$$$$$$ Finished reading the output variables from the Terraform state.
$$$$$$ Parsing the Terraform output variables as JSON...
$$$$$$ Finished parsing the Terraform output variables as JSON.
$$$$$$ Writing the output variables to the Kitchen instance state...
$$$$$$ Finished writing the output variables to the Kitchen instance state.
$$$$$$ Writing the input variables to the Kitchen instance state...
$$$$$$ Finished writing the input variables to the Kitchen instance state.
       Finished converging <default-ubuntu> (0m50.52s).
-----> Test Kitchen is finished. (0m51.02s)
```

- Running the tests now
```shell
bundle exec kitchen verify

-----> Starting Test Kitchen (v2.12.0)
-----> Setting up <default-ubuntu>...
       Finished setting up <default-ubuntu> (0m0.00s).
-----> Verifying <default-ubuntu>...
$$$$$$ Reading the Terraform input variables from the Kitchen instance state...
$$$$$$ Finished reading the Terraform input variables from the Kitchen instance state.
$$$$$$ Reading the Terraform output variables from the Kitchen instance state...
$$$$$$ Finished reading the Terraform output variables from the Kitchen instance state.
$$$$$$ Verifying the systems...
$$$$$$ Verifying the 'default' system...

Finished in 0.00063 seconds (files took 9.46 seconds to load)
0 examples, 0 failures

$$$$$$ Finished verifying the 'default' system.
$$$$$$ Finished verifying the systems.
       Finished verifying <default-ubuntu> (0m9.41s).
-----> Test Kitchen is finished. (0m9.91s)
vagrant@terraform:/vagrant$ vim test/integration/default/controls/operating_system_spec.rb
vagrant@terraform:/vagrant$ vim test/integration/default/controls/operating_system_spec.rb
vagrant@terraform:/vagrant$ bundle exec kitchen verify
-----> Starting Test Kitchen (v2.12.0)
-----> Verifying <default-ubuntu>...
$$$$$$ Reading the Terraform input variables from the Kitchen instance state...
$$$$$$ Finished reading the Terraform input variables from the Kitchen instance state.
$$$$$$ Reading the Terraform output variables from the Kitchen instance state...
$$$$$$ Finished reading the Terraform output variables from the Kitchen instance state.
$$$$$$ Verifying the systems...
$$$$$$ Verifying the 'default' system...

Command: `lsb_release -a`
  stdout
    is expected to match /Ubuntu/

Finished in 0.35071 seconds (files took 4.7 seconds to load)
1 example, 0 failures

$$$$$$ Finished verifying the 'default' system.
$$$$$$ Finished verifying the systems.
       Finished verifying <default-ubuntu> (0m5.01s).
-----> Test Kitchen is finished. (0m5.48s)
```

- Destroy the test instances
```shell
bundle exec kitchen destroy

-----> Starting Test Kitchen (v2.12.0)
-----> Destroying <default-ubuntu>...
$$$$$$ Reading the Terraform client version...
       Terraform v0.12.31
       + provider.aws v3.74.0
       
       Your version of Terraform is out of date! The latest version
       is 1.1.5. You can update by downloading from https://www.terraform.io/downloads.html
$$$$$$ Finished reading the Terraform client version.
$$$$$$ Verifying the Terraform client version is in the supported interval of < 1.1.0, >= 0.11.4...
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
       
       * provider.aws: version = "~> 3.74"
       
       Terraform has been successfully initialized!
$$$$$$ Finished initializing the Terraform working directory.
$$$$$$ Selecting the kitchen-terraform-default-ubuntu Terraform workspace...
$$$$$$ Finished selecting the kitchen-terraform-default-ubuntu Terraform workspace.
$$$$$$ Destroying the Terraform-managed infrastructure...
       aws_security_group.allow_ssh: Refreshing state... [id=sg-066e7d187feb31a94]
       aws_instance.example: Refreshing state... [id=i-08ebcbd2c055927d5]
       aws_instance.example: Destroying... [id=i-08ebcbd2c055927d5]
       aws_instance.example: Still destroying... [id=i-08ebcbd2c055927d5, 10s elapsed]
       aws_instance.example: Still destroying... [id=i-08ebcbd2c055927d5, 20s elapsed]
       aws_instance.example: Still destroying... [id=i-08ebcbd2c055927d5, 31s elapsed]
       aws_instance.example: Destruction complete after 33s
       aws_security_group.allow_ssh: Destroying... [id=sg-066e7d187feb31a94]
       aws_security_group.allow_ssh: Destruction complete after 2s
       
       Destroy complete! Resources: 2 destroyed.
$$$$$$ Finished destroying the Terraform-managed infrastructure.
$$$$$$ Selecting the default Terraform workspace...
       Switched to workspace "default".
$$$$$$ Finished selecting the default Terraform workspace.
$$$$$$ Deleting the kitchen-terraform-default-ubuntu Terraform workspace...
       Deleted workspace "kitchen-terraform-default-ubuntu"!
$$$$$$ Finished deleting the kitchen-terraform-default-ubuntu Terraform workspace.
       Finished destroying <default-ubuntu> (0m49.06s).
-----> Test Kitchen is finished. (0m49.55s)
```

- Logout from the Vagrant virtual machine
```shell
logout
```

- Destroy the Virtual Machine
```shell
vagrant destroy
```