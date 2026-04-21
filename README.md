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

Here’s a **clean, click-by-click guide** to deploy a **static website using AWS Amplify Hosting (latest UI ~2025+)** — from GitHub → live URL → CI/CD.

---

# 🚀 1. Prerequisites (Do this first)

* AWS account (logged in)
* GitHub repo with your website
  (HTML/CSS/JS or React, etc.)

---

# 🔗 2. Connect GitHub Repository (Click-by-click)

### Step 1: Open Amplify

1. Go to **AWS Console**
2. Search → **Amplify**
3. Click **AWS Amplify**
4. Click **“Create new app”**
5. Click **“Host web app”**

---

### Step 2: Choose GitHub

1. Select **GitHub**
2. Click **Next**
3. Click **Authorize GitHub**
4. Grant permissions (important!)

👉 If repo not visible → permission issue (fix later)

---

### Step 3: Select Repository

1. Choose your **Repository**
2. Choose **Branch** (usually `main`)
3. Click **Next**

📌 Amplify directly connects repo + branch for deployment ([AWS Documentation][1])

---

# ⚙️ 3. Configure Build Settings

### Auto Detection (Default)

* Amplify auto-detects framework (React, Vue, HTML)
* Generates `amplify.yml`

👉 Works for:

* Static HTML → no config needed
* React → auto build detected ([Medium][2])

---

## ✨ Sample Build Settings

### 🟢 Static HTML (NO build needed)

```yaml
version: 1
frontend:
  phases:
    build:
      commands: []
  artifacts:
    baseDirectory: /
    files:
      - '**/*'
```

---

### ⚛️ React (Vite / CRA)

```yaml
version: 1
frontend:
  phases:
    preBuild:
      commands:
        - npm install
    build:
      commands:
        - npm run build
  artifacts:
    baseDirectory: dist   # or build
    files:
      - '**/*'
```

---

### Step 4: Continue

1. Click **Next**
2. Review settings
3. Click **Save and deploy**

---

# 🚀 4. Deploy Website (Automatic)

Now Amplify will:

* Install dependencies
* Build your app
* Deploy to CDN

👉 Takes ~2–5 minutes ([AWS Documentation][1])

You’ll see:

* Build logs
* Deployment status: **Provisioning → Building → Deploying → Live**

---

# 🌍 5. Access Live URL

After deployment:

1. Go to **Amplify Dashboard**
2. Click your app
3. Click **“Visit deployed URL”**

You’ll get:

```
https://main.xxxxxx.amplifyapp.com
```

👉 Your site is now LIVE globally (CDN hosted) ([AWS Documentation][1])

---

# 🔁 6. CI/CD (Automatic Deploy Explained)

This is the best part.

### How it works:

1. You push code → GitHub
2. Amplify detects change
3. Automatically:

   * Pulls code
   * Builds app
   * Deploys new version

👉 No manual steps needed ([Medium][2])

### Example:

```bash
git add .
git commit -m "update UI"
git push origin main
```

➡️ Amplify auto rebuilds + redeploys

---

# 🌐 7. Add Custom Domain (Optional)

### Step-by-step:

1. Open Amplify app dashboard
2. Click **Domain management**
3. Click **Add domain**
4. Enter domain (e.g. `yourdomain.com`)
5. Click **Configure domain**

---
