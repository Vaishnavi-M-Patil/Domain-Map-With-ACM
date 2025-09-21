# Domain Map With ACM

## Access private S3 bucket through cloudfront:
 - Enable Block All Public Access.
 - Create CloudFront Distribution
 - Set your S3 bucket as the Origin.
 - Use **Origin Access Control** (recommended)
  ( OAC is the new and recommended way for CloudFront to securely access private S3 buckets)
 - Update **S3 Bucket Policy**: Allow only CloudFront to read objects.
 - Set **Default Root Object**
      - In CloudFront console → your distribution → General settings → set Default Root Object to: **index.html**
      - This ensures when someone visits: `https://dXXXXXXXX.cloudfront.net/`
      - they see index.html from your bucket instead of a 403 error.


## Custom Domain Mapping with cloudfront:
  - Refer for settings: [Domain-Map](https://www.youtube.com/watch?v=99H96S-Neq0)
### 1. Buy or Use an Existing Domain
  - You need a registered domain (e.g., from Route 53, GoDaddy, Namecheap, etc.).
  - Create a **public hosted zone** for your domain name.
  - Get Nameservers from **Route 53** and Update at Your **Domain Registrar**.
      - In AWS Console → Route 53 → Hosted Zones → yourdomain.com.
      - You’ll see 4 NS (Name Server) records
      - Copy these NS values.
  - Log in to your registrar (GoDaddy) → Find **DNS Management / Nameserver** Settings → Choose** Custom Nameservers** → Paste the **4 nameservers from Route 53** → Save/Update.

### 2. Request/Validate SSL Certificate (Required for HTTPS)
 - Go to AWS Certificate Manager (ACM).
 - Request a public certificate for your custom domain (e.g., free-domain.shop or *.free-domain.shop).
 - **Important:** Certificate must be created in us-east-1 (N. Virginia) for CloudFront.

DNS Validation (recommended): Add the provided CNAME record in Route 53.

Update CloudFront Distribution
Go to your CloudFront distribution → Settings.

Under Alternate Domain Names (CNAMEs), add your custom domain (free-domain.shop).
Under Custom SSL Certificate, choose the ACM certificate you created.
Save changes.

DNS Configuration 
Go to Route 53 → Hosted Zones → free-domain.shop
Create an Alias Record:
Name: free-domain.shop
Type: Alias (A record in Route 53)
Value: CloudFront distribution domain (d1234abcd.cloudfront.net)

Open browser: `https://free-domain.shop`

You don’t have to use only S3 as a CloudFront origin — you can also use an EC2 instance.
Add website content in `/var/www/html/` directory.

Copy **dns name** of ec2 and add it in the origin domain.

## Screenshots

### Created an S3 bucket to host static website files.
![createBucket](https://github.com/Vaishnavi-M-Patil/Domain-Map-With-ACM/blob/main/ACM/1-createBucket.png)

### Enabled static website hosting on the S3 bucket.
![enableStaticWebsite](https://github.com/Vaishnavi-M-Patil/Domain-Map-With-ACM/blob/main/ACM/2-enableStaticWebsite.png)

### Blocked public access settings for the S3 bucket.
![blockPublicAccess](https://github.com/Vaishnavi-M-Patil/Domain-Map-With-ACM/blob/main/ACM/3-blockPublicAccess.png)

### Created a Route 53 hosted zone for the domain.
![createHostedZone](https://github.com/Vaishnavi-M-Patil/Domain-Map-With-ACM/blob/main/ACM/4-createHostedZone.png)

### Updated domain’s nameservers with Route 53 NS records.
![changeNS](https://github.com/Vaishnavi-M-Patil/Domain-Map-With-ACM/blob/main/ACM/5-changeNS.png)

### Requested an ACM SSL certificate and added records in Route 53.
![createACMCertificate](https://github.com/Vaishnavi-M-Patil/Domain-Map-With-ACM/blob/main/ACM/6-createACMCertificateAndAddRecordInHostedZone.png)

### Created a CloudFront distribution for secure content delivery.
![createCloudfrontDistribution](https://github.com/Vaishnavi-M-Patil/Domain-Map-With-ACM/blob/main/ACM/7-createCloudfrontDistribution.png)

### Added an alternate domain (CNAME) to CloudFront.
![addAlternateDOmain](https://github.com/Vaishnavi-M-Patil/Domain-Map-With-ACM/blob/main/ACM/8-addAlternateDOmain.png)

### Set the default root object (index.html) in CloudFront.
![addDefaultRootObject](https://github.com/Vaishnavi-M-Patil/Domain-Map-With-ACM/blob/main/ACM/9-addDefaultRootObject.png)

### Edited the CloudFront origin to point to the S3 bucket.
![editOrigin](https://github.com/Vaishnavi-M-Patil/Domain-Map-With-ACM/blob/main/ACM/10-editOrigin.png)

### Applied an S3 bucket policy to allow only CloudFront access.
![addBucketPolicy](https://github.com/Vaishnavi-M-Patil/Domain-Map-With-ACM/blob/main/ACM/11-addBucketPolicy.png)

### Created a Route 53 record pointing the domain to CloudFront.
![createRecordForCLoudfront](https://github.com/Vaishnavi-M-Patil/Domain-Map-With-ACM/blob/main/ACM/12-createRecordForCLoudfront.png)

### Verified the final hosted zone configuration.
![finalHostedZone](https://github.com/Vaishnavi-M-Patil/Domain-Map-With-ACM/blob/main/ACM/13-finalHostedZone.png)

### Accessed the website using the custom domain secured with HTTPS.
![website](https://github.com/Vaishnavi-M-Patil/Domain-Map-With-ACM/blob/main/ACM/14-website.png)
