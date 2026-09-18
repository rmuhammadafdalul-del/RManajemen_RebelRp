# RManajemen_RebelRp — Discord Multi-Ticket Bot

Bot Discord siap deploy dengan:

- 🎫 Multi-ticket (user boleh membuat banyak tiket sekaligus)
- 📢 Report
- 📝 Formulir
- 💰 Donasi + bukti pembayaran
- 👮 Admin/Helper Confirm / Reject
- ⏰ Donasi auto-delete setelah 24 jam jika belum Confirm
- 📁 Kategori tiket terpisah
- 👋 Welcome
- 🚪 Goodbye
- 🎭 Self-role / pengambilan role
- 💾 SQLite persistent

## Struktur Discord

```text
💡 SUPPORT
└── #create-ticket

💰 TIKET DONASI
├── #ticket-0001
└── #ticket-0004

📢 TIKET REPORT
├── #ticket-0002
└── #ticket-0005

📝 TIKET FORMULIR
├── #ticket-0003
└── #ticket-0006

👥・welcome
👋・goodbye
🎭・ambil-role
ticket-logs
```

Kategori dan channel akan dibuat otomatis untuk panel/tiket bila belum ada. Channel welcome/goodbye/role panel dibuat/ditentukan lewat `.env`; untuk role panel, channel harus tersedia agar bot tahu tempat mengirim panel.

## Multi-ticket

Tidak ada pembatasan satu tiket per user. Setiap klik Report/Formulir/Donasi membuat channel baru dengan nomor berurutan `ticket-0001`, `ticket-0002`, dst. Nomor disimpan di SQLite sehingga tetap lanjut walaupun channel lama dihapus.

## Donasi

1. User klik **Donasi**.
2. Isi nominal dan metode.
3. Bot membuat tiket privat di `💰 TIKET DONASI`.
4. User mengirim bukti pembayaran sebagai attachment.
5. Admin/Helper memilih **Confirm Donasi** atau **Reject Donasi**.
6. Jika belum Confirm sampai 24 jam sejak tiket dibuat, bot otomatis menghapus channel.

Ubah `DONATION_TIMEOUT_HOURS` jika ingin durasi lain.

## Instalasi biasa

Persyaratan: Node.js 20 LTS atau lebih baru.

```bash
npm install
```

Copy `.env.example` menjadi `.env`, lalu isi:

```env
DISCORD_TOKEN=...
CLIENT_ID=...
GUILD_ID=...
```

Jalankan:

```bash
npm start
```

Setelah bot online, gunakan:

```text
/panel
/rolepanel
```

`/panel` mem-post panel ticket ke `#create-ticket` di kategori SUPPORT.
`/rolepanel` mem-post panel self-role ke channel `ROLE_PANEL_CHANNEL_NAME`.

## Welcome / Goodbye

Pastikan channel sesuai `.env` sudah ada. Bot akan mengirim embed otomatis saat member masuk/keluar.

Placeholder yang tersedia:
- `{user}` = mention member
- `{username}` = username

Untuk banner, isi `WELCOME_IMAGE_URL` dan `GOODBYE_IMAGE_URL` dengan URL gambar yang dapat diakses Discord.

## Self-role

Contoh:

```env
ROLE_BUTTONS=Announcement|123456789012345678|📢,Giveaway|234567890123456789|🎁,Partner|345678901234567890|🤝
```

Bot harus memiliki permission **Manage Roles**, dan role bot harus berada di atas role self-role.

## Discord Developer Portal

Aktifkan **Server Members Intent** untuk Welcome/Goodbye.

Bot invite membutuhkan minimal permission:
- View Channels
- Send Messages
- Read Message History
- Embed Links
- Attach Files
- Manage Channels
- Manage Roles

## Docker

```bash
docker build -t RManajemen_RebelRp .
docker run -d --name RManajemen_RebelRp --restart unless-stopped --env-file .env -v "$(pwd)/data:/app/data" RManajemen_RebelRp
```

## Penyimpanan

Database berada di:

```text
data/tickets.db
```

Folder `data` sebaiknya dipasang sebagai volume ketika memakai Docker agar data tiket tetap ada setelah container dibuat ulang.

## Keamanan

Jangan pernah membagikan `DISCORD_TOKEN` atau file `.env`. Jika token pernah tersebar, reset token di Discord Developer Portal.
