# AWSUG-Pal-Nov-2022

## 1- Log in to AWS and Create a EC2 Instance t2.micro EC2 Instance
- Go to EC2 service, create an EC2 Instance of type `t2.micro`, With piublic access, create key if needed
- Use SSM or SSH to connect to that Instance
- Allow SSH via the Security group

## 2- Log in tot he instance

```bash
chmod 400 <PEM File>
ssh -i <PEM File> ubuntu@<Instance IP Address>
```

## 3- Set up the instance

```bash
sudo apt-get update && sudo apt-get install jq curl wget ruby python3 python3-pip docker.io  -y
```
  
## 4- Install tools
```bash
wget --quiet https://releases.hashicorp.com/terraform/1.2.5/terraform_1.2.5_linux_386.zip && unzip terraform_1.2.5_linux_386.zip && sudo mv terraform /usr/bin && rm terraform_1.2.5_linux_386.zip
sudo pip3 install checkov
sudo gem install cfn-nag`
```

Finally, install awscli version2

```bash
export AWSCLI_URL=https://awscli.amazonaws.com/awscli-exe-linux-x86_64-2.1.24.zip && curl -sL ${AWSCLI_URL} -o awscliv2.zip && sudo unzip -q awscliv2.zip && sudo aws/install
```

  
## 5- Clone the following repos

```bash
git clone https://github.com/Ahmed-AG/cdf.git
git clone https://github.com/Ahmed-AG/vpc-cloudformation.git
git https://github.com/Ahmed-AG/vpc-terraform.git
```

## 6- Create CodeCommit repos
- Create CodeCommit SSh Keys using your IAM user
- Create two repos `vpc-cloudformation` and `vpc-terraform`
- Upload your local `vpc-cloudformation` and `vpc-terraform code` into the codecommit repos

## 7- Use CDF to create the pipelines
- Bootstrap CDK:
```bash
cdk bootstrap aws://ACCOUNT-NUMBER-1/REGION-1
```
- Configure `config.d/config.json`:
```bash
{
    "pipelines" :[
        {
            "name" : "cfn-pipeline1",
            "provider" : "aws",
            "source" : {
                "source_type" : "codecommit",
                "repo_name" : "<CloudFormation Repo>",
                "branch" : "main"
            },
            "deployment" : {
                "assume_role" :{
                    "role": "TODO"
                },
                "aws_account" : "",
                "iam_policy_file" : "config.d/iam-policy.json",
                "region" : "us-east-1",
                "type" : "cfn",
                "parameters" : "VpcCIDR=10.0.0.0/16 Region=$REGION",
                "capabilities" : "CAPABILITY_IAM CAPABILITY_NAMED_IAM",
                "deployment_file" : "main.yaml",
                "checks" : [
                    "general_all",
                    "cfn_nag", 
                    "checkov"
                ]
            }
        },
        {
            "name" : "terraform-pipeline2",
            "provider" : "aws",
            "source" : {
                "source_type" : "codecommit",
                "repo_name" : "<terraform Repo>",
                "branch" : "master"
            },
            "deployment" : {
                "assume_role" :{
                    "role": "TODO"
                },
                "aws_account" : "",
                "iam_policy_file" : "config.d/iam-policy.json",
                "region" : "us-east-1",
                "type" : "terraform",
                "parameters" : "VpcCIDR=10.0.0.0/16 Region=$REGION",
                "deployment_folder" : "./",
                "checks" : [
                    "general_all",
                    "checkov"
                ]
            }
        }
    ]
}
```
- Update the cloudformation repo name and the terraform repo name
- Deploy the pipelines
```bash
cdk deploy --all
```
