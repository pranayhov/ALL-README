# 🌐 Subdomain Setup with AWS Elastic Beanstalk

## ✅ Prerequisites
- An application deployed on AWS Elastic Beanstalk
- Access to your domain registrar (e.g., Hostinger)

---

## 1. Get Your Elastic Beanstalk Environment URL
After deploying your app, you will get a URL like:

```
http://my-app-env.eba-xyz123.us-east-1.elasticbeanstalk.com
```

Keep this handy — you'll point your subdomain to it.

---

## 2. Add a CNAME Record on Hostinger (or your DNS Provider)

1. Log in to Hostinger
2. Navigate to **DNS Zone Editor** for your domain `reidatasolutions.com`
3. Add a **CNAME Record**:
   - **Type**: `CNAME`
   - **Name/Host**: `lab`
   - **Value/Target**: `my-app-env.eba-xyz123.us-east-1.elasticbeanstalk.com`
   - **TTL**: Default or 1400

> 🔄 This will make `lab.reidatasolutions.com` point to your EB environment.

---

## 3. Enable HTTPS (Optional but Recommended)

### Option A: Use AWS Certificate Manager (ACM)

1. Go to **AWS Certificate Manager**
2. Click **Request a Certificate**
3. Enter `lab.reidatasolutions.com` as the domain
4. Validate using DNS (AWS provides a CNAME entry)
5. Go to EC2 → **Load Balancers** → **Listeners**
   - Add Listener for port **443 (HTTPS)**
   - Attach the validated SSL certificate

### Option B: Use Certbot on EC2 (Nginx Managed)

If you're manually managing NGINX on an EC2 server:

```bash
sudo apt install snapd
sudo snap install --classic certbot
sudo certbot --nginx -d lab.reidatasolutions.com
```

> Certbot automatically configures NGINX and renews the certificate.

---

## 4. Verify Everything

1. Visit: `https://lab.reidatasolutions.com`
2. Check if SSL is active (🔒 lock icon in browser)
3. Make sure it loads the Elastic Beanstalk app

---

## 📌 Tips
- Use **Route 53** if you want tighter AWS integration
- Use `pm2` for running backend apps if deploying outside EBS
- Make sure port 443 is open in EC2 security group if using your own EC2 setup

---

## ✅ Done!
Your subdomain is now pointing to your AWS-hosted app with optional HTTPS 🔐.
