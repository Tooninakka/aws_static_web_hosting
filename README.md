DEtailed Document:
  https://docs.google.com/document/d/1woW6C-IE-84fEY-UDrOmHEjaitZ66oY6Jl6boJzNnrM/edit?usp=sharing 

Use Case Description
Host a secure, low-latency static website with global distribution.
In this document, I’m going to host a secure, low-latency static website with global distribution. 
You can find a complete guide, step-by-step implementation with result-oriented screenshots, outcomes, and deliverables below,
⚒️ Tools & Services Used:
Amazon S3
Amazon Route 53
Amazon Certificate Manager (ACM)
Amazon CloudFront
Amazon IAM
🖼️ Architectural Diagram:


🛠️ Step-by-Step Guide
Step 1: Register a Domain 
1.1 Register a Domain with Route 53
Navigate to the Route 53 Console.


Register a new domain (e.g., tooniawsprojects.com) or use an existing one.


A hosted zone will be automatically created for your domain.


Step 2: Create and Configure S3 Buckets
2.1. Create Two S3 Buckets
One for website content: www.tooniawsprojects.com


One for redirection: tooniawsprojects.com


2.2. Enable Static Website Hosting on www.tooniawsprojects.com
Go to Properties → Enable Static website hosting


Enter index.html as the index document.




2.3. Upload Website Files
Upload files like index.html, style.css, etc., to the www.tooniawsprojects.com bucket.



2.4 Disable the Block all public access
Go to www.tooniawsprojects.com bucket, go to permissions
Edit Block public access(bucket settings)
Disable the Block all public access

2.5. Set Public Read Permissions (if not using CloudFront OAI)
Add this bucket policy (replace www.example.com with your bucket name):
{
  "Version": "2012-10-17",
  "Statement": [{
    "Sid": "PublicReadGetObject",
    "Effect": "Allow",
    "Principal": "*",
    "Action": "s3:GetObject",
    "Resource": "arn:aws:s3:::www.tooniawsprojects.com/*"
  }]
}


Step 3: Configure Route 53 DNS Records
Go back to the Route 53 Console.
3.1 Add Alias Records
Record 1: A Record for www.tooniawsprojects.com


Alias: Yes → Target: CloudFront distribution


Record 2: A Record for tooniawsprojects.com


Alias: Yes → Target: same CloudFront distribution or redirect bucket









 
Step 4: Test Your Website



As you can see, the above website is not secure, so we have to request an SSL Certificate to make the website secure.
Step 5: Request SSL Certificate Using AWS Certificate Manager (ACM)
5.1. Open ACM in us-east-1
CloudFront only accepts certificates from the N. Virginia region


5.2. Request a Public Certificate
Add www.tooniawsprojects.com and tooniawsprojects.com


Choose DNS Validation


5.3. Validate Domain
ACM provides CNAME records → add them to your Route 53 Hosted Zone


Wait until the certificate status is Issued.








Go to Route 53, Hosted Zones, you can find two more records, which we’ve created.
Records with CNAME



An SSL Certificate is successfully issued.



Step 6: Configure CloudFront CDN
6.1. Create a CloudFront Distribution
Origin domain: Select www.tooniawsprojects.com.s3.amazonaws.com


Set Viewer Protocol Policy to Redirect HTTP to HTTPS


Choose Custom SSL Certificate: Select the one from ACM





6.2 Set Behavior
Cache policy: Use default or optimized caching


Enable compression (optional)



Do the same for another bucket - tooniawsprojects.com

CloudFront Distributions have been created and deployed successfully.








6.3 Change the Protocol to HTTPS in Amazon S3
Go back to S3, select the www.tooniawsprojects.com bucket
Go to the properties, and edit the static website hosting
Change the Protocol from HTTP to HTTPS.



6.4 Test the Website
Copy the Domain name from the CloudFront Distribution of www.tooniawsprojects.com.
Run the Domain name on the browser.


As you can see in the above picture, the website is successfully running, we’re getting the response from the HTTPS protocol, and the website is secured with an SSL Certificate, but we’ve to change the domain name to our created hostname. We will do that in the next step.
Step 7: Changing the Domain Name
7.1 Edit the A record in the Route 53
Go to Amazon Route 53
Select the A-type record of both records
tooniawsprojects.com
www.tooniawsprojects.com
Edit the records
Change the Route Traffic to
From Alias S3 end to Alias CloudFront Distribution.



7.2 Test the Website
Now, finally, test the website using the created hostname (www.tooniawsprojects.com).


📦 Deliverables
Live Website: Accessible at https://www.example.com.


CloudFront Distribution: Configured with your S3 bucket as the origin and SSL certificate attached.


SSL Certificate: Issued and active in ACM.


Route 53 Configuration: DNS records pointing your domain to the CloudFront distribution.
🐈‍⬛ GitHub:
You can find a complete document and static website files in the GitHub repo below
https://github.com/Tooninakka/aws_static_web_hosting 




