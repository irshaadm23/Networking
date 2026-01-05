# networking
Deploying and exposing web server using EC2  instance , NGINX, security groups and DNS

## Overview
This project demonstrates core networking fundamentals by deploying a Linux EC2 instance on AWS, installing NGINX, and exposing it to the internet over HTTP. A custom domain was purchased and configured using Cloudflare DNS to resolve to the EC2 public IP. The goal was to understand how DNS, IP addressing, security groups, and web servers work together to serve web traffic end-to-end.

## Architecture & Request Flow
Browser
→ DNS (Cloudflare)
→ EC2 Public IP
→ Security Group (port 80)
→ NGINX
→ HTTP response

When a user visits the domain, DNS resolves the domain name to the EC2 public IPv4 address. The request reaches AWS, where the EC2 security group allows inbound HTTP traffic on port 80. NGINX running on the EC2 instance receives the request and serves the default HTML page.

## Implementation Steps
- Launched an EC2 instance using Amazon Linux.
- Configured a security group to allow inbound HTTP (port 80) and SSH (port 22).
- Installed and started NGINX as a systemd service.
- Verified HTTP access via the EC2 public IP before configuring DNS.
- Created DNS A and CNAME records in Cloudflare to map the domain to the EC2 instance.

## Linux Commands Used
sudo yum install -y nginx

sudo systemctl start nginx

sudo systemctl enable nginx

sudo systemctl status nginx


## Verification & Evidence 
- EC2 instance running in AWS console confirms compute is active. 
- `systemctl status nginx` confirms the web server is running.
- Accessing the EC2 public IP in a browser confirms HTTP connectivity. 
  http://3.8.92.23
- Cloudflare DNS records confirm domain-to-IP resolution.
- Accessing the domain displays the NGINX welcome page, proving end-to-end functionality.
  http://irshaadm.com
  
## Debugging & Failure Scenarios
- If port 80 were blocked in the security group, the browser would time out despite NGINX running.
- If NGINX were stopped, the connection would be refused because no process would be listening on port 80.
- If DNS were misconfigured, the domain would fail to resolve even though the EC2 IP works.

## Key Learnings
- DNS is a naming layer and does not handle connectivity itself.
- Security groups act as a network firewall before traffic reaches the OS.
- Verifying services by IP before DNS simplifies debugging.
