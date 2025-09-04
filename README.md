<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Peminjaman Buku Perpustakaan</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background-color: #f0f0f0;
      padding: 20px;
    }
    h1 {
      text-align: center;
    }
    form, table, .tampilan-json {
      margin: auto;
      max-width: 600px;
      background: #fff;
      padding: 20px;
      border-radius: 8px;
      box-shadow: 0 0 10px rgba(0,0,0,0.1);
    }
    label, input, button {
      display: block;
      width: 100%;
      margin-bottom: 10px;
    }
    table {
      margin-top: 30px;
      border-collapse: collapse;
      width: 100%;
    }
    th, td {
      border: 1px solid #ccc;
      padding: 8px;
      text-align: left;
    }
    th {
      background-color: #eaeaea;
    }
    .tampilan-json {
      white-space: pre-wrap;
      word-wrap: break-word;
      margin-top: 20px;
    }
  </style>
</head>
<body>
  <h1>Formulir Peminjaman Buku</h1>
  <form id="formPeminjaman">
    <label for="nama">Nama Peminjam:</label>
    <input type="text" id="nama" name="nama" required>

    <label for="judul">Judul Buku:</label>
    <input type="text" id="judul" name="judul" required>

    <label for="tglKembali">Tanggal Pengembalian:</label>
    <input type="date" id="tglKembali" name="tglKembali" required>

    <label for="jaminan">Uang Jaminan (Rp):</label>
    <input type="number" id="jaminan" name="jaminan" required min="0">

    <button type="submit">Catat Peminjaman</button>
  </form>

  <table id="tabelPeminjaman">
    <thead>
      <tr>
        <th>Nama Peminjam</th>
        <th>Judul Buku</th>
        <th>Tanggal Pengembalian</th>
        <th>Uang Jaminan (Rp)</th>
      </tr>
    </thead>
    <tbody></tbody>
  </table>

  <button onclick="tampilkanDataJson()" style="display: block; margin: 20px auto;">Lihat Data Peminjaman (JSON)</button>
  <div class="tampilan-json" id="tampilanJson"></div>

  <script>
    const form = document.getElementById('formPeminjaman');
    const tabelBody = document.querySelector('#tabelPeminjaman tbody');
    const tampilanJson = document.getElementById('tampilanJson');

    function simpanKeLocalStorage(data) {
      const semuaData = JSON.parse(localStorage.getItem('peminjaman')) || [];
      semuaData.push(data);
      localStorage.setItem('peminjaman', JSON.stringify(semuaData));
    }

    function muatDariLocalStorage() {
      const semuaData = JSON.parse(localStorage.getItem('peminjaman')) || [];
      semuaData.forEach(data => tambahBarisTabel(data));
    }

    function tambahBarisTabel(data) {
      const row = document.createElement('tr');
      row.innerHTML = `
        <td>${data.nama}</td>
        <td>${data.judul}</td>
        <td>${data.tglKembali}</td>
        <td>Rp ${parseInt(data.jaminan).toLocaleString('id-ID')}</td>
      `;
      tabelBody.appendChild(row);
    }

    form.addEventListener('submit', function(event) {
      event.preventDefault();

      const data = {
        nama: document.getElementById('nama').value,
        judul: document.getElementById('judul').value,
        tglKembali: document.getElementById('tglKembali').value,
        jaminan: document.getElementById('jaminan').value
      };

      tambahBarisTabel(data);
      simpanKeLocalStorage(data);
      form.reset();
    });

    function tampilkanDataJson() {
      const data = JSON.parse(localStorage.getItem('peminjaman')) || [];
      tampilanJson.textContent = JSON.stringify(data, null, 2);
    }

    window.addEventListener('load', muatDariLocalStorage);
  </script>
</body>
</html>
