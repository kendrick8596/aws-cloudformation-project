CloudFormation Template for Automated Infrastructure Setup
Overview
This CloudFormation template automates the deployment of a fully functioning infrastructure on AWS. It provisions EC2 instances, EBS volumes, an RDS PostgreSQL database, an Application Load Balancer (ALB), auto-scaling groups, and associated networking components, allowing for a robust and scalable environment. The infrastructure supports an application with multiple EC2 instances that can connect to an RDS PostgreSQL database, leveraging AWS services such as EBS for persistent storage.

Components
1. VPC and Networking
VPC: A Virtual Private Cloud (VPC) is created with two subnets:

A public subnet for the bastion host.

A private subnet for the application instances.

Internet Gateway: Allows communication between the VPC and the internet.

Route Tables: Associated with the public and private subnets for routing traffic correctly.

Security Groups: Separate security groups are defined for EC2 instances, the application, and the RDS database.

2. EC2 Instances
Bastion Host: A public EC2 instance used to securely SSH into private instances. It is placed in the public subnet and connects to the private subnet instances.

Application Instances: EC2 instances in the private subnet that run your application. These instances are configured with user data to install the PostgreSQL client and connect to the RDS PostgreSQL database.

3. EBS (Elastic Block Store) Volumes
EBS volumes are created and attached to the EC2 instances to provide persistent storage. Unlike EFS, EBS volumes are tied to a single EC2 instance at a time, making them ideal for storing data directly on an instance.

4. RDS PostgreSQL Database
An RDS PostgreSQL instance is created for the application. The connection details are managed via AWS Secrets Manager to securely store the database credentials.

5. Application Load Balancer (ALB)
An Application Load Balancer (ALB) is set up to distribute traffic across the EC2 instances. It ensures high availability and load balancing for the application.

6. Auto Scaling Group
An Auto Scaling group is configured to automatically scale the application instances based on demand. The Auto Scaling group uses a launch template to define how new instances are created.

7. IAM Roles and Policies
IAM roles and policies are set up for the EC2 instances, allowing them to interact with services like S3, CloudWatch, and Systems Manager (SSM) securely.

8. User Data Scripts
EC2 instances are configured using user data to install the PostgreSQL client and configure the instances for database connectivity. The user data scripts ensure the instances are ready for application interaction with the RDS PostgreSQL database.

Setup Instructions
Prepare the CloudFormation Template:

Save the CloudFormation template to a .yaml or .json file.

Launch the Stack:

Go to the AWS Management Console.

Navigate to CloudFormation and create a new stack.

Upload the saved CloudFormation template and follow the prompts to create the stack.

Monitor the Stack Creation:

Once the stack creation starts, monitor the progress via the CloudFormation console.

CloudFormation will create all the necessary resources, and the status will be updated as each resource is provisioned.

Access the Bastion Host:

After the stack creation completes, you can SSH into the bastion host using the provided private key.

Use the bastion host to access the private instances by configuring appropriate security groups.

Verify the Application:

Once the EC2 instances are running, the Application Load Balancer will distribute traffic to them.

You can test the application by accessing the ALB URL provided in the output.

PostgreSQL Client Access:

The PostgreSQL client will be installed on the application instances via the user data script. You can connect to the RDS PostgreSQL instance by running psql from the EC2 instances, using the credentials stored in Secrets Manager.

Customization
Database Credentials: You can change the database username and password by modifying the parameters in the Secrets Manager configuration.

Instance Type: Modify the InstanceType parameter in the EC2 instances to scale the application as needed.

Auto Scaling Configuration: Adjust the scaling parameters for the Auto Scaling group based on your expected traffic load.

EBS Volume Configuration: Modify the size or type of the EBS volumes based on your application's storage requirements.

Conclusion
This template is a scalable solution that automates the setup of a cloud infrastructure for your application. It leverages several AWS services to ensure high availability, security, and scalability. EBS provides persistent block storage directly attached to the instances, while RDS PostgreSQL handles your database needs.
