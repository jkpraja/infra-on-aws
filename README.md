
# Capacity Management Made Easy With Amazon Auto Scaling and Load Balancer





## Authors

- [@jkpraja](https://www.github.com/octokatherine)


## Introduction
In this project, we're going to migrate an on premise architecture to the cloud.
By adding the auto scaling features, it offers capacity management experience to help customers:
- maintain a healthy fleet,
- improve application availability
- reduce costs

Auto scaling solves various cases:
1. Automates provisioning of instances,
2. Reduces paging frequency,
3. Makes it easier than ever to use spot instances,
4. Allow scaling infrastructure up and down to save cost.
## Architecture
![Topology](https://github.com/jkpraja/infra-on-aws/blob/007847abcc829ca7155f120b87d5efca10d6a54d/project%204.png)
## You'll need
- An AWS Account
- At least 2 applications, in this case, we'll use:
    - [Cilsy landing page](https://github.com/jkpraja/cilsy-landing-page)
    - [Cilsy Social Media app](https://github.com/sdcilsy/sosial-media)

The social media is an application that will need access to the database.
## What To Do
1. Create VPC that can provide enough IP numbers.
2. Create 3 subnets : 1 public and the rests are private. In this case, we need to plan about the Server High Availability too.
   The high availibity can be reached by selecting various Availibility Zones.
3. Create 2 security groups that handle public and private subnet.
   - Public Security Group : 
        - Inbound : allow HTTP, SSH
        - Outbound : allow all traffics
   - Private Security Group :
        - Inbound : allow SSH, ICMP, TCP
        - Outbound : similar with Inbound
4. Create an EC2 Instance in Public subnet. This needs to be done, so that we can ssh the instances in private subnets.

### Private Subnet 1 Installation
1. Access Public EC2 using SSH and then access EC2 Private 1 using SSH too.
2. Run 'git clone' to download the cilsy-landing-page
3. Install the NGINX and start the server
4. Move the landing page source code to /var/www
5. Test if the installation is successful locally

#### Auto Scaling Setup
1. Create AMI of the existing instance. The purpose is that whenever the Auto Scaler needs to scale up the instances, it can create new instances based on this AMI.
2. Create a new Launch Configuration. This is to choose which hardware specification we need to use for scaling up.
3. Create a new auto scaling group. The key points in this step : the scaling capacity, and load balacing.

#### Load Balancer Setup
1. Create a new classic Balancer
2. Please make sure the Balancer is on the desired VPC
3. In this phase, choosing various subnets in different AZ is preferred to provide High Availability of our applications.
4. Go to Auto Scaling Groups to add the Elastic Load Balancer that has been created recently
5. Test the load balancer if it's ready

### Private Subnet 2 Installation
1. Access EC2 Private 2 using SSH from EC2 Public
2. Download the cilsy social media application
3. Install the Apache2 and start the server
4. Move the application code to /var/www
5. Test if the installation is successful locally

#### Amazon RDS Setup
Because this application requires a database, we'll need to use Amazon RDS as a database server.

1. Create a new RDS by selecting MySQL as its engine.
2. Select the appropriate use case, since we only use this for test, then dev/test case is suitable.
3. Select the appropriate instance type and its storage.
4. Make sure this RDS is private by linking it to EC2 Private where the social media app is located.
5. Enable the enhanced performance monitoring.
6. If RDS is ready, access the MySQL server via EC2 Private Subnet 2
7. If Database is not created yet, please make sure it's created and migrate the existing data to the RDS.
8. Configure the DB and Application connection with correct parameter.
9. Test the connection between DB and application.

#### Auto Scaling Setup
1. Create AMI of the existing instance. The purpose is that whenever the Auto Scaler needs to scale up the instances, it can create new instances based on this AMI.
2. Create a new Launch Configuration. This is to choose which hardware specification we need to use for scaling up.
3. Create a new auto scaling group. The key points in this step : the scaling capacity, and load balacing.

#### Load Balancer Setup
1. Create a new classic Balancer
2. Please make sure the Balancer is on the desired VPC
3. In this phase, choosing various subnets in different AZ is preferred to provide High Availability of our applications.
4. Go to Auto Scaling Groups to add the Elastic Load Balancer that has been created recently
5. Test the load balancer if it's ready

## Final Step
The final step in this phase is mitigate the domain to Amazon Route53
and create a CNAME for a sub domain and link it to existing Load Balancers.


That's all. This project is completed.
