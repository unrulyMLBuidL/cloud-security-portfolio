# Lab: Host a Static Website on Amazon S3

**Date:** 29/04/2026
**Environment:** AWS SimuLearn (ephemeral lab)
**Scenario:** A company needs a highly available way to host a static weather 
website that stays accessible during peak traffic.

---

## Why S3 for Static Website Hosting?

S3 is ideal here because:
- It is regionally redundant by default — data is stored across multiple 
  Availability Zones automatically
- It scales infinitely during peak traffic with no server management needed
- No web server (EC2, Nginx, Apache) required — S3 serves the files directly
- Cost is based only on storage and requests — not on uptime

This directly solves the scenario: high availability + no bottleneck at peak hours.

---

## What I Did

1. Opened S3 and identified the pre-provisioned bucket in the SimuLearn environment
2. Renamed the main HTML file to `waves.html` (the index document)
3. Renamed the error HTML file to `error.html` (the error document)
4. Enabled static website hosting on the bucket and set the index and error documents
5. Copied the S3 website endpoint URL
6. Pasted the URL into a browser and confirmed the website loaded successfully

---

## Screenshots

![S3 Buckets Page](screenshots/01-s3-buckets-overview.png)
![Renaming index file to waves.html](screenshots/02-rename-index-waves.png)
![Renaming error file to error.html](screenshots/03-rename-error.png)
![Copying the S3 website endpoint URL](screenshots/04-copy-endpoint-url.png)
![Website loading in browser](screenshots/05-website-live-in-browser.png)

---

## Security Observations

- The bucket must have **Block Public Access turned off** for static website 
  hosting to work; in production this should be paired with CloudFront 
  so the bucket itself stays private and only CloudFront can access it
- There is **no HTTPS** on a raw S3 website endpoint; production deployments 
  should always sit behind CloudFront with an SSL certificate (ACM)
- No authentication or WAF is applied here; for a real weather site with 
  peak traffic, adding CloudFront + AWS Shield Standard gives DDoS protection 
  at no extra cost
  
  ---

## What I Would Do Differently in Production

1. Keep the S3 bucket private; use CloudFront as the CDN in front of it
2. Enable versioning on the bucket to allow rollback if a bad file is deployed
3. Use a custom domain with Route 53 + ACM certificate for HTTPS
4. Enable S3 server access logging to track who is requesting which files
