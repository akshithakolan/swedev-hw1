# Assignment 1 : Student e-portfolio using AWS S3 and EC2

Part 1: Static Website Hosting (Amazon S3)
Homepage URL (S3 Hosted):
http://swe645hwonee.s3-website.us-east-2.amazonaws.com

Part 2: Survey Form Hosting (Amazon EC2)
Survey Form URL (EC2 Hosted):
http://3.21.75.162/survey.html

### Instructions to Access the Application
Visit the homepage by clicking the S3 URL provided above.
To access the survey form, click on the link provided on the homepage, which directs to the survey form hosted on an EC2 instance.
You can also directly visit the survey form by using the EC2 URL provided.

### Part 1
Step-by-Step Procedure: Hosting a Static Website on Amazon S3 
Log in to AWS Console:

Open AWS Management Console and sign in to your account.
Create an S3 Bucket:

Navigate to S3 Service.
Click Create Bucket.
Choose a unique bucket name (e.g., swe645hwonee).
Choose the appropriate region (e.g., us-east-2).
Uncheck the Block all public access option, as the website will need to be public.
Click Create Bucket.
Upload Website Files:

Click on your newly created bucket.
Navigate to the Objects tab and click Upload.
Upload your HTML, CSS, and image files, including your index.html (homepage) and other necessary files.
Enable Static Website Hosting:

In the Properties tab of your S3 bucket, scroll down to Static website hosting.
Enable static website hosting.
Set index.html as the Index document.
Set a custom error page (e.g., error.html) if desired.
Note the S3 Endpoint URL (e.g., http://swe645hwonee.s3-website.us-east-2.amazonaws.com).
Set Permissions:

Go to the Permissions tab of your S3 bucket.
In Bucket Policy, add the following policy to make the bucket accessible to the public:
```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::swe645hwonee/*"
    }
  ]
}
```
Save the bucket policy.
Test Your Website:

Access your static website via the S3 Endpoint URL you noted in step 4.

### Part 2: 

Step-by-Step Procedure: Hosting a Survey Form on Amazon EC2 
Launch an EC2 Instance:

In the AWS Management Console, navigate to EC2 under the compute section.
Click Launch Instance.
Choose the Amazon Linux 2 AMI (or Ubuntu) as the base operating system.
Select the t2.micro instance type (eligible for free tier).
Configure the instance settings, and in the Security Group section, allow HTTP (port 80) and SSH (port 22).
Connect to EC2 Instance:

Once the instance is running, connect to it using SSH. Open your terminal and run:
bash
Copy code
ssh -i /path/to/your-key.pem ec2-user@<public-ip>
Replace <public-ip> with the public IP address of your instance.
Install Web Server (Apache):

After connecting to the instance, install Apache web server:
```
sudo yum update -y
sudo yum install httpd -y
```
Start the Apache service:
```
sudo systemctl start httpd
sudo systemctl enable httpd
```
Upload the Survey Form Files:

Create a directory for your website files:
```
sudo mkdir /var/www/html/survey
```
Upload your survey.html file and other assets using SCP or any file transfer tool. For example:
```
scp -i /path/to/your-key.pem survey.html ec2-user@<public-ip>:/var/www/html/survey/
```
Move the files to the appropriate directory:
```
sudo mv survey.html /var/www/html/survey/
```
Configure EC2 Security Group:

Ensure your EC2 security group allows inbound traffic on port 80 (HTTP).
Test Your Survey Form:

Visit your public IP address in a browser (e.g., http://<public-ip>/survey.html).
