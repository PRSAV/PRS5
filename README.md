# PRS.AssetVerify 5.0 — Independent frontend release

This project is derived from PRS 4.6 and is designed to deploy separately to `PRSAV/PRS5`.
**Never upload these files to `PRSAV/PRS4` or attach the original Cloudflare D1/R2 resources.**

## Changes
1. After a barcode scan, the verification form keeps the original evidence photograph and allows additional camera/gallery photos up to 12 total.
2. On scanned asset rows, **Item Name Source** offers **Code of asset encoded in barcode** (default) or **Fill name of asset manually**. Manual names are stored through the existing `assetName` field.

## Deployment prerequisite — IMPORTANT
This ZIP includes the frontend only. The source ZIP did not contain `worker.js`, the D1 schema, or any backend migrations. PRS 5 will not authenticate or save records until a separate PRS 5 Worker and its D1/R2 resources are provisioned. **Do not bind PRS 5 to PRS 4.6 databases or buckets.**

1. Create new GitHub repository `PRSAV/PRS5`; upload the website files from this folder at repo root; enable GitHub Pages on main/root.
2. In Cloudflare create **NEW** D1 `pv-capture-db-v5`, **NEW** R2 `pv-capture-photos-v5`, and **NEW** Worker `pv-capture-ai-v5`.
3. Deploy an independently obtained copy of the compatible V4.6 backend `worker.js` to the new Worker, configuring **only** the NEW v5 D1 binding `DB` and R2 binding `Photos`. Provision the schema into the NEW D1 according to the backend source. Do not run migrations against v4 databases.
4. Configure new Worker secrets and CORS rules to permit `https://prsav.github.io/PRS5/`. Do not put secrets into GitHub.
5. Confirm the new Worker address and change `WORKER_URL` in `app.js` if Cloudflare assigned a different workers.dev subdomain. The URL in source is a proposed address, not verified live.
6. Test scan -> additional camera photo -> additional gallery photo -> item-name source dropdown -> save, using test data in the NEW database. Test existing PRS 4.6 still works before inviting users to PRS 5.

### Isolation
- PRS5 repository and GitHub Pages scope separate from PRS4.
- New Worker, D1 database, R2 bucket, session/localStorage keys, IndexedDB database, and service-worker cache.
- New service worker removes only caches belonging to PRS 5, not those of PRS 4.

### Current limitations
Backend code and schema are NOT included in original upload. UI/static and JavaScript syntax tests are possible here; end-to-end auth, uploading, and persistence require the new backend to be deployed. The `photos` payload uses the existing frontend API contract; the new backend must support multiple photos as the old one did.
