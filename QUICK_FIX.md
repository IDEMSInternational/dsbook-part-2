# Quick Fix for PreTeXt Not Rendering

## The Problem
You're seeing the repository README on GitHub Pages instead of the PreTeXt book.

## The Solution (2 minutes)
Go to your repository's Settings and change one dropdown:

### Steps:
1. Click **Settings** (top menu bar)
2. Click **Pages** (left sidebar, under "Code and automation")
3. Find **"Build and deployment"** section
4. Under **"Source"**, change the dropdown from:
   - ~~Deploy from a branch (main / docs)~~ ❌
   - To: **GitHub Actions** ✅
5. Click **Save** (if there's a save button)

### Visual Guide:
```
Settings → Pages → Source: [Change to "GitHub Actions"]
```

## Why This Works
- Your PreTeXt workflow (in `.github/workflows/pretext.yml`) is already building and deploying successfully
- The workflow uses GitHub Actions to deploy the book
- But GitHub Pages was configured to serve from the `/docs` folder instead
- The `/docs` folder contains an old Quarto book, not your PreTeXt book
- Changing the source to "GitHub Actions" tells GitHub Pages to serve the PreTeXt deployment

## Verification
After changing the setting:
1. Wait 2-5 minutes
2. Visit: https://idemsinternational.github.io/dsbook-part-2/
3. You should see your PreTeXt book (not the README)
4. If you still see the README, clear your browser cache and try again

## Need More Help?
See `GITHUB_PAGES_SETUP.md` for detailed information and troubleshooting.
