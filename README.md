## Installing

* [Install Vagrant 1.2](http://docs.vagrantup.com/v2/installation/index.html)
* Install the [vagrant-aws plugin](https://github.com/mitchellh/vagrant-aws)
* configure the `Vagrantfile` according to the documentation 

## Dependencies
* Set up [EC2 CLI Tools](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/setting_up_ec2_command_linux.html) locally
* An AWS security group granting ssh access to the world must be set up as well
* An AWS keypair private key must be accessible locally from the Vagrantfile
* The best way to get the chef cookbooks is to sign up for a free account on the hosted chef service from [opscode.com](https://www.opscode.com) (Some are already copied in the example box)
* The provisioning part of the Vagrantfile has been built using the Rove.io service

## Running
If everything has been set up correctly running `vagrant up --provider=aws` should spin up the EC2 instance and after several minutes all the provisioning script should install the desired software, resulting on a fully provisioned box.

## Caveats
Some of the cookbooks are still experimental for AWS boxes and return errors (from a non-backwards compatible api change in Chef, particularly around mongod services calls during installation). However this isn't a problem because in despite of certain returned errors the provisioning still goes well.