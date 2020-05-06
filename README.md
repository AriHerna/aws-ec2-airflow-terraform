# AWS-ec2-airflow

Project designed to deploy an Apache Airflow cluster on AWS, backed by RDS PostgreSQL for metadata, S3 for logs and SQS as message broker with CeleryExecutor
                                                           
This project uses basically Terraform to build all the infrastructure as needed according to parameters provided. 

### Prerequisites

Have SSH keys configured without a passphrase using (Mac OS commands):
```bash
mkdir ~/.ssh/airflow
ssh-keygen -t rsa -b 4096 -f ~/.ssh/airflow/id_rsa
```

### Terraform version

[0.12.6](https://github.com/hashicorp/terraform/tree/v0.12.16)


This project uses a Terraform version manager called [tfenv](https://github.com/tfutils/tfenv).
If you already have tfenv installed, it will point automatically your Terraform to the 0.12.6 version.


### How to deploy an environment

In [terraform.tfvars](IaC2/terraform.tfvars) file configure parameters for your infrastructure, for example

```bash
webserver_instance_type = "t2.micro"
scheduler_instance_type = "t2.micro"
worker_instance_type    = "t2.micro"
```

Change this parameter on [terraform.tfvars](https://github.com/wizeline-sre/aws-ec2-airflow-terraform/blob/master/IaC/terraform.tfvars) with your profile name configured in ~/.aws/credentials file

```bash
aws_profile = "your-aws-profile"
```

Move inside IaC folder:
```bash
cd aws-ec2-airflow-terraform/IaC
```

Run Terraform plan command to have an insight of the infrastructure

```bash
terraform plan -var-file=terraform.tfvars 
```

Run Terraform apply command to build your infrastructure

```bash
terraform apply -var-file=terraform.tfvars
```

*After this point you should wait some minutes to let EC2 instances perform the bootstrap actions and launch Airflow.*

Run Terraform destroy command to delete your infrastructure

```bash
terraform destroy -var-file=terraform.tfvars
```

SSH connection to the airflow instances:

Get `.pem` file from private id_rsa:

```bash
openssl rsa -in ~/.ssh/airflow/id_rsa -outform pem > ~/.ssh/airflow/id_rsa.pem
chmod 400 id_rsa.pem
```

```bash
ssh -i "~/.ssh/airflow/id_rsa.pem" ubuntu@[airflow-ec2-ip]
```

## Contributing

Please read [CONTRIBUTING.md](CONTRIBUTING.md) to know about the process for submitting pull requests to us.

## License

This project is licensed under the Apache License - see the [LICENSE.md](LICENSE.md) file for details

## Acknowledgments

* Inspired on project built by [PowerDataHub Project](https://github.com/PowerDataHub/terraform-aws-airflow)
