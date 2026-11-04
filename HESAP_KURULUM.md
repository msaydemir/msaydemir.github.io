# Geliştirici hesapları kurulum rehberi

Beş uygulamayı (Star Match, Pati Birleştir, Renkli Obur, Köprü Çiz, Balon Patlat ASMR) mağazalara yüklemek için gereken hesaplar ve her adımda girilecek bilgiler. Sıra önemlidir: Expo → Google Play → Apple → AdMob (ileride).

Tüm hesaplarda aynı bilgileri kullan, böylece mağaza listeleri ve gizlilik politikası birbirini tutar:

| Alan | Değer |
|---|---|
| Geliştirici adı (mağazada görünen) | Said Aydemir |
| Destek e-postası | msaidaydemir@gmail.com |
| Web sitesi | https://msaydemir.github.io/ |
| Gizlilik politikası (uygulama başına) | https://msaydemir.github.io/`<repo>`/privacy-policy.html |

Repo adları: `star-match`, `merge-pets`, `color-gobbler`, `draw-bridge`, `bubble-pop`.

---

## 1. Expo (EAS) — ücretsiz, 5 dakika

Build'ler burada, bulutta alınır. Mac gerekmez.

1. https://expo.dev → Sign up. Kullanıcı adı olarak `msaydemir` öner (app.json'daki `owner` alanı için).
2. E-postayı doğrula.
3. Bilgisayarında Node 20+ kuruluyken her repo klasöründe sırayla:
   ```bash
   npx eas-cli@latest login
   npx eas-cli@latest init        # app.json'a extra.eas.projectId yazar → commit et
   ```
4. Ücretsiz plan ayda 30 build verir; 5 uygulama × (Android + iOS) = 10 build, yeterli. Kuyruk bekleme süresi ücretsiz planda 30 dakikaya kadar çıkabilir.

## 2. Google Play Console — 25 $ tek seferlik, onay 1–3 gün

1. https://play.google.com/console → Google hesabınla (msaidaydemir@gmail.com öner) "Create developer account".
2. Hesap türü: **Kişisel** (Personal). Şirket hesabı için DUNS numarası gerekir, gerek yok.
3. İstenenler: tam ad, adres, telefon (SMS doğrulama), kimlik belgesi (TC kimlik veya pasaport fotoğrafı), 25 $ kart ödemesi.
4. Geliştirici adı: `Said Aydemir`. E-posta: `msaidaydemir@gmail.com`. Web sitesi: `https://msaydemir.github.io/`.
5. **Önemli kural (kişisel hesap):** üretime çıkmadan önce her uygulama için **kapalı test**: en az 12 test kullanıcısı, kesintisiz 14 gün. Aile ve arkadaşların Gmail adreslerini şimdiden bir listeye yaz. Test linki konsoldan alınır, kullanıcılar linke tıklayıp uygulamayı yükler.
6. Onay gelince **Setup → API access** bölümünden bir servis hesabı oluştur ("Create new service account" → Google Cloud'da anahtar JSON indir → Play Console'da "Release manager" yetkisi ver). İndirilen dosyayı her repoda `google-service-account.json` adıyla kök klasöre koy. `.gitignore`'da zaten engelli; **asla commit etme**. Bu dosya `eas submit --platform android` için gerekir.

