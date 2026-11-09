# Quick Share
Upload → QR code → scan → download. No accounts. React + Vite + Tailwind frontend, Express + Multer backend.

## Run locally
    cp .env.example .env
    npm run install:all
    npm run dev          # API :3001, app :5173
To test with a phone, set `VITE_PUBLIC_URL=http://<your-LAN-IP>:5173` in `.env`, restart, and open that address on the sender device too.

## Deploy (single server)
    npm run install:all && npm run build
    NODE_ENV=production npm start    # serves API + built client on $PORT
Put it behind HTTPS (Caddy/nginx, or Render/Railway/Fly) and use a persistent volume for `STORAGE_DIR`.
Set `MAX_FILE_MB` / `MAX_FILES` in `.env`. For public traffic add `express-rate-limit` and a reverse-proxy body-size limit.

## API
POST /api/shares (multipart: type, expiresIn, files | payload) · GET /api/shares/:id · GET /api/shares/:id/files/:fid · GET /api/shares/:id/zip · DELETE /api/shares/:id (x-delete-token header)

## Notes
Files are stored under random UUID names outside the web root; metadata lives in `storage/shares.json`. Expired shares are swept every minute. Executable extensions are blocked.
