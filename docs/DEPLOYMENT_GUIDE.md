# Panduan Deploy GitBook Documentation

## Langkah 1: Push ke GitHub Repository

### 1.1 Inisialisasi Git (jika belum)

```bash
# Inisialisasi git
git init

# Tambahkan semua file
git add .

# Commit pertama
git commit -m "Initial commit: Complete BethNa documentation structure"
```

### 1.2 Buat Repository di GitHub

1. Buka https://github.com
2. Klik tombol **"New"** atau **"+"** > **"New repository"**
3. Isi detail:
   - **Repository name**: `bethna-docs`
   - **Description**: "Official documentation for BethNa AI Trader"
   - **Visibility**: Public (atau Private jika preferred)
   - **JANGAN** centang "Add a README file" (karena sudah ada)
4. Klik **"Create repository"**

### 1.3 Connect dan Push

```bash
# Tambahkan remote repository (ganti YOUR-USERNAME dengan username GitHub Anda)
git remote add origin https://github.com/YOUR-USERNAME/bethna-docs.git

# Rename branch ke main (jika perlu)
git branch -M main

# Push ke GitHub
git push -u origin main
```

**Contoh:**
```bash
git remote add origin https://github.com/akbarwjyy/bethna-docs.git
git branch -M main
git push -u origin main
```

---

## Langkah 2: Connect ke GitBook

### 2.1 Buat Akun GitBook

1. Buka https://app.gitbook.com
2. **Sign up** atau **Log in**
3. Pilih plan (Free plan sudah cukup untuk start)

### 2.2 Buat Space Baru

1. Di GitBook dashboard, klik **"New Space"**
2. Pilih **"Import from Git"**
3. Klik **"GitHub"**
4. Authorize GitBook untuk akses GitHub Anda

### 2.3 Configure Git Sync

1. Pilih repository: **bethna-docs**
2. Pilih branch: **main**
3. Pilih root directory: **/** (root)
4. Klik **"Import"**

GitBook akan otomatis:
- Membaca `.gitbook.yaml`
- Load `README.md` sebagai cover page
- Load `SUMMARY.md` untuk struktur navigasi
- Render semua markdown files

### 2.4 Verifikasi Import

Cek apakah struktur sudah benar:
- ✅ Cover page muncul (README.md)
- ✅ Table of contents terlihat di sidebar (SUMMARY.md)
- ✅ Semua section dapat diakses
- ✅ Internal links berfungsi

---

## Langkah 3: Customize GitBook

### 3.1 Branding & Design

1. **Sidebar** > **Customize** > **Appearance**
   - **Logo**: Upload logo BethNa (jika ada)
   - **Favicon**: Upload favicon
   - **Primary Color**: `#b68df5` (ungu muda)
   - **Theme**: Dark mode default

2. **Cover Image**
   - Upload banner image untuk README.md
   - Size: 1200x630px (recommended)

### 3.2 Domain Configuration (Optional)

1. **Settings** > **Domains**
2. Klik **"Add custom domain"**
3. Masukkan domain: `docs.bethna.ai` (jika punya)
4. Follow instruksi DNS configuration

### 3.3 Search & Navigation

1. **Settings** > **Search**
   - Enable **"Search"** (default ON)
   
2. **Settings** > **Navigation**
   - Hide/show page titles
   - Configure sidebar behavior

---

## Langkah 4: Publish Documentation

### 4.1 Set Visibility

1. **Share** button (top-right)
2. Pilih visibility:
   - **Public** - Semua orang bisa akses
   - **Unlisted** - Hanya yang punya link
   - **Private** - Hanya invited members

3. Klik **"Publish"**

### 4.2 Get Shareable Link

Setelah publish, GitBook akan memberikan URL:
- Format: `https://YOUR-ORG.gitbook.io/bethna-docs`
- Atau custom domain: `https://docs.bethna.ai`

---

## Langkah 5: Update Documentation (Workflow)

### 5.1 Update via GitHub

```bash
# Edit file di local
# Misal: edit introduction/README.md

# Commit changes
git add .
git commit -m "Update: Improve executive summary"

# Push ke GitHub
git push origin main
```

**GitBook akan otomatis sync dalam ~1-2 menit**

### 5.2 Update via GitBook Editor (Alternative)

1. Buka space di GitBook
2. Klik **"Edit"** (top-right)
3. Edit content langsung di GitBook
4. Klik **"Merge"** untuk apply changes
5. GitBook akan otomatis push ke GitHub

---

## Troubleshooting

### Issue 1: Git Push Ditolak

```bash
# Jika sudah ada remote
git remote remove origin
git remote add origin https://github.com/YOUR-USERNAME/bethna-docs.git
git push -u origin main --force
```

### Issue 2: GitBook Tidak Sync

1. Cek **Settings** > **Git Sync** > **Status**
2. Klik **"Re-import"** jika ada error
3. Pastikan `.gitbook.yaml` valid

### Issue 3: SUMMARY.md Tidak Terdeteksi

Pastikan `.gitbook.yaml` correct:
```yaml
root: ./

structure:
  readme: README.md
  summary: SUMMARY.md
```

### Issue 4: Broken Links

- Pastikan semua path relatif correct
- Cek file names (case-sensitive di Linux/GitBook)
- Test links di GitBook preview

---

## Checklist Final

- [ ] Repository pushed ke GitHub
- [ ] GitBook space created dan synced
- [ ] Semua pages visible dan accessible
- [ ] Internal links berfungsi
- [ ] Branding applied (logo, colors)
- [ ] Search berfungsi
- [ ] Documentation published (public/unlisted)
- [ ] Shareable link obtained
- [ ] Custom domain configured (optional)

---

## Quick Commands Reference

```bash
# Initial setup
git init
git add .
git commit -m "Initial commit"
git remote add origin https://github.com/YOUR-USERNAME/bethna-docs.git
git branch -M main
git push -u origin main

# Update documentation
git add .
git commit -m "Update: description of changes"
git push origin main

# Check status
git status
git log --oneline

# View remote
git remote -v
```

---

## Support Links

- GitBook Documentation: https://docs.gitbook.com
- GitHub Docs: https://docs.github.com
- Markdown Guide: https://www.markdownguide.org

---

**Last Updated**: January 10, 2026  
**Version**: 1.0
