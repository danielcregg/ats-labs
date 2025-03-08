# Lab: Adding HTTPS Certificates via Load Balancer and Targets to EC2 Instance
## Objective
Configure an AWS Application Load Balancer with HTTPS certificates and targets to serve an EC2 instance hosting a web application.

## Prerequisites
- An AWS account with access to EC2, Elastic Load Balancing, and AWS Certificate Manager.
- An existing EC2 instance running a web server (e.g., Apache or Nginx).
- A registered domain name.

## Step 1: Obtain an SSL/TLS Certificate
### Using AWS Certificate Manager (ACM)

1. **Navigate to AWS Certificate Manager**:
   - Go to the AWS Management Console and select the AWS Certificate Manager service.

2. **Request a Certificate**:
   - Click on "Import a certificate" or "Request a certificate" depending on your needs.
   - Follow the wizard to request a certificate for your domain.

3. **Validate Domain Ownership**:
   - Use email validation or DNS validation to prove domain ownership.

4. **Wait for Certificate Issuance**:
   - Once validated, AWS will issue your certificate.

## Step 2: Create a Target Group
### For Your EC2 Instance

1. **Navigate to Elastic Load Balancing**:
   - In the AWS Management Console, go to the Elastic Load Balancing service.

2. **Create a Target Group**:
   - Click on "Target groups" and then "Create target group".
   - Choose "Instances" as the target type.
   - Set the protocol to HTTP and port to 80 (or your web server's port).
   - Select your EC2 instance.

3. **Configure Health Checks**:
   - Set the health check protocol to HTTP and path to `/` (or another path that returns a 200 status code).
   - Adjust other health check settings as needed.

## Step 3: Create an Application Load Balancer
### With HTTPS Listener

1. **Create a Load Balancer**:
   - In the Elastic Load Balancing dashboard, click on "Load balancers" and then "Create load balancer".
   - Choose "Application Load Balancer".
   - Set the scheme to "internet-facing".

2. **Configure Listeners**:
   - Create an HTTPS listener on port 443.
   - Select your SSL/TLS certificate from AWS Certificate Manager.
   - Forward traffic to your target group.

3. **Add HTTP Listener (Optional)**:
   - Create an HTTP listener on port 80 if needed.
   - You can redirect HTTP to HTTPS using a rule.

4. **Select Availability Zones**:
   - Ensure the load balancer is enabled in the same AZ as your EC2 instance.

## Step 4: Configure Security Groups
### Allow Traffic Between Load Balancer and EC2

1. **Update EC2 Security Group**:
   - Ensure the security group associated with your EC2 instance allows inbound traffic on port 80 from the load balancer's security group.

2. **Update Load Balancer Security Group**:
   - Ensure the security group associated with your load balancer allows inbound traffic on ports 80 and 443 from anywhere (0.0.0.0/0).

## Step 5: Update A Records in Hosted Zone
### Point to Load Balancer

1. **Navigate to Route 53**:
   - Go to the AWS Management Console and select the Route 53 service.

2. **Select Your Hosted Zone**:
   - Choose the hosted zone that corresponds to your domain name.

3. **Create or Update A Records**:
   - Click on "Create record set" or edit an existing A record.
   - Set the **Name** to your domain name (e.g., `randombusiness.tech`) or a subdomain if needed.
   - Set the **Type** to A - IPv4 address.
   - Set the **Alias** to "Yes".
   - In the **Alias target** dropdown, select your load balancer's DNS name (e.g., `lbWebServerAuto-1810038198.eu-west-1.elb.amazonaws.com`).

4. **Save Changes**:
   - Click "Create records" or "Save changes" to apply the updates.

### Using AWS CLI

```

aws route53 change-resource-record-sets --hosted-zone-id ZONE_ID --change-batch '{
"Changes": [
{
"Action": "UPSERT",
"ResourceRecordSet": {
"Name": "randombusiness.tech",
"Type": "A",
"AliasTarget": {
"DNSName": "lbWebServerAuto-1810038198.eu-west-1.elb.amazonaws.com",
"HostedZoneId": "Z32O12XQLNTSW2",
"EvaluateTargetHealth": false
}
}
}
]
}'

```

Replace `ZONE_ID` with your actual hosted zone ID.

## Step 6: Test Your Setup
### Verify HTTPS Connection

1. **Access Your Website**:
   - Use the load balancer's DNS name to access your website over HTTPS.

2. **Verify Certificate**:
   - Check that the SSL/TLS certificate is correctly applied and trusted by your browser.

## Step 7: Update WordPress Configuration (If Applicable)
### Update Site URLs

1. **Access WordPress Admin**:
   - Log in to your WordPress dashboard.

2. **Update Site URLs**:
   - Go to **Settings > General**.
   - Update both **WordPress Address (URL)** and **Site Address (URL)** to use HTTPS.

## Conclusion
You have successfully configured an Application Load Balancer with HTTPS certificates and targets to serve an EC2 instance hosting a web application. Ensure all resources are correctly configured and test your setup thoroughly.

---

### Additional Tips

- **Monitor Load Balancer Logs**:
  - Use AWS CloudWatch to monitor load balancer logs for errors or unusual patterns.

- **Use AWS CLI for Automation**:
  - Use AWS CLI commands to automate tasks like creating load balancers and target groups.

- **Security Best Practices**:
  - Regularly review security group configurations and ensure they follow least privilege principles.
```

This lab guide covers all the necessary steps to set up an Application Load Balancer with HTTPS certificates and targets for an EC2 instance, including updating DNS records and WordPress configurations if applicable.
  
