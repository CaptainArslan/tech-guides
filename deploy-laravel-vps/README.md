# Deploy Laravel on a VPS

A copy-paste, step-by-step guide for deploying a Laravel application on a bare Ubuntu VPS — AWS EC2, a DigitalOcean Droplet, or any other Ubuntu 22.04/24.04 host. Covers hardening the server, installing Nginx, PHP-FPM, Composer, and MySQL, setting up phpMyAdmin, connecting the server to GitHub over SSH, configuring the Nginx vhost, enabling HTTPS, running queue workers under Supervisor, wiring up the Laravel scheduler with cron, and a zero-downtime redeploy script. Includes dark and light mode.

**[View the live guide →](https://CaptainArslan.github.io/deploy-laravel-vps/)**

## What's covered

1. Provisioning and hardening the server (non-root user, UFW firewall, timezone) — notes on AWS Security Groups vs DigitalOcean Cloud Firewalls
2. Installing Nginx
3. Installing PHP and PHP-FPM with every extension Laravel needs
4. Installing Composer
5. Installing and securing MySQL, creating a dedicated database and user
6. Installing phpMyAdmin behind Nginx, with security notes
7. Generating an SSH key and connecting the server to GitHub (account key or repo deploy key)
8. Cloning the project and configuring `.env`
9. Setting file/storage permissions and running migrations
10. Writing the Nginx server block for Laravel (`public/` root, PHP-FPM socket, `.env` protection)
11. Enabling HTTPS with Let's Encrypt / Certbot
12. Running queue workers under Supervisor
13. Scheduling `artisan schedule:run` with cron
14. A zero-downtime redeploy script (`git pull` → `composer install` → `migrate` → cache → restart workers)
15. Reference: full Nginx vhost, full Supervisor config, a service cheatsheet, an Artisan cheatsheet, `.gitignore` and `.env` safety, a complete copy-all setup script, a directory/request-flow map, and a troubleshooting table

## Using this guide

Every command block has a **Copy** button — no manual selecting and pasting. Wherever you see `laravel-app`, `your-username`, `your-domain.com`, or a password placeholder, swap in your own values.

Toggle **dark / light mode** with the button in the top-right corner; your preference is remembered on your next visit.

## Running it locally

This is a single self-contained HTML file — no build step, no dependencies.

```bash
git clone https://github.com/CaptainArslan/deploy-laravel-vps.git
cd deploy-laravel-vps
open index.html   # or just double-click it in Finder/Explorer
```

## Hosting

This repo is set up for [GitHub Pages](https://pages.github.com/), served from the `main` branch, root folder.

## License

Free to use, copy, and adapt for your own deployment docs.
