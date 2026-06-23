```
1. Register Domain 
2. Create Bucket 
3. Upload index.html to bucket 

{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Sid": "PublicRead",
			"Effect": "Allow",
			"Principal": "*",
			"Action": "s3:GetObject",
			"Resource": "arn:aws:s3:::atulkamble.in/*"
		},
		{
			"Sid": "AllowCloudFrontServicePrincipal",
			"Effect": "Allow",
			"Principal": {
				"Service": "cloudfront.amazonaws.com"
			},
			"Action": "s3:GetObject",
			"Resource": "arn:aws:s3:::atulkamble.in/*",
			"Condition": {
				"ArnLike": {
					"AWS:SourceArn": "arn:aws:cloudfront::535002879962:distribution/EZ4SKXS3U7F3I"
				}
			}
		}
	]
}

4. create certificate - certificate manager 
5. Cloudfront create 
6. check records in domain 
7. cloudfront >> distributions >> origins >> atulkamble.in.s3-website-us-east-1.amazonaws.com

origin path: (empty)
8. check website 
https://atulkamble.in/


```
