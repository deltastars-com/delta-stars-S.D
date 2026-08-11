# دليل إدارة مفاتيح التوقيع

## إنشاء Keystore لـ Android

### الخطوة 1: إنشاء مفتاح التوقيع
```bash
keytool -genkey -v -keystore deltastars.keystore \
  -keyalg RSA -keysize 2048 -validity 10000 \
  -alias deltastars \
  -storepass deltastars123 \
  -keypass deltastars123 \
  -dname "CN=Ali Al dahan, OU=Delta Stars, O=Delta Stars Trading, L=Riyadh, S=Riyadh, C=SA"
```

### الخطوة 2: التحقق من المفتاح
```bash
keytool -list -v -keystore deltastars.keystore \
  -storepass deltastars123
```

### الخطوة 3: توقيع APK
```bash
# بناء APK بدون توقيع
./gradlew assembleRelease

# توقيع APK
jarsigner -verbose -sigalg SHA1withRSA -digestalg SHA1 \
  -keystore deltastars.keystore \
  -storepass deltastars123 \
  -keypass deltastars123 \
  app/build/outputs/apk/release/app-release-unsigned.apk \
  deltastars

# تحسين APK
zipalign -v 4 \
  app/build/outputs/apk/release/app-release-unsigned.apk \
  app/build/outputs/apk/release/app-release.apk
```

## ملفات Firebase

### Google Services JSON
- الملف: `google-services.json`
- المسار: `android/app/google-services.json`
- الحصول عليه من: [Firebase Console](https://console.firebase.google.com)

### GoogleService Info Plist
- الملف: `GoogleService-Info.plist`
- المسار: `ios/Runner/GoogleService-Info.plist`
- الحصول عليه من: [Firebase Console](https://console.firebase.google.com)

## الأمان

⚠️ **تحذير:** لا تشارك هذه الملفات علناً
- احفظ `deltastars.keystore` في مكان آمن
- لا تضع كلمات المرور في الكود
- استخدم متغيرات البيئة لتخزين كلمات المرور

## النسخ الاحتياطية

```bash
# إنشاء نسخة احتياطية
cp deltastars.keystore deltastars.keystore.backup

# التحقق من النسخة الاحتياطية
keytool -list -v -keystore deltastars.keystore.backup
```

---

**آخر تحديث:** 2026-08-03
