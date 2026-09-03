# AWS S3 

Amazon S3 (Simple Storage Service) is a highly scalable, secure, and durable **cloud object storage service** designed to store and retrieve any amount of data from anywhere on the web.

## Key Concepts

- **Buckets:** Top-level containers for objects. Bucket names must be globally unique across all AWS accounts and regions. Names cannot contain underscores (`_`), but hyphens (`-`) are allowed.
- **Objects:** Individual files stored in a bucket. Each object is identified by a unique key (path).
- **Regions:** Buckets are created in a specific AWS region. Choose a region geographically close to your users to reduce latency.

# Setting up AWS S3

1. From AWS Console, search S3 and then click create Bucket. Give a name (e.g. `re-s3-images`). S3 does not allow underscores `_`.

2. After creating an S3 bucket, click on "Permissions" tab.

3. Then click on Edit Bucket Policy.

4. Then we need to edit the policy like this:

   ```json
   {
     "Version": "2012-10-17",
     "Statement": [
       {
         "Sid": "Statement1",
         "Effect": "Allow",
         "Principal": "*",
         "Action": "s3:GetObject",
         "Resource": "arn:aws:s3:::res3-images-29/*"
       }
     ]
   }
   ```

   > **Security Warning:** The policy above grants public read access to all objects in the bucket. Only use this for genuinely public assets (e.g., product images). For sensitive files, use presigned URLs instead (see below).

5. Next, click on "Objects" Tab.

6. Drag the files or images we want to upload and hit "Upload".

---

## Important S3 Features to Know

### Bucket Versioning
Enabling versioning keeps multiple variants of an object in the same bucket. If a file is accidentally deleted or overwritten, you can restore a previous version. Enable it under **Properties** > **Bucket Versioning**.

### Server-Side Encryption (SSE)
S3 can encrypt objects at rest automatically. The main options:
- **SSE-S3:** AWS manages the encryption keys (simplest, free).
- **SSE-KMS:** You manage keys via AWS KMS (gives you audit trails of who accessed your keys).

Enable default encryption under **Properties** > **Default encryption** so every new upload is encrypted automatically.

### CORS (Cross-Origin Resource Sharing)
If your frontend (e.g., hosted on Amplify/Vercel) needs to upload files directly to S3 or read objects via the browser, you must configure CORS on the bucket. Go to **Permissions** > **Cross-origin resource sharing (CORS)** and add a configuration like:

```json
[
  {
    "AllowedHeaders": ["*"],
    "AllowedMethods": ["GET", "PUT", "POST"],
    "AllowedOrigins": ["https://your-frontend-domain.com"],
    "ExposeHeaders": []
  }
]
```

### Presigned URLs
For private objects (e.g., user-uploaded documents), instead of making the bucket public, generate a **presigned URL** from your backend. This URL grants temporary, time-limited read (or write) access to a specific object without exposing the bucket publicly. Use the AWS SDK (`@aws-sdk/client-s3` + `@aws-sdk/s3-request-presigner`) on your backend to generate these.