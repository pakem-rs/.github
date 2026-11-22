<div align="center">

# 🎨 Pakem Framework
### *Look like Laravel, Run at Rust Speed.*

[![Rust](https://img.shields.io/badge/Rust-2021%20Edition-DEA584?style=for-the-badge&logo=rust&logoColor=white)](https://www.rust-lang.org)
[![Version](https://img.shields.io/badge/Release-v1.0.0-6366F1?style=for-the-badge)](https://github.com/pakem-rs/framework/releases)
[![Crates.io](https://img.shields.io/crates/v/pakem?style=for-the-badge&logo=rust&color=e05d44)](https://crates.io/crates/pakem)
[![CLI](https://img.shields.io/badge/CLI-canting-ec4899?style=for-the-badge&logo=rust)](https://crates.io/crates/canting)
[![Axum](https://img.shields.io/badge/Engine-Axum%200.7-3B82F6?style=for-the-badge&logo=tokio&logoColor=white)](https://github.com/tokio-rs/axum)
[![Tokio](https://img.shields.io/badge/Runtime-Tokio%20Async-06B6D4?style=for-the-badge&logo=tokio&logoColor=white)](https://tokio.rs)
[![License: MIT](https://img.shields.io/badge/License-MIT-10B981?style=for-the-badge)](LICENSE)

<br/>

**Pakem** adalah *batteries-included fullstack web framework* modern untuk bahasa **Rust**, terinspirasi dari keanggunan filosofi *Convention over Configuration* (**Pakem**) ala **Laravel**. Menenun mahakarya perangkat lunak dengan ketelitian **Canting** dan kecepatan komputasi ekstrem tanpa *Garbage Collector*.

[📖 Dokumentasi Resmi (pakem.my.id)](https://pakem.my.id) • [Ekosistem](#-ekosistem-resmi-pakem-rs) • [Tolok Ukur](#-keunggulan-tolok-ukur-performa) • [Quickstart](#-panduan-cepat-quickstart)

---

</div>

<br/>

## 🏛️ Filosofi Inti (The 3 Pillars)

<table>
  <tr>
    <td width="33%" align="center">
      <h3>⚡ 1. Fast (Rust Speed)</h3>
      <p>Dibangun di atas Tokio Async Runtime dan Axum Engine. <i>Zero-cost abstractions</i>, tanpa Garbage Collector, memori minimal (~15MB RAM), Throughput 118.000+ RPS, dan latensi sub-milidetik.</p>
    </td>
    <td width="33%" align="center">
      <h3>🎨 2. Beauty (Laravel Elegance)</h3>
      <p>Sintaks deklaratif, ekspresif, dan manusiawi. Ucapkan selamat tinggal pada <i>boilerplate</i> Axum mentah. Unified <code>Request</code> bundle, Fluent ORM, dan Blade Template Native.</p>
    </td>
    <td width="33%" align="center">
      <h3>🛡️ 3. Safety (Enterprise Guard)</h3>
      <p>Zero-Panic Error Boundary (<code>catch_unwind</code>) yang menjamin server anti tumbang, Anti Brute-Force lockout cache, Granular RBAC (<code>@can</code>, <code>@role</code>), dan Audit Trail ISO 27001.</p>
    </td>
  </tr>
</table>

---

## 📦 Ekosistem Resmi pakem-rs

Organisasi **pakem-rs** mengelola suite pustaka modular independen yang saling melengkapi:

| Repositori / Crate | Status | Deskripsi |
| :--- | :---: | :--- |
| [🚀 **pakem-rs/framework**](https://github.com/pakem-rs/framework) | `v1.0.0` | **Core Framework Monorepo**: Engine HTTP, Unified `Request`, Blade View Engine, Fluent ORM, Security, Session, dan Queue System. |
| [📦 **pakem-rs/pakem**](https://github.com/pakem-rs/pakem) | `v1.0.0` | **Official Starter Skeleton**: Template boilerplate aplikasi yang di-clone otomatis saat menjalankan `canting new <nama_project>`. |
| [🎨 **pakem-rs/docs**](https://github.com/pakem-rs/docs) | `v1.0.0` | **Portal Dokumentasi Resmi**: Dokumentasi interaktif VitePress di [pakem.my.id](https://pakem.my.id). |
| [🛠️ **canting**](https://crates.io/crates/canting) | `v1.0.0` | **Artisan CLI Toolchain**: Perintah terminal sang pengrajin (`canting new`, `canting make:*`, `canting migrate`, `canting serve`). |
| [🔮 **pakem_macros**](https://crates.io/crates/pakem_macros) | `v1.0.0` | **Procedural Macros Engine**: Macro `#[derive(PakemModel)]` untuk model ORM dan `#[pakem_form]` untuk validasi form deklaratif. |
| [⚡ **pakem-live**](https://github.com/pakem-rs/framework/tree/master/pakem-live) | `v1.0.0` | **Server-Driven Reactive UI**: Interaktivitas realtime WebSocket streaming (`< 1ms`) ala Livewire dengan Tri-Dialect (`kilat:*`, `live:*`, `wire:*`). |

---

## 💻 Keindahan Sintaksis Controller

### Controller Halaman Web
```rust
use pakem::prelude::*;

pub async fn index(req: Request) -> impl IntoResponse {
    let users = User::query(req.db())
        .where_eq("is_active", true)
        .order_by("created_at", "DESC")
        .get()
        .await
        .unwrap_or_default();

    View::make("dashboard.index")
        .with("title", "Dashboard — Pakem Framework")
        .with("users", users)
        .render(req)
        .await
}
```

### Controller RESTful API
```rust
pub async fn api_users(req: Request) -> PakemResult {
    req.authorize("users.view")?; // Proteksi RBAC 1-baris otomatis HTTP 403 jika tidak berhak
    let (page, per_page) = req.pagination();
    let paginator = User::paginate(req.db(), page, per_page).await?;
    req.paginate(paginator)
}
```

---

## 🚀 Panduan Cepat (Quickstart)

Mulai aplikasi web pertama Anda dalam kurang dari 30 detik:

```bash
# 1. Pasang Canting CLI Toolchain dari Crates.io
cargo install canting

# 2. Bangkitkan aplikasi baru
canting new toko_online

# 3. Masuk ke direktori & jalankan server
cd toko_online
canting serve
```

Server lokal Anda langsung aktif dan siap dibuka di:  
👉 **`http://127.0.0.1:8000`**

---

## 📊 Keunggulan Tolok Ukur Performa

```
  Throughput Permintaan JSON API (wrk -t8 -c128)
  ┌─────────────────────────────────────────────────────────┐
  │ Pakem (Rust)      ████████████████████ 118.420 RPS      │
  │ Go / Gin          ████████████████     96.850 RPS       │
  │ Node / Express    ████                  22.310 RPS      │
  │ PHP / Laravel     ███                   18.940 RPS      │
  └─────────────────────────────────────────────────────────┘

  Konsumsi Memori Server Saat Beban Puncak (10k Koneksi)
  ┌─────────────────────────────────────────────────────────┐
  │ Pakem (Rust)      38 MB (Sangat Hemat)                  │
  │ Go / Gin          48 MB                                 │
  │ Node.js           245 MB                                │
  │ PHP Octane        395 MB                                │
  └─────────────────────────────────────────────────────────┘
```

---

## 🤝 Berkontribusi (Join the Revolution!)

Kami menyambut antusiasme pengembang di seluruh dunia untuk mewujudkan ekosistem web backend paling menyenangkan dan tercepat.

- 📖 **Baca Dokumentasi:** Pelajari seluruh modul di [Portal Dokumentasi Resmi (pakem.my.id)](https://pakem.my.id).
- 🐛 **Temukan bug atau punya ide fitur?** Buka [Issue](https://github.com/pakem-rs/framework/issues) di repositori terkait.
- 💡 **Ingin berkontribusi kode?** Buka **Pull Request** pada repositori [pakem-rs/framework](https://github.com/pakem-rs/framework) atau [pakem-rs/docs](https://github.com/pakem-rs/docs).
- ⭐ **Dukung kami:** Berikan bintang (*star*) pada repositori organisasi pakem-rs agar semakin banyak developer merasakan keindahan Pakem!

---

<div align="center">

Dibuat dengan dedikasi dan cinta ❤️ oleh **Slamet Sugandi** dan seluruh **Pakem Framework Core Team & Contributors**.

<sub>Copyright &copy; 2026 Pakem Ecosystem. Dirilis di bawah lisensi terbuka [MIT](LICENSE).</sub>

</div>
