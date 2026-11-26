# Mekanik + İSG Depo 2027 · V8.7

Mekanik ve İSG depoları için rol bazlı stok yönetimi, onay havuzu, planlı bakım reçeteleri, tüketim analizleri ve **bağımsız depo QR çıkış terminali**.

## Bileşenler

| Dosya | Açıklama |
| --- | --- |
| `index.html` | Ana yönetim paneli (tek HTML · Demo, Doğrudan GAS veya `/api/gas` destekli) |
| `qr-terminal.html` | Bağımsız depo QR çıkış terminali (kök dizinde, doğrudan ve `dist/` içinde çalışır) |
| `.github/workflows/deploy.yml` | `main` dalına her gönderimde GitHub Pages'e otomatik yayınlama |

### Bağımsız QR Çıkış Terminali Akışı (`qr-terminal.html`)
1. Depodaki raf QR kodu (veya `ISG-DOLAP`) okutulur ya da `?qr=MEK-0001` bağlantısı ile açılır.
2. Sistem önce **Operatör PIN şifresini** sorar.
3. Doğrulama sonrasında okutulan QR koduna ait stok kartı açılır.
4. Parçada **Depo Onayı Gerekli** işareti varsa Depocu PIN ile anında onaylanır veya Onay Havuzuna gönderilir; onay gerekmiyorsa doğrudan stoktan düşülür.

## GitHub Pages ile Yayınlama (Önerilen)

1. Projeyi GitHub reponuza gönderin:
   ```bash
   git init
   git add .
   git commit -m "Mekanik + ISG Depo V8.7 ilk yayin"
   git branch -M main
   git remote add origin https://github.com/<KULLANICI_ADINIZ>/<REPO_ADINIZ>.git
   git push -u origin main
   ```
2. GitHub reponuzda **Settings → Pages** bölümünü açın.
3. **Build and deployment → Source** seçeneğini **GitHub Actions** yapın.
4. **Actions** sekmesinde `GitHub Pages Yayınla` iş akışı tamamlandığında adresleriniz hazır olur:
   - Ana Panel: `https://<KULLANICI_ADINIZ>.github.io/<REPO_ADINIZ>/`
   - Bağımsız QR Terminali: `https://<KULLANICI_ADINIZ>.github.io/<REPO_ADINIZ>/qr-terminal.html`
   - Doğrudan parça bağlantısı örneği: `https://<KULLANICI_ADINIZ>.github.io/<REPO_ADINIZ>/qr-terminal.html?qr=MEK-0001`

> GitHub Pages HTTPS üzerinden yayınlandığı için mobil cihazlarda arka kamera QR okuyucu doğrudan çalışır.

## Canlı Google Sheets Bağlantısı

GitHub Pages statik barındırma olduğu için canlı Google Sheets bağlantısında **Doğrudan Google Apps Script (`/exec`)** modunu kullanın:

1. Ana paneldeki **Bağlantı** sekmesinden güncel `Kod.gs` kodunu kopyalayıp Google Apps Script projenize yapıştırın.
2. Apps Script içinde **Proje Ayarları → Komut Dosyası Özellikleri** kısmına `ALLOW_DIRECT` = `true` (ve gerekirse `DEPOT_SPREADSHEET_ID`) ekleyin.
3. **Dağıt → Yeni Dağıtım → Web Uygulaması** ile `/exec` adresini alın.
4. Hem ana panelde hem `qr-terminal.html` sağ üstündeki **Bağlantı Ayarı** penceresinde modu **Doğrudan Google Apps Script (`/exec`)** seçip adresi kaydedin (iki ekran aynı tarayıcı ayarını paylaşır).

## Demo Test PIN Kodları

| Rol | PIN |
| --- | --- |
| Bakım Operatörü (`OP`) | `1111` (veya `1155`) |
| Mekanik Depo Sorumlusu (`MEK_ADMIN`) | `2222` |
| İSG Depo Sorumlusu (`ISG_ADMIN`) | `3333` |
| Bakım Şefi / Yönetici (`YONETICI`) | `4444` |

## Yerel Geliştirme

```bash
npm install
npm run dev
npm run build
```
