# terraform
Terraform notes

Refrence: https://www.zero2devops.com/blog/ultimate-guide-to-terraform

## Commands

# Commands

- **terraform init**: This command initializes a new or existing Terraform working directory. It downloads the necessary plugins and sets up the environment for your project. You typically run this command once in a new project directory.

- **terraform plan**: This command generates an execution plan that describes what changes Terraform will make to your infrastructure. It shows you a preview of the actions that will be taken, such as creating, updating, or deleting resources. (Like Dry Run)

- **terraform apply**: This command is used to apply the changes defined in your Terraform configuration to your infrastructure. After reviewing the plan, you can run this command to actually create, update, or delete resources as specified in your code.

- **terraform destroy**: This command is used to destroy all the resources that were created by Terraform. It's essentially a way to tear down the infrastructure you've provisioned using Terraform.

- **terraform validate**: This command validates the syntax and format of your Terraform configuration files. It checks for errors and issues in your code without actually deploying anything.

- **terraform state**: This set of subcommands allows you to manage the state file that keeps track of the current state of your infrastructure. You can inspect the state, move resources between states, and perform other state-related operations.

- **terraform import**: This command lets you import existing resources into your Terraform state. It's useful when you want to manage existing infrastructure using Terraform.

- **terraform output**: This command allows you to view the outputs defined in your Terraform configuration. Outputs are values that your configuration produces, which can be useful for getting information about the resources you've created.

- **terraform refresh**: This command updates the state of your infrastructure resources by querying the cloud provider. It's useful if you suspect that the state file is out of sync with the actual state of your resources.

- **terraform taint**: The `terraform taint` command marks a resource for recreation. When you taint a resource, Terraform will destroy and recreate it during the next `terraform apply`. This is useful for forcing updates to resources that might be in a bad state or need to be replaced for some reason.

## Provider Versioning
During `terraform init`, if the version argument is not specified in the config file, the most recent provider plugin will be downloaded during initialisation.

For production use you should constrain the acceptable provider versions via configuration, this ensures that new versions with breaking changes will not be installed. 

You specify this in the tf config file in the version field for the provider:


**There is syntax for describing versions:**
```
>=1.0 - Greater than or equal to the version
<=1.0 - Less than or equal to the version
~>2.0 - Any version in the 2.X range
>=2.10, <=2.30 - Any version between 2.10 and 2.30
```


## Dependency Lock File:

The Terraform dependency lock file (`terraform.lock.hcl`) allows us to lock to a specific version of the provider. 
If a particular provider already has a version selection recorded in the lock file then Terraform will always re-select that version for installation, even if a newer version becomes available. 

You can override that behaviour by adding -upgrade when you run `terraform init`


# Terraform **state file**

Terraform stores the state of the infrastructure that are created in the `.tf` files in the Terraform **state file**.

This **state file** allows Terraform to map real world resources to your configuration files.

When we first ran `terraform init` on our initial `first_ec2.tf` file it created a **state file** within the folder.

Once we used terraform apply to bring up the EC2 instance Terraform then added the state of that EC2 Instance to the **state file**.

This is how Terraform knows what needs creating and what doesn't when subsequent changes are made in the folder. For example when we then created the GitHub repo, Terraform didn't also try to spin up another EC2 instance when we ran terraform apply. That is because while we had `first_ec2.tf` and `github.tf` in the same folder, all the resources in `first_ec2.tf` had already been created and were up and running, this information was stored in the **state file**. So Terraform checked this file and said _"Oh the EC2 instance is already here, no need to create that. But the GitHub repo is not in the **state file** so I must still need to create that"._

Once the GitHub repository was created its state was stored in the **state file**. So if we tried to run `terraform plan` it would say no changes need to be made as Terraform can see that all the requested infrastructure in the config files is up and running in the exact way the config files want it to be. 

However when we destroyed the EC2 Instance, Terraform removed it from the **state file**. This is why if we tried to run `terraform plan` after this destruction Terraform would plan to recreate the EC2 instance. The code to create the AWS instance is still in the first_ec2.tf config file but its state is no longer in the **state file** so Terraform assumes it still needs to create it. 
The **state file** is maintained by Terraform itself is not something you should ever really be directly editing. 


## Terraform Desired & Current State

Terraform's primary function is to create, modify, and destroy infrastructure resources to match the desired state described in a Terraform configuration.
The current state is the actual state of a resource that is currently deployed. Terraform will try to make sure that the deployed infrastructure is always based on the desired state defined by the .tf config files. 

If there is a difference between the desired state and current state then `terraform plan` will present a description of the changes necessary to bring the current state in line with the desired state.

If the current state and desired state match then `terraform plan` will output that no changes are needed. 

**An important thing to note is that if something is not specified in the .tf config file then it is not part of the desired state.**  

For example in our AWS EC2 instance created at the start we have specified only the aspects that are mandatory to specify:
We have not for example specified what security group this should be part of. As such Terraform has put it in the default security group. However if I went on to the AWS  Browser Console and manually changed the security group then the current state of the EC2 Instance would differ from what was in the **state file**. The **state file** will still show that the security group is default while the actual, physical current state of the EC2 security group is that it's a custom one (not default). 

So you might think that if we now run `terraform plan` that Terraform will pick this change up and plan to change the security group back to the default. However that is not the case, even though the EC2 instance would differ from the **state file** it doesn't actually differ from the desired state. We have not specified the security group in the config file so there is not a "desired state" for the security group. 

As such all terraform will do when we run `terraform plan` is request the current state of the EC2 see that it doesn't differ from the desired state and so not plan any changes, however it will see that it does differ from the **state file** so Terraform will update the **state file** to show the EC2 instance has a custom security group rather than the default. 