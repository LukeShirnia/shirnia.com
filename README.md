# shirnia.com

Luke Shirnia's personal website. The publishable site is the `app/` directory,
containing HTML, CSS, JavaScript and images. No application server, database or
frontend compilation is required.

## Cloudflare hosting

The site uses Cloudflare Workers Static Assets with the Worker name `shirnia-com`.
Only `app/` is uploaded; repository files and deployment configuration are not
served. Unmatched URLs return 404, rather than the home page.

Cloudflare Builds settings:

- Repository: `LukeShirnia/shirnia.com`
- Production branch: `main`
- Root directory: `/`
- Node.js: 22 (also recorded in `.nvmrc`)
- Build command: `npm run check:deploy`
- Deploy command: `npm run deploy`
- Non-production branch builds: disabled

Cloudflare installs the pinned dependencies from `package-lock.json` and deploys
pushes to `main`. The custom domain is managed in the dashboard, allowing the
deployment token to omit DNS permissions. The `workers.dev` address and version
preview URLs are disabled to keep the custom domain as the entry point.

## Local preview and validation

```sh
npm ci
npm run check:deploy
npm run dev
```

The preview normally runs at `http://localhost:8787`. Check the page, stylesheet,
images and external icons/fonts before publishing. The site currently loads
Google Fonts, Font Awesome and Iconify from external providers.

## Previous Kubernetes deployment

The Dockerfile, Docker Compose configuration and existing GitHub image workflows
are retained for rollback. The old automatic image workflow targets `master`,
not `main`; Cloudflare Builds handles normal releases from `main`.

The root `CNAME` file belongs to an older GitHub Pages setup and is not used by
Cloudflare or included in the uploaded assets.

Do not remove the Kubernetes workload until the Cloudflare deployment has been
verified and the old host is no longer needed for rollback.
