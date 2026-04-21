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

Good — now that nginx is installable, here’s the **final clean sequence from connect → live website using GitHub**. No fluff, just the exact flow.

---

# 🚀 FULL DEPLOYMENT (EC2 + Nginx + GitHub)

## 🔹 0. Connect

EC2 → Instance → **Connect → EC2 Instance Connect → Connect**

---

# ⚙️ 1. Fix repos (your case — already done, keep it here for completeness)

```bash
sudo add-apt-repository main
sudo add-apt-repository universe
sudo add-apt-repository restricted
sudo add-apt-repository multiverse

sudo rm -rf /var/lib/apt/lists/*
sudo apt clean
sudo apt update
```

---

# ⚙️ 2. Install required software

```bash
sudo apt install nginx git -y
```

---

# ⚙️ 3. Start Nginx

```bash
sudo systemctl start nginx
sudo systemctl enable nginx
```

👉 Quick check:

```bash
sudo systemctl status nginx
```

You want: **active (running)**

---

# ⚙️ 4. Go to web root

```bash
cd /var/www
```

---

# ⚙️ 5. Remove default site

```bash
sudo rm -rf html
```

---

# ⚙️ 6. Clone your GitHub repo

```bash
sudo git clone https://github.com/YOUR_USERNAME/YOUR_REPO.git html
```

---

# ⚙️ 7. Fix permissions (don’t skip)

```bash
sudo chown -R www-data:www-data /var/www/html
sudo chmod -R 755 /var/www/html
```

---

# ⚙️ 8. Restart Nginx

```bash
sudo systemctl restart nginx
```

---

# 🌍 9. Open your website

In browser:

```
http://YOUR_PUBLIC_IP
```

---

# ⚠️ REAL-WORLD CHECKS (if something fails)

## ❌ Site not opening

* Security Group → **Port 80 (HTTP) must be allowed**

---

## ❌ Default page showing

```bash
sudo rm -rf /var/www/html/*
sudo git clone https://github.com/YOUR_USERNAME/YOUR_REPO.git /var/www/html
```

---

## ❌ Repo structure wrong

Your repo must look like:

```
index.html   ✅
style.css
```

NOT:

```
frontend/index.html ❌
```

Fix:

```bash
sudo mv /var/www/html/frontend/* /var/www/html/
```

---

## ❌ Updates not showing

```bash
cd /var/www/html
sudo git pull
sudo systemctl restart nginx
```

---

# 🧠 FINAL UNDERSTANDING

* EC2 = server
* Nginx = serves your files
* GitHub = your code source
* Public IP = your website URL

---

If you want next step:

* 🔒 HTTPS (SSL)
* 🌐 Custom domain
* ⚡ Auto deploy on push (CI/CD)

Say what you want — I’ll take you there.