### Her uygulama için konsolda doldurulacaklar
- **Create app**: ad (listing dosyasındaki 30 karakterlik ad), dil Türkçe, Oyun, Ücretsiz.
- **Store listing**: `docs/store/listing-tr.md` ve `listing-en.md` metinleri; ikon `assets/icon.png` (512×512'ye küçült, konsol kabul eder); tanıtım görseli `docs/store/feature-graphic.png`; ekran görüntüleri `docs/store/screenshots/android/`.
- **App content** (sol menü → Policy):
  - Privacy policy: uygulamanın URL'si (yukarıdaki tablo).
  - Ads: **Hayır** (bu sürümde reklam yok).
  - App access: "All functionality is available without special access".
  - Content rating (IARC): Oyun; şiddet yok; kullanıcı etkileşimi **Hayır** (Star Match'te Firebase kapalıysa Hayır). Sonuç: PEGI 3 / Everyone.
  - Target audience: 13 ve üzeri. "Çocuklara yönelik" seçme (Families politikası ekstra zorunluluk getirir).
  - News app: Hayır. COVID: Hayır. Data safety: "Does your app collect or share user data?" → **Hayır**. Government app: Hayır. Financial: Hayır. Health: Hayır.
- **Testing → Closed testing**: track oluştur, 12+ e-posta ekle, `eas submit` çıktısını bu track'e al, 14 gün bekle, sonra "Apply for production access".

## 3. Apple Developer Program — 99 $/yıl, onay 1–2 gün

1. İki aşamalı doğrulama açık bir Apple ID gerekir (iPhone'daki hesabın olur). Apple ID e-postası olarak msaidaydemir@gmail.com kullanmak zorunda değilsin, ama destek e-postası mağazada ayrıca girilir.
2. https://developer.apple.com/programs/enroll/ → **Bireysel (Individual)** olarak kaydol. İstenenler: tam ad (kimlikle aynı), adres, telefon, kimlik doğrulama (bazen Apple Developer uygulamasından kimlik fotoğrafı ister), 99 $ ödeme.
3. Onay e-postası gelince https://appstoreconnect.apple.com açılır.
4. **Users and Access → Integrations → App Store Connect API**: "Team Key" oluştur, rol "App Manager". `.p8` dosyasını indir, **Key ID** ve **Issuer ID**'yi not al. Bunlar `eas submit --platform ios` için gerekir; EAS ilk seferde ister ve kendi kasasında saklar. `.p8` dosyasını commit etme.
5. **Team ID**: https://developer.apple.com/account → Membership details. Her reponun `eas.json` dosyasındaki `appleTeamId` alanına yaz.

### Her uygulama için App Store Connect'te
- **My Apps → +**: Platform iOS, ad (listing dosyasındaki), birincil dil Türkçe, Bundle ID (`com.msaydemir.<pkg>`, ilk `eas build` sonrası listede görünür; önce EAS build al), SKU = bundle id.
- Oluşunca uygulamanın **Apple ID (sayısal, "App Information" sayfasında)** değerini `eas.json` → `submit.production.ios.ascAppId` alanına yaz; `appleId` alanına Apple ID e-postanı yaz.
- **App Information**: kategori Oyunlar › Bulmaca (Balon Patlat için Oyunlar › Casual), içerik hakları: üçüncü taraf içerik var (sadece Star Match: Wikimedia Commons fotoğrafları, lisanslı), diğerleri hayır.
- **App Privacy**: "Data Not Collected" (Star Match'te Firebase kapalıyken de aynı).
- **Age Rating**: tüm sorular Hayır/None → 4+.
- **Pricing**: Ücretsiz, tüm ülkeler.
- **Version 1.0.0**: ekran görüntüleri `docs/store/screenshots/ios-6.9/` (6.9" iPhone zorunlu, iPad gerekmez), tanıtım metni, açıklama, anahtar kelimeler, destek URL `https://msaydemir.github.io/`, pazarlama URL boş, gizlilik politikası URL'si.
- **App Review Information**: iletişim adı/telefon/e-posta; demo hesabı gerekmez; notlar alanına `docs/RELEASE_CHECKLIST.md` içindeki "Review notes" metnini yapıştır.
- Export compliance sorusu `usesNonExemptEncryption=false` sayesinde sorulmaz.

## 4. Build ve gönderim sırası (hesaplar hazır olunca, her repo için)

```bash
npx eas-cli@latest login
npx eas-cli@latest init                                      # bir kez, projectId'yi commit et
npx eas-cli@latest build --platform android --profile preview  # APK, telefona kur, oyna
npx eas-cli@latest build --platform android --profile production
npx eas-cli@latest build --platform ios --profile production    # Apple hesabınla giriş ister, sertifikayı kendisi üretir
npx eas-cli@latest submit --platform android --profile production   # google-service-account.json gerekir
npx eas-cli@latest submit --platform ios --profile production       # API key (p8) ister
```

Her uygulama için toplam süre: build 15–30 dk, Play kapalı test 14 gün, Apple inceleme 1–7 gün.

## 5. AdMob (ileride, reklam eklenince)

1. https://admob.google.com → Play ile aynı Google hesabı.
2. Her uygulama için "Add app" → App ID'leri (`ca-app-pub-…~…`) ve banner / geçiş reklam birimi ID'leri al.
3. Bana ID'leri ver; `react-native-google-mobile-ads`, ATT izni ve UMP onay formunu beş uygulamaya birden eklerim, mağaza beyanlarını (Play "Ads: Yes", Apple "Tracking") güncelleriz.
4. AdMob yeni hesabı birkaç günde onaylar; reklam uygulama yayına girip mağaza bağlantısı yapılana kadar görünmez.

## 6. Kontrol listesi

- [ ] Expo hesabı + 5 repoda `eas init` commit'lendi
- [ ] Play Console hesabı onaylandı, servis hesabı JSON'u indirildi
- [ ] 12 test kullanıcısının e-postaları hazır
- [ ] Apple Developer onaylandı, Team ID ve API key alındı
- [ ] `eas.json` submit alanları dolduruldu (5 repo)
- [ ] Her uygulama için preview APK telefonda denendi
- [ ] Mağaza kayıtları açıldı, metin ve görseller yüklendi
- [ ] Production build + submit
