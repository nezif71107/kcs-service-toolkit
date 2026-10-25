# KCS Service Toolkit - sürümler

**Kyrenia Computer Solutions** teknisyenleri için Windows servis ve teşhis aracı. Bu depo **yalnızca imzalı sürüm dosyalarını** (Releases) barındırır; **kaynak kod içermez**.

Ürün sayfası ve bilgi: **https://axisng.com/kcs**

## İndirme

En son sürüm: [**KCSServiceToolkit.exe**](https://github.com/nezif71107/kcs-service-toolkit/releases/latest/download/KCSServiceToolkit.exe)
(tüm sürümler: [Releases](https://github.com/nezif71107/kcs-service-toolkit/releases)).

- Windows 10 / 11, 64 bit. Tek dosya; .NET kurulumu gerekmez. Yönetici yetkisi ister.
- Dosya henüz Authenticode ile imzalı değildir; Windows SmartScreen "bilinmeyen yayıncı" uyarısı gösterebilir: **Ek bilgi -> Yine de çalıştır**.
- Kurulu uygulama yeni sürümleri kendisi denetler (açılışta, en çok 6 saatte bir) ve onayınızla günceller. Elle denetim: **Genel > Araçlar > Güncellemeleri denetle**.

## Doğrulama

Her sürümün iki dosyası vardır: `KCSServiceToolkit.exe` ve `version.json`.

- `version.json`, ECDSA P-256 ile imzalı bir sürüm bildirimidir; içinde exe'nin **SHA-256** özeti ve boyutu bulunur. Uygulama imzayı kendi içine gömülü açık anahtarla doğrular ve
  indirilen dosyanın boyut/SHA-256 değerleri tutmazsa güncellemeyi uygulamaz.
- Elle doğrulamak için indirdiğiniz exe'nin özetini hesaplayın ve `version.json` içindeki `payload` (base64 -> JSON) alanındaki `sha256` ile karşılaştırın:

```powershell
Get-FileHash .\KCSServiceToolkit.exe -Algorithm SHA256
# version.json içindeki sha256 değerini görmek için:
$j = Get-Content .\version.json -Raw | ConvertFrom-Json
[Text.Encoding]::UTF8.GetString([Convert]::FromBase64String($j.payload)) | ConvertFrom-Json | Select-Object version, sha256, size
```

İki özet (küçük/büyük harf farkı önemsiz) aynı olmalıdır.

---

# English (short)

**KCS Service Toolkit** is a Windows service and diagnostics tool for Kyrenia Computer Solutions technicians. This repository hosts **signed release binaries only** - there is **no source code** here.
Product page: **https://axisng.com/kcs**

- Download the latest build: [KCSServiceToolkit.exe](https://github.com/nezif71107/kcs-service-toolkit/releases/latest/download/KCSServiceToolkit.exe) (Windows 10/11 x64, requires administrator rights, no .NET install needed).
- The binary is not Authenticode-signed yet, so Windows SmartScreen may warn about an unknown publisher (**More info -> Run anyway**).
- **Verification:** each release ships `version.json`, an ECDSA P-256 signed manifest containing the exe's SHA-256 and size. The app verifies the signature with an embedded public key and refuses a download whose
  size or SHA-256 does not match. To check by hand, compare `Get-FileHash KCSServiceToolkit.exe -Algorithm SHA256` with the `sha256` field inside the base64 `payload` of `version.json`.
