# AWSUG-Pal-Nov-2022

## 1- Log in to AWS and Create a EC2 Instance t2.micro EC2 Instance
- Go to EC2 service, create an EC2 Instance of type `t2.micro`, With piublic access, create key if needed
- Use SSM or SSH to connect to that Instance
- Allow SSH via the Security group

## 2- Clone the following repos

`git clone https://github.com/Ahmed-AG/cdf.git
git clone https://github.com/Ahmed-AG/vpc-cloudformation.git
git https://github.com/Ahmed-AG/vpc-terraform.git`

## 3- Log in tot he instance
`chmod 400 <PEM File>
ssh -i <PEM File> ubuntu@<Instance IP Address>`

## 4- Set up the instance

`
sudo apt-get update && sudo apt-get install jq curl wget ruby python3 python3-pip docker.io  -y
`
  
## 5- Install tools
`wget --quiet https://releases.hashicorp.com/terraform/1.2.5/terraform_1.2.5_linux_386.zip && unzip terraform_1.2.5_linux_386.zip && sudo mv terraform /usr/bin && rm terraform_1.2.5_linux_386.zip`
  
`sudo pip3 install checkov` and `sudo gem install cfn-nag`
  
