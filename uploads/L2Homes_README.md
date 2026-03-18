# L2 Homes Website

------------------------------------------------------------------------

# ⚠️ Important Rules

### ❌ Do NOT manually push to `main` or `dev`

These branches are automatically managed.

If you push there manually:

-   Your changes may be overwritten.
-   The website may not update correctly.
-   Deployment behavior may become inconsistent.

If you want to update the website:

-   Push to `master` (production)
-   Push to `development` (dev)

------------------------------------------------------------------------

### ❌ Do NOT manually push to the assets repository

Do not edit:

https://github.com/l2homes-au/l2homes-assets

Changes there will not be adopted by the website unless processed
automatically by the deployment workflow.

------------------------------------------------------------------------

### ✏️ To Update Website Content

Use the CMS:

-   Production CMS: https://www.l2homes.com.au/admin/
-   Development CMS: https://l2homes.vercel.app/admin/

No manual Git operations are required.

------------------------------------------------------------------------

## 🏠 Project Overview

This repository contains the source code for the **L2 Homes** marketing
website, a modern, content-driven website built with **Next.js** and
deployed on **Vercel**.

The site is designed for performance, SEO, and easy content management,
with assets hosted in a separate repository and delivered via CDN.

------------------------------------------------------------------------

## 🧱 Tech Stack

-   Framework: Next.js (App Router)
-   Language: TypeScript
-   Styling: Tailwind CSS
-   Deployment: Vercel
-   CMS: Sveltia CMS (https://sveltiacms.app/en/)
-   Assets CDN: jsDelivr (https://www.jsdelivr.com/)
-   Source Control: GitHub

------------------------------------------------------------------------

## 📁 Repository Structure

l2homes/ ├── app/ \# Next.js App Router │ ├── content/ \# JSON content
files (CMS-managed) │ ├── components/ \# Reusable UI components │ ├──
config/ \# Site-level configuration │ └── globals.css ├── public/ │ └──
uploads/ \# Local uploads (synced to assets repo) ├── .github/ \# GitHub
Actions workflows ├── tailwind.config.ts ├── next.config.js └──
README.md

------------------------------------------------------------------------

# 🖼️ Asset Management

Static assets (images, videos) are hosted in a separate public
repository:

https://github.com/l2homes-au/l2homes-assets

Assets are delivered via jsDelivr CDN:

https://cdn.jsdelivr.net/gh/l2homes-au/l2homes-assets@`<commit-sha>`{=html}

------------------------------------------------------------------------

## Why a Separate Public Assets Repository Exists

### 1️⃣ CMS Requirement

We use Sveltia CMS, which requires content configuration and media
uploads to live in the same repository.

Uploaded files are committed into this repository under:

public/uploads/

------------------------------------------------------------------------

### 2️⃣ GitHub CDN Requirement

jsDelivr can only serve files from public repositories.

We want to keep the website source code private, so assets are synced to
a public repository instead.

------------------------------------------------------------------------

### Final Architecture

Private Source Repo (code + CMS) ↓ GitHub Action sync ↓ Public Assets
Repo ↓ GitHub CDN (jsDelivr) ↓ Website

------------------------------------------------------------------------

# 🚀 Deployment Architecture

The project uses GitHub Actions to implement a controlled deployment
pipeline that:

-   Keeps source code private
-   Serves assets via public GitHub CDN
-   Prevents stale asset issues
-   Ensures each pushed change deploys exactly once
-   Works around Vercel Hobby plan limitations

------------------------------------------------------------------------

## 🌿 Branch Strategy

  Environment   Working Branch   Deploy Branch   Assets Repo Branch
  ------------- ---------------- --------------- --------------------
  Production    master           main            main
  Development   development      dev             dev

------------------------------------------------------------------------

## Why Separate Working and Deploy Branches?

If Vercel were connected directly to working branches:

-   Asset sync commits could trigger extra deployments
-   Internal reference updates could trigger additional builds
-   A single logical change could deploy multiple times

By separating branches:

1.  Contributors push to working branches.
2.  Workflow prepares assets and updates references.
3.  Deploy branch is force-synced once.
4.  A single owner-triggered push hits Vercel.

This guarantees:

One pushed change → One deployment

------------------------------------------------------------------------

## Asset Sync & Commit-SHA Referencing

When files under public/uploads/ change:

1.  GitHub Actions syncs them to the public assets repo.

2.  The resulting assets commit SHA is captured.

3.  The SHA is written into:

    -   app/content/assets_ref.json (production)
    -   app/content/assets_ref.dev.json (development)

The website loads assets using that exact commit reference.

This prevents stale CDN issues when images are renamed or replaced.

------------------------------------------------------------------------

## Vercel Hobby Plan Workaround

On the Vercel Hobby plan:

-   Only pushes made by the repository owner trigger deployments.
-   Contributor pushes do not automatically deploy.

To ensure reliable deployments:

1.  The workflow force-syncs the deploy branch.
2.  It creates a small timestamp file (.vercel-deploy-trigger.txt).
3.  Commits it using the repository owner's identity.
4.  Force-pushes the deploy branch.

From Vercel's perspective:

The project owner pushed a new commit.

Deployment is triggered consistently.

------------------------------------------------------------------------

# 🌐 Environments

Production Website: https://www.l2homes.com.au/ Development Website:
https://l2homes.vercel.app/

------------------------------------------------------------------------

# 🚀 Local Development

Prerequisites:

-   Node.js 20.9+
-   npm

Setup:

npm install npm run dev

Visit:

http://localhost:3000

------------------------------------------------------------------------

# 👤 Maintainer

Xinyu Kang\
Canberra, Australia
