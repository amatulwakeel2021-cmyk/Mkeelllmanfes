<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Kirim Pesan Rahasia</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gradient-to-br from-pink-500 to-purple-600 min-h-screen flex items-center justify-center p-4">

  <div class="bg-white rounded-2xl shadow-xl w-full max-w-md p-6">
    <h1 class="text-2xl font-bold text-center text-gray-800 mb-2">Kirim Pesan Anonim</h1>
    <p class="text-xs text-center text-gray-500 mb-6">Pesan Anda hanya bisa dilihat oleh Admin.</p>

    <form id="nglForm" class="space-y-4">
      <!-- Pesan Teks -->
      <div>
        <label class="block text-sm font-medium text-gray-700 mb-1">Pesan Teks</label>
        <textarea id="message" rows="3" class="w-full p-3 border border-gray-300 rounded-xl focus:ring-2 focus:ring-pink-500 outline-none" placeholder="Tulis pesan rahasia..."></textarea>
      </div>

      <!-- Unggah Foto -->
      <div>
        <label class="block text-sm font-medium text-gray-700 mb-1">Lampirkan Foto</label>
        <input type="file" id="photo" accept="image/*" class="w-full text-sm text-gray-500 file:mr-4 file:py-2 file:px-4 file:rounded-full file:border-0 file:text-sm file:font-semibold file:bg-pink-50 file:text-pink-700 hover:file:bg-pink-100">
      </div>

      <!-- Rekaman Suara -->
      <div>
        <label class="block text-sm font-medium text-gray-700 mb-1">Pesan Suara</label>
        <div class="flex items-center space-x-2">
          <button type="button" id="recordBtn" class="bg-gray-100 hover:bg-gray-200 text-gray-700 px-4 py-2 rounded-xl text-sm font-medium transition">🎤 Mulai Rekam</button>
          <span id="recordStatus" class="text-xs text-gray-500">Belum ada rekaman</span>
        </div>
      </div>

      <!-- Tombol Kirim -->
      <button type="submit" class="w-full bg-pink-600 hover:bg-pink-700 text-white font-bold py-3 rounded-xl shadow-lg transition">Kirim Pesan</button>
    </form>
  </div>

  <script>
    let mediaRecorder;
    let audioChunks = [];
    let audioBlob = null;

    const recordBtn = document.getElementById('recordBtn');
    const recordStatus = document.getElementById('recordStatus');

    // Fitur Rekam Suara
    recordBtn.addEventListener('click', async () => {
      if (!mediaRecorder || mediaRecorder.state === "inactive") {
        const stream = await navigator.mediaDevices.getUserMedia({ audio: true });
        mediaRecorder = new MediaRecorder(stream);
        audioChunks = [];

        mediaRecorder.ondataavailable = e => audioChunks.push(e.data);
        mediaRecorder.onstop = () => {
          audioBlob = new Blob(audioChunks, { type: 'audio/mp3' });
          recordStatus.textContent = "✓ Rekaman tersimpan";
          recordBtn.textContent = "🔄 Rekam Ulang";
        };

        mediaRecorder.start();
        recordBtn.textContent = "⏹️ Stop Rekam";
        recordStatus.textContent = "Merekam...";
      } else if (mediaRecorder.state === "recording") {
        mediaRecorder.stop();
      }
    });

    // Form Submit (Hubungkan ke Backend/API di sini)
    document.getElementById('nglForm').addEventListener('submit', (e) => {
      e.preventDefault();
      alert('Pesan berhasil dikirim secara anonim!');
      location.reload();
    });
  </script>
</body>
</html>
