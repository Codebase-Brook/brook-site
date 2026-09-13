# Publishing buildwithbrook.com

Everything is built and committed here. Three steps need your GitHub
account and Namecheap login.

## 1. Create the public repo

github.com/new
  Owner: Codebase-Brook
  Name:  brook-site
  Public
  Do NOT add a README, .gitignore or licence (this folder already has a commit)

## 2. Push

    cd "~/Documents/AI Workspace/deploy/brook-site"
    git remote add origin https://github.com/Codebase-Brook/brook-site.git
    git push -u origin main

## 3. Turn on Pages

Repo → Settings → Pages
  Source: Deploy from a branch
  Branch: main / (root)
  Save

The CNAME file is already in the repo, so Pages will pick up
buildwithbrook.com by itself. Tick "Enforce HTTPS" once the
certificate is issued (can take ~15 min).

## 4. DNS at Namecheap — the apex has NO records right now

`dig +short A buildwithbrook.com` returns nothing, so the domain
resolves nowhere. Add these in Namecheap → Domain List →
buildwithbrook.com → Advanced DNS:

    A     @     185.199.108.153
    A     @     185.199.109.153
    A     @     185.199.110.153
    A     @     185.199.111.153
    CNAME www   codebase-brook.github.io.

Leave the existing `systems` CNAME alone — that's the kits site.

## 5. Verify (not before ~20 min; DNS can take longer)

    dig +short A buildwithbrook.com          # expect the four IPs
    curl -sI https://buildwithbrook.com | head -1   # expect 200

Do not trust a green dashboard — check `dig` output. This domain was
dead for nine weeks once behind an "Active" status.

## Changing the theme later

Themes live in the source repo (Brook OS), not here.

    cd "~/Documents/AI Workspace/Brook OS/deliverables/site"
    ./serve.sh                 # pick a palette in the theme lab
    ./bake-theme.sh indigo     # bake it in

Then re-copy index.html and products/ into this folder, strip the
theme-lab script tag, commit and push.
