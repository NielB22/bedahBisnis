# CLAUDE.md — bedahBisnis

Konteks untuk Claude Code. Dibaca otomatis setiap sesi.

## Project
DIY pipeline video pendek animasi isometrik "bedah bisnis" (1080×1920, 30fps), terinspirasi kitabedah.id.
Tujuan utama: belajar + portofolio. Monetisasi hanya bonus.
"Bedah" versi bisnis = produk dibelah jadi lapisan biaya; tinggi tiap lapisan = porsi rupiah dari harga jual.

## Cara kerja dengan saya (Niel)
- Mahasiswa CS UMN. Saya mau **paham** tiap langkah, jadi jelaskan singkat *kenapa* sebelum/sesudah mengubah sesuatu.
- Jawab dalam Bahasa Indonesia, terstruktur, step-by-step.
- Satu langkah kecil per giliran → tunjukkan hasilnya → lanjut. Jangan kerjakan banyak fase sekaligus.
- Tanya dulu sebelum perubahan besar, hapus file, atau ganti skema data.
- Hindari kerja manual yang makan waktu (misal gambar manual) — semua visual dibuat lewat kode.

## Environment
- Windows, PowerShell. Repo di `D:\bedahBisnis_PRJ\bedahBisnis`. Remote: github.com/NielB22/bedahBisnis (branch `main`).
- Node v24, npm 11, Git 2.45, VS Code + Live Server (port 5500).
- PowerShell ≠ CMD: jangan pakai `echo ... >` multi-baris; buat file lewat editor/tool.
- Belum ada: Python, ffmpeg, WSL2 (kemungkinan dibutuhkan di Fase 2 untuk WhisperX).

## Struktur
```
docs/decisions/   ADR (001-3d-lewat-kode.md)
docs/devlog/      catatan per hari/fase
docs/media/
research/<topik>/ parts.json (+ nanti sources.md, facts.json)
engine/scene.html Three.js r128 (cdnjs), baca ../research/kopi-susu/parts.json via fetch → harus lewat Live Server
pipeline/         (kosong) skrip Node untuk TTS, alignment, render
films/NNN-<topik>/ script.md, beats.json, metrics.md
site/             (kosong) showcase nanti
```
`.gitignore` mengabaikan `.env`, `*.mp4`, `*.wav`, `*.mp3`. Media besar → Google Drive.

## Skema parts.json (v0.2)
`meta`: topic, question, price, days, cupsPerDay, unitPerRp, rBottom, rTop.
`layers[]` urut dari bawah ke atas: id, label, group (`bahan`|`operasional`|`potongan`|`untung`),
salah satu dari `perCup` / `monthly` (dibagi gelas sebulan) / `pctOfPrice` (komisi, hanya `channel: "aplikasi"`) / `computed`,
basis, tier (`resmi`|`media`|`dimodelkan`|`dihitung`), color (token CSS: aren, milk, ice, espresso, cup, ops1, ops2, red, green).
Semua angka kopi susu saat ini **dimodelkan**, bukan data kedai nyata.

## Konvensi
- Commit: `feat(engine): ...`, `fix(tts): ...`, `docs(devlog): ...`, `research(kopi-susu): ...`
- 1 milestone GitHub per fase, 1 issue per langkah; tutup issue saat selesai.
- Setiap keputusan teknis penting → ADR baru di `docs/decisions/`.
- Tiap akhir sesi/fase → devlog.

## Status
- Fase 0 Fondasi ✅
- Fase 1 Mesin isometrik ✅ (scene.html = port prototipe "Kopi Susu Bedah": satu gelas, label leader, cincin harga, kontrol channel/slider/sorot)
- **Fase 2 Naskah & Suara ← mulai di sini**

## Rencana Fase 2
1. Gemini API key (free tier, aistudio.google.com) → `.env` (`GEMINI_API_KEY`). Langganan Google AI Pro TIDAK termasuk kredit API.
2. Naskah kopi susu: 10 beat, 95–130 kata, dua track `display` (teks layar) vs `spoken` (dibaca TTS) → `films/001-kopi-susu/script.md`
3. `pipeline/tts.js`: Gemini TTS (model Flash TTS), 3 take → `voice.wav`
4. Alignment kata (WhisperX, kemungkinan via WSL2) → `words.json`; potong jeda
5. Fase 3: `beats.json` (event animasi terikat ke kata) → render (Remotion atau Playwright + ffmpeg), audio loudnorm −14 LUFS
6. Fase 4: QA (juri Gemini + cek skrip), Fase 5: `run.py`/`run.js` end-to-end, Fase 6 (opsional): VPS + cron + Telegram approval
