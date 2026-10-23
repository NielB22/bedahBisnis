# ADR 001 — Gambar 3D lewat kode, bukan manual

**Tanggal:** 2026-10-23  
**Status:** Accepted

## Konteks
Pipeline butuh visual isometrik exploded-view dari objek bisnis (contoh: kopi susu).
Menggambar manual di Blender atau Illustrator memakan waktu berjam-jam per video.

## Keputusan
Pakai **Three.js** untuk generate gambar 3D secara prosedural dari `parts.json`.
Kamera ortografik isometrik (35.26°), flat shading, EdgesGeometry untuk outline.

## Konsekuensi
✅ Satu file JSON cukup untuk mendefinisikan seluruh objek  
✅ Tidak butuh skill desain manual  
✅ Mudah dianimasikan (explode, rotate, highlight per-part)  
⚠️ Visual lebih simpel dibanding render Blender — tapi cocok untuk style "bedah"