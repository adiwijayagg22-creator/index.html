# index.html
Website buatan sendiri 
<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Safe Media Downloader</title>
  <style>
    body{font-family:system-ui,-apple-system,Segoe UI,Roboto,Ubuntu; background:#0f1220; color:#fff; min-height:100vh; margin:0; display:flex; align-items:center; justify-content:center}
    .card{background:#1e223a; padding:24px; border-radius:16px; width:min(520px,92vw); box-shadow:0 20px 50px rgba(0,0,0,.5)}
    h1{margin:0 0 8px}
    p{opacity:.85}
    label{display:block; margin:16px 0 6px}
    input,select,button{width:100%; padding:12px; border-radius:10px; border:none}
    input,select{background:#0f1220; color:#fff}
    button{margin-top:16px; background:linear-gradient(90deg,#6c5ce7,#00cec9); color:#fff; font-weight:600; cursor:pointer}
    button:disabled{opacity:.6; cursor:not-allowed}
    .note{font-size:.9rem; margin-top:12px; opacity:.75}
    .log{margin-top:12px; font-size:.9rem; opacity:.9}
  </style>
</head>
<body>
  <div class="card">
    <h1>Safe Media Downloader</h1>
    <p>Template legal untuk mengunduh <b>konten milik sendiri / public-domain / URL langsung</b>.</p>

    <label>URL File (langsung ke .mp3 / .mp4 / .webm / .wav)</label>
    <input id="url" placeholder="https://example.com/media.mp4" />

    <label>Nama File</label>
    <input id="filename" placeholder="media" />

    <label>Format</label>
    <select id="format">
      <option value="mp4">MP4 (video)</option>
      <option value="mp3">MP3 (audio)</option>
      <option value="webm">WEBM</option>
      <option value="wav">WAV</option>
    </select>

    <button id="btn">Unduh</button>
    <div class="log" id="log"></div>
    <div class="note">Catatan: Template ini <b>tidak</b> mengekstrak dari aplikasi ber-DRM / platform tertutup. Gunakan hanya untuk konten yang Anda punya haknya.</div>
  </div>

  <script>
    const btn = document.getElementById('btn');
    const log = document.getElementById('log');

    btn.onclick = async () => {
      const url = document.getElementById('url').value.trim();
      const name = document.getElementById('filename').value.trim() || 'media';
      const format = document.getElementById('format').value;
      if(!url){ log.textContent = 'URL kosong.'; return; }
      btn.disabled = true; log.textContent = 'Mengunduh…';
      try{
        const res = await fetch(url, { mode:'cors' });
        if(!res.ok) throw new Error('Gagal mengambil file');
        const blob = await res.blob();
        const a = document.createElement('a');
        a.href = URL.createObjectURL(blob);
        a.download = `${name}.${format}`;
        document.body.appendChild(a); a.click(); a.remove();
        URL.revokeObjectURL(a.href);
        log.textContent = 'Selesai.';
      }catch(e){
        log.textContent = 'Error: ' + e.message;
      }finally{ btn.disabled = false; }
    };
  </script>
</body>
</html>
