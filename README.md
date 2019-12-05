## Useful commands

 * Create initial stack, update stack:
   `aws cloudformation deploy --template-file stack.yaml --stack-name mydrupalstack --parameter-overrides KeyName=MyKeyPair MyHomeCIDR=203.0.113.123/0 DatabaseMasterPassword=my53cr37p455w0rd LogsBucketName=mys3logsbucket MyMobilePhoneNumber=+61412345678 --capabilities CAPABILITY_NAMED_IAM`
 * Delete stack and all of its resources:
   `aws cloudformation delete-stack --stack-name mydrupalstack`

## Pre

 - [X] CloudTrail logs saved to S3
 - [X] Config snapshots saved to S3

## Web application

 - [X] EC2 web servers (Drupal)
   - [X] Autoscaling
   - [X] Minimum 3 instances, spread across 3 availability zones
   - [X] Self configured on launch
 - [X] EC2 bastion host
 - [X] RDS MySQL
 - [X] ELB
 - [X] Uploads stored in EFS
 - [X] Sensitive parameters stored in Parameter Store

## High Availability and Scaling

 - [ ] Multi-AZ RDS
 - [ ] Read replicas for RDS
 - [ ] ElastiCache cluster to cache database queries
 - [ ] Autoscale target group
 - [ ] CloudFront CDN
   - [ ] Static asset (e.g. images, CSS, JS) caching with a high TTL
   - [ ] Dynamic asset (e.g. webpage) caching with a low TTL
 - [ ] Failover (via Route 53) to a static page in S3 if ELB unhealthy
 - [ ] Event and notification on RDS failover to secondary
 - [ ] Automated backups for RDS
 - [ ] Turn on MFA delete and versioning for logs bucket

## Security

 - [ ] ACM certificate provisioned on the ELB
 - [ ] Basic WAF
 - [ ] Resources provisioned by dedicated user account
 - [ ] IAM authentication to RDS
 - [ ] RDS encrypted at rest
 - [ ] NACL to block SSH and RDS from anything except your home IP

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

## Monitoring

 - [X] ELB logs saved to S3
 - [X] All resources tagged and displayed within a resource group (manually)
 - [X] CloudWatch dashboard (manually)
    - [X] [baseline monitoring](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/monitoring_ec2.html)
    - [X] Plus disk and memory usage custom metrics
    - [X] With dimensions to allow easily filtering metrics to just the webservers
 - [X] Ship Apache logs to CloudWatch logs
 - [ ] Alarm on unexpectedly high billing
 - [X] Alarm on zero healthy hosts
 - [X] Compliance check on worldwide port 22 access

## Tasks

 - [X] Apply stack via AWS CLI
 - [X] Export CloudWatch logs to S3
 - [X] Query logs with Athena
 - [ ] Install AWS Inspector via SSM and run an assessment
