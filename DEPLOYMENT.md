# دليل النشر على متاجر التطبيقات

## المحتويات
1. [نشر على Google Play](#نشر-على-google-play)
2. [نشر على App Store](#نشر-على-app-store)
3. [نشر على الويب](#نشر-على-الويب)
4. [متطلبات عامة](#متطلبات-عامة)

---

## نشر على Google Play

### المتطلبات الأساسية
- حساب Google Play Developer ($25 رسم تسجيل لمرة واحدة)
- ملف APK موقع
- صور وصفية (Screenshots)
- أيقونة التطبيق (512x512 بكسل)
- صورة الميزة (1024x500 بكسل)
- وصف التطبيق والكلمات المفتاحية

### خطوات النشر

#### 1. إعداد ملف APK
```bash
cd /path/to/project
npm run build:apk
```

#### 2. التوقيع الرقمي
```bash
# إنشاء مفتاح التوقيع (Keystore)
keytool -genkey -v -keystore deltastars.keystore \
  -keyalg RSA -keysize 2048 -validity 10000 \
  -alias deltastars

# توقيع APK
jarsigner -verbose -sigalg SHA1withRSA -digestalg SHA1 \
  -keystore deltastars.keystore \
  app-release-unsigned.apk deltastars

# تحسين APK
zipalign -v 4 app-release-unsigned.apk app-release.apk
```

#### 3. إنشاء قائمة التطبيق على Google Play
- الذهاب إلى [Google Play Console](https://play.google.com/console)
- النقر على "إنشاء تطبيق جديد"
- ملء المعلومات الأساسية:
  - اسم التطبيق: "Delta Stars"
  - الفئة: "Shopping"
  - التصنيف العمري: "Everyone"

#### 4. إضافة المعلومات التفصيلية
- **الوصف:** وصف شامل للتطبيق والميزات
- **الكلمات المفتاحية:** delta stars, متجر, تسوق, سعودي
- **الصور:**
  - أيقونة التطبيق (512x512)
  - صورة الميزة (1024x500)
  - لقطات الشاشة (4-8 صور)

#### 5. تحديد المحتوى
- تصنيف المحتوى
- سياسة الخصوصية
- موقع الويب
- البريد الإلكتروني للدعم

#### 6. رفع APK
- الذهاب إلى "الإصدار" → "الإنتاج"
- النقر على "إنشاء إصدار جديد"
- رفع ملف APK الموقع
- إضافة ملاحظات الإصدار

#### 7. المراجعة والنشر
- مراجعة جميع المعلومات
- النقر على "إرسال للمراجعة"
- انتظار موافقة Google (عادة 24-48 ساعة)

---

## نشر على App Store

### المتطلبات الأساسية
- حساب Apple Developer ($99 سنوياً)
- ملف IPA موقع
- شهادة التطوير
- ملف التوفير (Provisioning Profile)
- صور وصفية
- أيقونة التطبيق

### خطوات النشر

#### 1. إعداد ملف IPA
```bash
cd /path/to/project
npm run build:ios
```

#### 2. التوقيع والتوثيق
```bash
# تحميل شهادات التطوير من Apple Developer
# تحميل ملفات التوفير (Provisioning Profiles)

# بناء IPA
xcodebuild -scheme DeltaStars -configuration Release \
  -derivedDataPath build archive \
  -archivePath build/DeltaStars.xcarchive

# تصدير IPA
xcodebuild -exportArchive \
  -archivePath build/DeltaStars.xcarchive \
  -exportOptionsPlist ExportOptions.plist \
  -exportPath build
```

#### 3. إنشاء قائمة التطبيق على App Store
- الذهاب إلى [App Store Connect](https://appstoreconnect.apple.com)
- النقر على "تطبيقات" → "تطبيق جديد"
- ملء المعلومات الأساسية:
  - اسم التطبيق: "Delta Stars"
  - معرف الحزمة: "com.deltastars.store"
  - الفئة: "Shopping"

#### 4. إضافة المعلومات التفصيلية
- **الوصف:** وصف شامل للتطبيق
- **الكلمات المفتاحية:** delta stars, متجر, تسوق
- **الفئة الفرعية:** E-Commerce
- **الصور:**
  - أيقونة التطبيق (1024x1024)
  - لقطات الشاشة (2-5 صور لكل جهاز)

#### 5. تحديد المحتوى
- تصنيف المحتوى
- سياسة الخصوصية
- موقع الويب
- البريد الإلكتروني للدعم

#### 6. رفع IPA
- الذهاب إلى "الإصدارات" → "إصدار جديد"
- رفع ملف IPA
- إضافة ملاحظات الإصدار

#### 7. المراجعة والنشر
- مراجعة جميع المعلومات
- النقر على "إرسال للمراجعة"
- انتظار موافقة Apple (عادة 24-48 ساعة)

---

## نشر على الويب

### على Netlify
```bash
# تثبيت Netlify CLI
npm install -g netlify-cli

# تسجيل الدخول
netlify login

# نشر المشروع
netlify deploy --prod
```

### على Vercel
```bash
# تثبيت Vercel CLI
npm install -g vercel

# نشر المشروع
vercel --prod
```

### على خادم مخصص
```bash
# بناء المشروع
npm run build

# نسخ الملفات إلى الخادم
scp -r dist/* user@server:/var/www/deltastars/

# إعادة تشغيل الخادم
ssh user@server 'sudo systemctl restart nginx'
```

---

## متطلبات عامة

### معايير الجودة
- ✅ لا توجد أخطاء في البناء
- ✅ التطبيق يعمل على جميع الأجهزة المدعومة
- ✅ جميع الميزات تعمل بشكل صحيح
- ✅ الأداء مقبول (وقت التحميل < 3 ثواني)
- ✅ الأمان مطبق بشكل صحيح

### سياسة الخصوصية
- يجب توفير سياسة خصوصية واضحة
- يجب الامتثال لقوانين حماية البيانات
- يجب الإفصاح عن جمع البيانات

### الدعم الفني
- بريد إلكتروني للدعم: support@deltastars.store
- هاتف: +966-558828009
- ساعات العمل: 8:00 صباحاً - 11:00 مساءً

### التحديثات المستقبلية
- خطة تحديثات منتظمة
- إصلاح الأخطاء والمشاكل
- إضافة ميزات جديدة

---

## الملاحظات المهمة

1. **الأمان:** تأكد من أن جميع مفاتيح API آمنة ولا تُكشف في الكود
2. **الاختبار:** اختبر التطبيق على أجهزة متعددة قبل النشر
3. **الامتثال:** تأكد من الامتثال لجميع سياسات المتاجر
4. **الدعم:** جهز فريق دعم للرد على استفسارات المستخدمين

---

**آخر تحديث:** 2026-08-03  
**الإصدار:** 2.2 Enhanced
