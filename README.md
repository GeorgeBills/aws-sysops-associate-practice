## Useful commands

 * Create initial stack, update stack:
   `aws cloudformation deploy --template-file drupal.yaml --stack-name mydrupalstack --parameter-overrides KeyName=MyKeyPair MyHomeCIDR=203.0.113.123/0 DatabaseMasterPassword=my53cr37p455w0rd LogsBucketName=mys3logsbucket MyMobilePhoneNumber=+61412345678 --capabilities CAPABILITY_NAMED_IAM`
 * `ssh-add $env:userprofile\.ssh\MyKeyPair.pem`
 * `ssh -A ec2-user@$env:bastion_ip`
 * `ssh $drupal_ip`
 * `bash /tmp/install.cmd` (would be nice to automate this...)
 * Delete stack and all of its resources:
   `aws cloudformation delete-stack --stack-name mydrupalstack`

## Pre

 - [X] CloudTrail logs saved to S3
 - [X] Config snapshots saved to S3

## Networking

 - [X] Custom VPC
   - [X] DMZ (public subnets): ELB, bastion, NAT
   - [X] Internal (private subnets): web servers, RDS
 - [X] HTTPS access to the ELB
 - [X] SSH access to the bastion, only from your home IP
 - [X] SSH access to the web servers, only from the bastion
 - [X] HTTP access to the web servers, only from the ELB
 - [X] MySQL access to RDS, only from the web servers
 - [X] NAT instance to allow the EC2 servers to connect out for e.g. Yum updates
       (NAT instances are much cheaper than gateways...)
 - [ ] Registered domain name for the ELB
 - [X] Elastic IP for the bastion
 - [X] EC2 bastion host
 - [ ] Route AWS traffic internally with a VPC endpoint
   - [ ] [CloudFormation](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/cfn-vpce-bucketnames.html)
   - [ ] [Yum updates](https://aws.amazon.com/amazon-linux-ami/faqs/#vpc-endpoint)

## Web application

 - [X] EC2 web servers (Drupal)
   - [X] Autoscaling
   - [X] Minimum 3 instances, spread across 3 availability zones
   - [X] Self configured on launch
 - [X] RDS MySQL
 - [X] ELB
 - [X] Uploads stored in EFS
 - [X] Sensitive parameters stored in Parameter Store

## Monitoring

 - [X] ELB logs saved to S3
 - [X] All resources tagged and displayed within a resource group (manually)
 - [X] CloudWatch dashboard (manually)
    - [X] [baseline monitoring](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/monitoring_ec2.html)
    - [X] Plus disk and memory usage custom metrics
    - [X] With dimensions to allow easily filtering metrics to just the webservers
 - [X] Ship Apache logs to CloudWatch logs
 - [X] Alarm on unexpectedly high billing (manually)
 - [X] Alarm on zero healthy hosts

## Security

 - [ ] ACM certificate provisioned on the ELB
 - [X] Compliance check on worldwide port 22 access
 - [X] [Basic WAF](https://aws.amazon.com/premiumsupport/knowledge-center/waf-block-common-attacks/)
 - [X] Encryption in transit to RDS
 - [X] RDS encrypted at rest
 - [X] NACL per [VPC recommended NACL rules](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-recommended-nacl-rules.html#nacl-rules-scenario-2)

## Scaling

 - [ ] Read replicas for RDS
 - [ ] ElastiCache cluster to cache database queries
 - [ ] Autoscale target group
   - [ ] Use lifecycle hooks to only bring instances into service once everything is running
 - [ ] CloudFront CDN
   - [ ] Static asset (e.g. images, CSS, JS) caching with a high TTL
   - [ ] Dynamic asset (e.g. webpage) caching with a low TTL

## High Availability

 - [ ] Multi-AZ RDS
 - [ ] Failover (via Route 53) to a static page in S3 if ELB unhealthy
 - [ ] Event and notification on RDS failover to secondary
 - [ ] Automated backups for RDS
 - [ ] Turn on MFA delete and versioning for logs bucket
 - [ ] Use NAT _gateway_ if prod
 - [ ] Bastion HA if prod (https://aws.amazon.com/quickstart/architecture/linux-bastion/)

## Tasks

 - [X] Apply stack via AWS CLI
 - [X] Export CloudWatch logs to S3
 - [X] Query logs with Athena
 - [ ] Install AWS Inspector via SSM and run an assessment
