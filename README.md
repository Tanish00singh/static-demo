Here’s a **no-BS, click-by-click guide** to deploy a static website on AWS S3 using the **latest AWS Console UI (2025+)**. Follow exactly—most people mess this up at permissions.

---

# 🚀 STEP 1 — Create S3 Bucket

1. Go to **Amazon Web Services Console**
2. In search bar → type **S3**
3. Click **S3** → opens dashboard
4. Click **Create bucket**

### Fill details:

* **Bucket name** → must be:

  * globally unique
  * lowercase only
  * no spaces
  * example:

    ```
    tanish-static-site-2026
    ```

* **Region** → choose nearest (e.g., Mumbai → ap-south-1)

---

# 🚀 STEP 2 — Disable Block Public Access (IMPORTANT)

Scroll to **“Block Public Access settings for this bucket”**

👉 Uncheck:

* ✅ Block *all* public access

✔ Then tick:

* **“I acknowledge…”**

### Why?

Because S3 blocks public access by default.
If you don’t disable this → **your site = 403 Forbidden forever**

---

# 🚀 STEP 3 — Create Bucket

Scroll down → click **Create bucket**

Done.

---

# 🚀 STEP 4 — Upload Website Files

1. Open your bucket
2. Click **Upload**
3. Click **Add files**

Upload:

* `index.html`
* `style.css`
* `script.js`
* images, etc.

4. Click **Upload**

---

# 🚀 STEP 5 — Enable Static Website Hosting

1. Inside bucket → go to **Properties tab**
2. Scroll down → **Static website hosting**
3. Click **Edit**

### Configure:

* Enable → **ON**
* Hosting type → **Host a static website**

Fill:

* **Index document** → `index.html`
* **Error document** → `error.html` *(optional but recommended)*

4. Click **Save changes**

---

# 🚀 STEP 6 — Add Bucket Policy (CRITICAL STEP)

Now permissions.

1. Go to **Permissions tab**
2. Scroll → **Bucket Policy**
3. Click **Edit**
4. Paste this:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadGetObject",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::YOUR-BUCKET-NAME/*"
    }
  ]
}
```

👉 Replace:

```
YOUR-BUCKET-NAME
```

with your actual bucket name.

5. Click **Save changes**

---

# 🚀 STEP 7 — Access Your Website

1. Go back to **Properties**
2. Scroll to **Static website hosting**

You’ll see:

```
http://YOUR-BUCKET-NAME.s3-website-REGION.amazonaws.com
```

👉 Open it in browser — your site is LIVE.

---

# ⚠️ COMMON MISTAKES (THIS IS WHERE PEOPLE FAIL)

### ❌ 1. 403 Forbidden Error

Cause:

* Block public access still ON
* Bucket policy missing or wrong

Fix:

* Disable block public access
* Check policy ARN matches bucket name EXACTLY

---

### ❌ 2. AccessDenied XML Page

Cause:

* Missing `index.html`

Fix:

* Ensure file name is EXACT:

```
index.html
```

(not Index.html, not index.htm)

---

### ❌ 3. CSS/JS Not Loading

Cause:

* Wrong file paths

Fix:
Use relative paths:

```html
<link rel="stylesheet" href="style.css">
```

---

### ❌ 4. Bucket Name Mistake

Cause:

* Uppercase / spaces

Fix:
Use:

```
tanish-site-2026
```

---

# 💡 PRO TIP (IMPORTANT)

S3 static hosting:

* ❌ No backend (Node.js, PHP won’t work)
* ✅ Only HTML, CSS, JS

If you need backend → use:

* EC2
* Elastic Beanstalk
* or API Gateway + Lambda

---

# ✅ FINAL RESULT

You now have:

* Public static website
* Hosted globally via AWS S3
* Accessible via URL

---

If you want next level:
👉 I can show you how to connect this with **CloudFront + custom domain + HTTPS (production setup)**.
