# Premaira Cloud — Deployment Guide

> **Live URL:** https://www.premaira.com  
> **GitHub Repo:** https://github.com/cthakur9824/premaira-cloud  
> **Date Deployed:** May 23, 2026

---

## Table of Contents

1. [Project Overview](#1-project-overview)
2. [Git Setup (Local)](#2-git-setup-local)
3. [Push to GitHub](#3-push-to-github)
4. [Enable GitHub Pages](#4-enable-github-pages)
5. [GoDaddy DNS Configuration](#5-godaddy-dns-configuration)
6. [Enable HTTPS](#6-enable-https)
7. [Troubleshooting](#7-troubleshooting)
8. [Updating the Website](#8-updating-the-website)

---

## 1. Project Overview

This is a single-page consulting website built with HTML, CSS, and JavaScript. It uses zero external frameworks — just Font Awesome for icons and Google Fonts for typography.

**File structure:**
```
consulting-site/
├── index.html          # Main website (all code in one file)
└── README.md           # This documentation
```

**Services listed:**
- DevOps & Platform Engineering
- Cloud Architecture
- Blockchain & Web3
- EDI Healthcare Solutions
- Retail Solution Provider

---

## 2. Git Setup (Local)

Git tracks changes to your files and lets you push them to GitHub.

### Step 2.1 — Initialize the repository

Open **PowerShell** or **Git Bash** in the project folder and run:

```bash
cd "C:\ClaudeCode\consulting-site"
git init
```

**Expected output:**
```
Initialized empty Git repository in C:/ClaudeCode/consulting-site/.git/
```

A hidden `.git` folder is created — this is where Git stores version history.

### Step 2.2 — Set your identity

```bash
git config user.email "chetanthakurelite@gmail.com"
git config user.name "Chetan Thakure"
```

> **Note:** No `--global` flag means this identity is only used for this repository.

### Step 2.3 — Stage and commit

```bash
git add index.html
git commit -m "Initial commit — Premaira Cloud consulting site"
```

**Commands explained:**
- `git add index.html` — stages the file (tells Git "I want to include this in the next snapshot")
- `git commit -m "message"` — takes the snapshot with a descriptive message

**Expected output:**
```
[master (root-commit) abc1234] Initial commit — Premaira Cloud consulting site
 1 file changed, 982 insertions(+)
 create mode 100644 index.html
```

---

## 3. Push to GitHub

### Step 3.1 — Create a repository on GitHub

1. Go to https://github.com/new
2. Enter **Repository name:** `premaira-cloud`
3. Set visibility to **Public** (required for free GitHub Pages)
4. Do **NOT** check "Initialize with README" or any other boxes
5. Click **Create repository**

**What you'll see on the next page:**

After creation, GitHub shows a page with setup commands. Look for the section **"…or push an existing repository from the command line"**.

---

### Step 3.2 — Link local repo to GitHub and push

In your terminal, run:

```bash
git remote add origin https://github.com/cthakur9824/premaira-cloud.git
git push -u origin master
```

**Commands explained:**
- `git remote add origin <URL>` — tells your local Git where the remote repo lives on GitHub
- `git push -u origin master` — uploads your local commits to GitHub (`-u` sets upstream tracking so future pushes are just `git push`)

**Expected output:**
```
To https://github.com/cthakur9824/premaira-cloud.git
 * [new branch]      master -> master
branch 'master' set up to track 'origin/master'.
```

---

### Step 3.3 — Verify

1. Go to https://github.com/cthakur9824/premaira-cloud
2. You should see your `index.html` file listed

---

## 4. Enable GitHub Pages

GitHub Pages hosts static websites directly from your repository.

### Step 4.1 — Open Pages settings

1. Go to https://github.com/cthakur9824/premaira-cloud/settings/pages
2. Or: On your repo page → click **Settings** tab → click **Pages** in the left sidebar

### Step 4.2 — Configure source

Under **Branch**:
- Select `master` from the dropdown
- Select `/ (root)` from the folder dropdown
- Click **Save**

---

### Step 4.3 — Add custom domain

After a minute, your site will be live at:

```
https://cthakur9824.github.io/premaira-cloud/
```

Now set your custom domain:
1. In the **Custom domain** field, type `www.premaira.com`
2. Click **Save**
3. GitHub will run a **DNS check** — this will fail initially (that's expected, DNS hasn't been configured yet)
4. **Do NOT check** "Enforce HTTPS" yet — wait until DNS is configured

---

## 5. GoDaddy DNS Configuration

This step connects your domain name to GitHub's servers.

### Step 5.1 — Log into GoDaddy

1. Go to https://godaddy.com and sign in
2. Click **My Products**
3. Find your domain (`premaira.com`) and click **DNS** (or **Manage DNS**)

### Step 5.2 — Delete existing records

Before adding new records, remove anything that conflicts:

- Check for any **A records** with name `@` — these are from GoDaddy's website builder. Delete them.
- Check for any **CNAME records** — if one exists for `www` or for the bare domain, delete it.
- Go to the **Forwarding** section (separate tab/section) and delete any forwarding rules for `www`.

---

### Step 5.3 — Add A records (for bare domain)

The bare domain (`premaira.com` without `www`) needs to point directly to GitHub's IP addresses.

| Type | Name | Value | TTL |
|------|------|-------|-----|
| A | @ | 185.199.108.153 | 1 hour |
| A | @ | 185.199.109.153 | 1 hour |
| A | @ | 185.199.110.153 | 1 hour |
| A | @ | 185.199.111.153 | 1 hour |

**How to add each record:**
1. Click **Add Record**
2. Set **Type** to `A`
3. For **Name**, type `@` (GoDaddy interprets this as the root domain)
4. Paste the **Value** (IP address)
5. Set **TTL** to `1 hour`
6. Click **Save**

Repeat 4 times — once for each IP.

> **Note on TTL (Time To Live):** This tells other DNS servers how long to cache this record before checking for updates. 1 hour is standard. Lower = faster updates, higher = less DNS traffic.

---

### Step 5.4 — Add CNAME record (for www)

The `www` subdomain needs to point to your GitHub Pages URL.

| Type | Name | Value | TTL |
|------|------|-------|-----|
| CNAME | www | cthakur9824.github.io | 1 hour |

**How to add:**
1. Click **Add Record**
2. Set **Type** to `CNAME`
3. For **Name**, type `www` (GoDaddy auto-appends your domain, making it `www.premaira.com`)
4. For **Value**, type `cthakur9824.github.io` (no trailing slash)
5. Set **TTL** to `1 hour`
6. Click **Save**

> **If GoDaddy says "conflicting":** Make sure you deleted any existing `www` records and forwarding rules first. If it still fails, use 4 A records instead (same IPs as step 5.3, but with Name set to `www`).

---

### Step 5.5 — Verify DNS propagation

DNS changes take time to spread (5 minutes to 48 hours). To check if it's working:

1. Go to https://whatsmydns.net
2. Search for `premaira.com` — check `A` record type
3. You should see all 4 GitHub IPs showing up (green checkmarks)
4. Also search for `www.premaira.com` — check `CNAME` record type
5. It should show `cthakur9824.github.io`

---

## 6. Enable HTTPS

Once DNS is fully propagated:

### Step 6.1 — Check DNS in GitHub Pages

1. Go to https://github.com/cthakur9824/premaira-cloud/settings/pages
2. Under **Custom domain**, the DNS check should now show a green **"DNS check successful"**

### Step 6.2 — Enforce HTTPS

1. Check the **Enforce HTTPS** checkbox
2. GitHub will issue a free SSL certificate via Let's Encrypt
3. This can take **15–30 minutes** to complete
4. Once done, HTTPS will be active

> **If "Enforce HTTPS" is greyed out:** Wait longer for DNS propagation. If it's been hours, try removing the custom domain, saving, waiting 2 minutes, and re-adding it. This re-triggers certificate issuance.

---

## 7. Troubleshooting

| Problem | Likely Cause | Fix |
|---------|-------------|-----|
| DNS check fails on GitHub | Records not propagated | Wait 15–60 min or check whatsmydns.net |
| "Conflicting record" on GoDaddy | Old record blocking the new one | Delete all existing A/CNAME records first |
| HTTPS unavailable | Certificate not issued yet | Wait 30 min, or remove/re-add custom domain in GitHub |
| Site shows GoDaddy parking page | DNS still pointing to GoDaddy | Verify your A/CNAME records are correct |
| Blank page | File path or content issue | Check that index.html exists and is valid |
| CSS broken on mobile | Viewport issue | Verify `<meta name="viewport">` tag exists |

---

## 8. Updating the Website

To make changes and publish them:

### Step 8.1 — Edit the file locally

Make changes to `index.html` in your code editor.

### Step 8.2 — Commit and push

```bash
cd "C:\ClaudeCode\consulting-site"
git add index.html
git commit -m "Describe what you changed"
git push
```

### Step 8.3 — Wait for GitHub Pages

GitHub Pages automatically redeploys within 1–2 minutes after a push. Your site updates without any extra steps.

---

## Key Concepts Summary

| Term | Meaning |
|------|---------|
| **Git** | Version control — tracks changes to your files |
| **GitHub** | Cloud hosting for Git repositories |
| **GitHub Pages** | Free static website hosting from a GitHub repo |
| **DNS** | Phonebook of the internet — maps domain names to IP addresses |
| **A Record** | Points a domain directly to a server IP |
| **CNAME Record** | Points a domain to another domain name |
| **SSL Certificate** | Enables HTTPS (the padlock in the browser) |
| **Let's Encrypt** | Free automated SSL certificate provider |
| **DNS Propagation** | Time it takes for DNS changes to spread worldwide |

---

*Documentation generated for Premaira Cloud — Last updated: May 23, 2026*