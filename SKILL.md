---
name: deliver-via-download-link
description: "User delivery preference: when sending files (images, NFT art, banners, logos, scripts) to this user, ALWAYS share via an HTTP download link from the VPS (http://134.199.170.183:8000/ or :8002/) — NEVER send the media directly through Telegram/WhatsApp chat. Triggered by user saying 'ingat pake link download!' or any file-delivery request. This overrides the default Telegram MEDIA:/path behavior."
---

# Deliver Via Download Link (not chat media)

## ATURAN INTI
User eksplisit: **"ingat pake link download!"** → jangan kirim file lewat media TG langsung.
Selalu share **HTTP link** ke VPS user punya.

## FALLBACK CHAIN (urutan prioritas)
1. **VPS `python3 -m http.server` port 8000** — coba dulu local: `curl -s -o /dev/null -w "%{http_code}" http://127.0.0.1:8000/FILE`. Kalau 200 tapi eksternal timeout → UFW allow port, atau pindah ke Cloudflare Tunnel/Vercel.
   - **Hairpin NAT issue**: dari dalam VPS, akses pake internal IP (`hostname -I | awk '{print $1}'`), BUKAN public IP. Public IP dari dalem VPS sering timed out karena provider tidak support hairpin NAT.
2. **Cloudflare Tunnel** (`cloudflared tunnel --url http://localhost:8000`) — bypass hairpin NAT & firewall block. Install via deb: `curl -fsSL -o /tmp/cloudflared.deb "https://github.com/cloudflare/cloudflared/releases/download/$(curl -s https://api.github.com/repos/cloudflare/cloudflared/releases/latest | grep -o '"browser_download_url": *"[^"]*amd64.deb"' | head -1 | cut -d'"' -f4)" && sudo dpkg -i /tmp/cloudflared.deb`.
3. **Vercel static deploy** — buat halaman HTML sederhana yang embed file: `npx vercel deploy --confirm --prod --token "$VERCEL_TOKEN"`. Cocok buat file kecil (<50MB) yang butuh link langsung bisa dibuka dari HP.
4. **GitHub raw link** — push ke repo lalu share raw link (fallback terakhir).

## HOW-TO VPS SERVER (port 8000/8002)
- Server jalankan DARI home dir (bukan `/root/dl/` — Hermes agent jalannya sebagai user `ubuntu`, tidak punya akses write ke `/root`).  
  Create folder: `mkdir -p /home/ubuntu/dl`; copy file ke situ.  
  Jalankan: `python3 -m http.server 8000 --bind 0.0.0.0 -d /home/ubuntu/dl`
- Hairpin NAT: dari dalem VPS, akses pake internal IP (`hostname -I | awk '{print $1}'`), BUKAN public IP (`134.199.170.183`). External akses via Cloudflare Tunnel atau Vercel.
- UFW allow: `sudo ufw allow 8000/tcp` dan `sudo ufw allow 8002/tcp`.

## PITFALLS
- **`/root/dl/` tidak bisa ditulis agent** — user `ubuntu` tidak bisa `cp` ke `/root/dl/` tanpa sudo, tapi server juga run non-root. Solusi: pakai `/home/ubuntu/dl/` atau `/home/ubuntu/nft-work/tmp/`.
- **http.server bisa hang** → return 000. Kalau link gak bisa diakses, RESTART server.
- **Jangan kirim `MEDIA:`** buat file gede (user komplain gak bisa akses di Telegram desktop/mobile).
