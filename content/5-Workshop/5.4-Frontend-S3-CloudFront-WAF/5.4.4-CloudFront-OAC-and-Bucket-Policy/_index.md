---
title : "CloudFront OAC and Bucket Policy"
date : 2024-01-01
weight : 4
chapter : false
pre : " <b> 5.4.4. </b> "
---

## Objective

Create or verify the CloudFront distribution that serves the private S3 frontend bucket through Origin Access Control.

Key settings:

| Area | Value |
|---|---|
| Origin | S3 Static Frontend Bucket |
| Origin access | Origin Access Control |
| Viewer protocol policy | Redirect HTTP to HTTPS |
| Allowed methods | GET, HEAD |
| Default root object | `index.html` |

The origin configuration shows that CloudFront points to the static frontend bucket, not the data bucket.

![CloudFront create origin](/fcaj-internship-report/images/5-Workshop/5.4-Frontend-S3-CloudFront-WAF/w-phu-15-cloudfront-create-origin.png)

OAC allows CloudFront to read private S3 objects without opening the bucket to the public Internet.

![CloudFront OAC configuration](/fcaj-internship-report/images/5-Workshop/5.4-Frontend-S3-CloudFront-WAF/w-phu-16-cloudfront-oac-config.png)

The default behavior is tuned for static content and redirects viewers to HTTPS.

![CloudFront default behavior](/fcaj-internship-report/images/5-Workshop/5.4-Frontend-S3-CloudFront-WAF/w-phu-17-cloudfront-default-behavior.png)

The default root object makes `/` return the dashboard entrypoint.

![CloudFront default root object](/fcaj-internship-report/images/5-Workshop/5.4-Frontend-S3-CloudFront-WAF/w-phu-18-cloudfront-default-root-object.png)

The deployed distribution evidence confirms that CloudFront is ready to serve the frontend.

![CloudFront distribution deployed](/fcaj-internship-report/images/5-Workshop/5.4-Frontend-S3-CloudFront-WAF/w-phu-19-cloudfront-distribution-deployed.png)

CloudFront may generate a suggested S3 bucket policy for the OAC. Keep the bucket private and allow only the distribution source ARN.

![CloudFront OAC policy prompt](/fcaj-internship-report/images/5-Workshop/5.4-Frontend-S3-CloudFront-WAF/w-phu-20-cloudfront-oac-policy-prompt.png)

The bucket policy evidence shows CloudFront service access without public read permissions.

![S3 bucket policy for OAC](/fcaj-internship-report/images/5-Workshop/5.4-Frontend-S3-CloudFront-WAF/w-phu-21-s3-bucket-policy-oac.png)

The permissions page confirms that Block Public Access remains enabled after applying the OAC policy.

![S3 permissions after OAC policy](/fcaj-internship-report/images/5-Workshop/5.4-Frontend-S3-CloudFront-WAF/w-phu-22-s3-permissions-after-policy.png)

