# دليل التشغيل والإدارة

## المحتويات
1. [مراقبة الأداء](#مراقبة-الأداء)
2. [إدارة التحديثات](#إدارة-التحديثات)
3. [إدارة الأمان](#إدارة-الأمان)
4. [إدارة قاعدة البيانات](#إدارة-قاعدة-البيانات)
5. [الدعم الفني](#الدعم-الفني)
6. [استكشاف الأخطاء](#استكشاف-الأخطاء)

---

## مراقبة الأداء

### مؤشرات الأداء الرئيسية (KPIs)
- **وقت التحميل:** يجب أن يكون أقل من 3 ثواني
- **معدل الخطأ:** يجب أن يكون أقل من 0.1%
- **توفر الخدمة:** يجب أن يكون أكثر من 99.9%
- **سرعة الاستجابة:** يجب أن تكون أقل من 200 ملي ثانية

### أدوات المراقبة
```bash
# مراقبة الخادم
top
htop
iotop

# مراقبة الشبكة
netstat -an
ss -an

# مراقبة قاعدة البيانات
psql -c "SELECT * FROM pg_stat_statements;"
```

### تحليل السجلات
```bash
# عرض سجلات التطبيق
tail -f /var/log/deltastars/app.log

# البحث عن الأخطاء
grep "ERROR" /var/log/deltastars/app.log

# تحليل الأداء
grep "response_time" /var/log/deltastars/app.log | awk '{sum+=$NF; count++} END {print "Average:", sum/count}'
```

---

## إدارة التحديثات

### عملية التحديث
```bash
# 1. سحب الكود الجديد
git pull origin main

# 2. تثبيت التبعيات الجديدة
npm install

# 3. تشغيل الاختبارات
npm test

# 4. بناء المشروع
npm run build

# 5. نسخ احتياطية
cp -r dist dist.backup

# 6. نشر الإصدار الجديد
npm run deploy

# 7. التحقق من الصحة
curl https://deltastars.store/health
```

### جدول التحديثات
- **التحديثات الأمنية:** فوراً عند الاكتشاف
- **إصلاح الأخطاء:** أسبوعياً
- **الميزات الجديدة:** شهرياً
- **التحديثات الرئيسية:** كل ثلاثة أشهر

### إدارة الإصدارات
```bash
# عرض الإصدار الحالي
npm run version:show

# زيادة الإصدار الصغير
npm run version:minor

# زيادة الإصدار الرئيسي
npm run version:major

# زيادة الإصدار الصغير جداً
npm run version:patch
```

---

## إدارة الأمان

### فحص الثغرات الأمنية
```bash
# فحص التبعيات
npm audit

# إصلاح الثغرات
npm audit fix

# إصلاح قسري
npm audit fix --force
```

### تحديث شهادات SSL
```bash
# التحقق من صلاحية الشهادة
openssl x509 -in /etc/ssl/certs/deltastars.crt -noout -dates

# تجديد الشهادة (Let's Encrypt)
certbot renew --dry-run
certbot renew
```

### إدارة المفاتيح والأسرار
```bash
# تدوير مفاتيح API
# 1. إنشاء مفتاح جديد
# 2. تحديث التطبيق بالمفتاح الجديد
# 3. حذف المفتاح القديم

# تحديث متغيرات البيئة
nano .env
systemctl restart deltastars
```

### النسخ الاحتياطية
```bash
# نسخة احتياطية يومية من قاعدة البيانات
pg_dump deltastars > /backups/deltastars-$(date +%Y%m%d).sql

# نسخة احتياطية من الملفات
tar -czf /backups/files-$(date +%Y%m%d).tar.gz /var/www/deltastars/

# التحقق من النسخ الاحتياطية
ls -lh /backups/
```

---

## إدارة قاعدة البيانات

### الاتصال بقاعدة البيانات
```bash
# الاتصال بـ Supabase
psql -h rgusisancfcdabfnfwoy.supabase.co \
     -U postgres \
     -d postgres \
     -p 5432
```

### تحسين الأداء
```sql
-- إنشاء فهارس
CREATE INDEX idx_orders_user_id ON orders(user_id);
CREATE INDEX idx_products_category ON products(category_id);

-- تحليل الجداول
ANALYZE orders;
ANALYZE products;

-- عرض إحصائيات الاستخدام
SELECT * FROM pg_stat_user_tables;
```

### تنظيف البيانات
```sql
-- حذف الطلبات القديمة (أكثر من سنة)
DELETE FROM orders WHERE created_at < NOW() - INTERVAL '1 year';

-- أرشفة البيانات
INSERT INTO orders_archive SELECT * FROM orders WHERE created_at < NOW() - INTERVAL '1 year';
```

---

## الدعم الفني

### قنوات الدعم
- **البريد الإلكتروني:** support@deltastars.store
- **الهاتف:** +966-558828009
- **الواتس آب:** +966-558828009
- **الدردشة المباشرة:** على الموقع

### ساعات العمل
- **السبت - الخميس:** 8:00 صباحاً - 11:00 مساءً
- **الجمعة:** مغلق

### إجراءات الدعم
1. استقبال الشكوى من العميل
2. تسجيل الشكوى في نظام التذاكر
3. تعيين الشكوى لفريق الدعم المناسب
4. حل المشكلة والتواصل مع العميل
5. إغلاق التذكرة بعد التأكد من الحل

---

## استكشاف الأخطاء

### الأخطاء الشائعة

#### 1. خطأ الاتصال بقاعدة البيانات
```
Error: connect ECONNREFUSED 127.0.0.1:5432
```
**الحل:**
```bash
# التحقق من حالة قاعدة البيانات
systemctl status postgresql

# إعادة تشغيل قاعدة البيانات
systemctl restart postgresql

# التحقق من معلومات الاتصال
echo $DATABASE_URL
```

#### 2. خطأ في الذاكرة
```
Error: JavaScript heap out of memory
```
**الحل:**
```bash
# زيادة حد الذاكرة
NODE_OPTIONS="--max-old-space-size=4096" npm start

# تنظيف الذاكرة
npm cache clean --force
```

#### 3. خطأ في الملفات
```
Error: ENOENT: no such file or directory
```
**الحل:**
```bash
# التحقق من وجود الملف
ls -la /path/to/file

# إعادة بناء المشروع
npm run build

# التحقق من الأذونات
chmod 755 /path/to/file
```

### تصحيح الأخطاء
```bash
# تفعيل وضع التصحيح
DEBUG=* npm start

# عرض السجلات المفصلة
tail -f /var/log/deltastars/debug.log
```

---

## الإجراءات الروتينية

### فحص يومي
- [ ] التحقق من توفر الخدمة
- [ ] مراجعة سجلات الأخطاء
- [ ] التحقق من الأداء
- [ ] مراجعة الشكاوى الجديدة

### فحص أسبوعي
- [ ] تحديث التبعيات
- [ ] فحص الثغرات الأمنية
- [ ] تحليل الأداء
- [ ] مراجعة النسخ الاحتياطية

### فحص شهري
- [ ] تحديث الشهادات
- [ ] تحليل استخدام الموارد
- [ ] تحسين الأداء
- [ ] مراجعة الإحصائيات

---

**آخر تحديث:** 2026-08-03  
**الإصدار:** 2.2 Enhanced
