# Devir notu — bulut oturumundan yerel Claude Code'a geçiş

Tarih: 4 Kasım 2026. Bu dosya, bulutta yapılan işin durumunu ve yerel bir Claude Code oturumunda kaldığı yerden devam etmek için gereken bağlamı içerir.

## Yerelde nasıl devam edilir

1. Bilgisayarında Node 20+ ve Claude Code kurulu olsun.
2. Repoları klonla (hepsi private, GitHub hesabın `msaydemir`):
   ```bash
   mkdir -p ~/apps && cd ~/apps
   for r in star-match merge-pets color-gobbler draw-bridge bubble-pop msaydemir.github.io; do git clone https://github.com/msaydemir/$r; done
   ```
3. Hangi uygulama üzerinde çalışacaksan o klasörde `claude` çalıştır. Her repoda `CLAUDE.md` var; Claude kuralları oradan okur.
4. İlk mesaj olarak aşağıdaki "Başlangıç mesajı"nı yapıştır.

## Başlangıç mesajı (yerel oturuma yapıştır)

> Bu repo, mağazaya hazır beş Expo/React Native mobil oyundan biri (star-match, merge-pets, color-gobbler, draw-bridge, bubble-pop). Hepsi aynı şablondan türedi: `app.json` mağaza yapılandırması, `eas.json` profilleri, ikon/splash seti, `docs/RELEASE_CHECKLIST.md`, `docs/store/` (mağaza metinleri TR+EN, ekran görüntüleri `screenshots/ios-6.9/` ve `screenshots/android-phone/`, `feature-graphic.png`), `docs/privacy/privacy-policy.html` (https://msaydemir.github.io/<repo>/privacy-policy.html adresinde yayında). Destek e-postası msaidaydemir@gmail.com. Kalite kapısı `bash scripts/check.sh --full` ve `npx expo-doctor`; ikisi de temiz olmalı. Paketler yalnızca `npx expo install` ile. `eas build/submit` komutlarını benim onayım olmadan çalıştırma. Tüm plan ve hesap adımları https://github.com/msaydemir/msaydemir.github.io/blob/main/HESAP_KURULUM.md ve DEVIR.md dosyalarında. Önce `git status` ve `git log --oneline -5` ile durumu gör, sonra DEVIR.md'deki "Sıradaki işler" listesinden devam et.

## Repoların durumu

| Repo | Oyun | main'de ne var | Açık PR |
|---|---|---|---|
| star-match | Star Match (hafıza kart oyunu) | Mağaza yapılandırması, ikon, SDK 57 düzeltmeleri (PR #1 birleşti) | #2: destek e-postası, gizlilik URL'si, ekran görüntüleri, tanıtım görseli |
| merge-pets | Pati Birleştir (Suika tarzı birleştirme) | Oyunun tamamı (PR #1 birleşti) | #2: aynı içerik |
| color-gobbler | Renkli Obur (renk eşleştirme koşu) | Oyunun tamamı (PR #1 birleşti) | #2: aynı içerik |
| draw-bridge | Köprü Çiz (fizik bulmacası, 12 seviye) | Oyunun tamamı (PR #1 birleşti) | #2: aynı içerik |
| bubble-pop | Balon Patlat ASMR (3 modlu oyuncak) | Oyunun tamamı (PR #1 birleşti) | #2: aynı içerik |
| muhtar | Muhtar (konum tabanlı günlük bulmaca, gerçek mahalle sınırları, Supabase; veri: `muhtar-data` deposu) | Prototip v0.2 | #1 (taslak): tüm proje |
| msaydemir.github.io | Gizlilik politikaları + rehberler | Yayında: https://msaydemir.github.io/ | — |

PR #2'ler birleştirilmeyi bekliyor (sahibin onayı gerekir). Dal adı hepsinde `ccr-2bced9ff-2vu1ud`.

Paket kimlikleri: `com.msaydemir.starmatch`, `.mergepets`, `.colorgobbler`, `.drawbridge`, `.bubblepop`, `.muhtar`. Mağazaya yüklendikten sonra değişmez.

## Hesap durumu

- Google Play Console: saidaydemir87@gmail.com ile açıldı (4 Kasım). Kimlik doğrulaması ve servis hesabı JSON'u bekleniyor (bkz. HESAP_KURULUM.md §2).
- Apple Developer: henüz açılmadı.
- Expo (EAS): henüz açılmadı. Her repoda `npx eas-cli@latest init` çalıştırılıp `app.json`'a gelen `extra.eas.projectId` commit edilecek.
- AdMob: yok. Reklam v1'de bilerek yok; eklenince HESAP_KURULUM.md §5.

## Sıradaki işler (öncelik sırasıyla)

1. **PR #2'leri birleştir** (beş repo). GitHub'da "Merge pull request".
2. **Expo hesabı + `eas init`** (beş repo), projectId'leri commit et.
3. **Preview APK ile cihaz testi**: `npx eas-cli@latest build --platform android --profile preview`, telefona kur. Özellikle:
   - Köprü Çiz: seviye 2–12 hiç oynanmadı; mürekkep bütçesi veya platform konumu ayarı gerekebilir. Bilinen durum: top merkez hizasında çizilen çizgi topu sıkıştırıyor, platform üst hizasında çizgi çalışıyor. Çözüm fikri: topun başlangıçta üst üste bindiği mürekkep parçalarını yok say.
   - Renkli Obur: kaydırma eşiği (20 px) ve hız eğrisi gerçek parmakla denensin.
   - Pati Birleştir: 360 px genişlikte hayvan zinciri ikinci satıra taşıyor, kozmetik.
   - Hepsinde ses gecikmesi ve titreşim hissi.
4. **Play Console**: kimlik doğrulaması bitince servis hesabı JSON'u, sonra her uygulama için kayıt + kapalı test (12 kişi, 14 gün). Form cevapları HESAP_KURULUM.md ve her reponun RELEASE_CHECKLIST.md dosyasında.
5. **Apple Developer**: kayıt, Team ID ve API anahtarı; `eas.json` submit alanları.
6. **Production build + submit** (HESAP_KURULUM.md §4).
7. Sonraki sürümde: AdMob entegrasyonu, Star Match için Firebase çevrimiçi mod testi.
8. **Muhtar** (repo `muhtar`; PR #1 22 Kasım'da `main`'e birleşti, atlı akını dahil; web v0.6 `muhtar/app/` altında yayında: PR #2 meydan muharebesi (savaş sahnesi) dalından, `experiments.baseUrl: /muhtar/app` ile derlendi; PR #2 sahibin onayını bekliyor, yaş derecelendirmesi kararı §20.12): Supabase projesi aç ve `supabase/README.md` adımlarını uygula (anon key'i `src/backend/config.ts`'e yaz); `eas build --profile preview` ile cihazda konum + harita testi (Expo Go çalışmaz); İstanbul'da 10–20 kişiyle 2 haftalık test. v0.2 listesi `docs/RELEASE_CHECKLIST.md` §5'te (rumuz filtresi, hesap silme, sahte GPS önlemi, bildirimler).

## Bilinen teknik notlar

- Expo web export (`npx expo export -p web`) paylaşılan Metro önbelleği yüzünden başka projenin `app.json` adını gömebilir; ekran görüntüsü alırken `--clear` kullan ve Ayarlar › Hakkında'da doğru uygulama adını doğrula.
- `.gitignore` içindeki `android/` kuralı `docs/**/android/` klasörlerini de yok sayar; bu yüzden Android ekran görüntüleri `android-phone/` klasöründe.
- `PRIVACY_POLICY_URL` ve `SUPPORT_CONTACT` sabitleri `: string` olarak tiplenmeli, yoksa `SettingsScreen` içindeki `!== ''` karşılaştırması tsc hatası verir.
- Shallow klonlarda `--force-with-lease` için açık lease gerekir (`--force-with-lease=<dal>:<eski-sha>`).
