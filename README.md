# GlobalStudio AI — Prototyp v2 (Android source)

یہ Android Studio کا **source project** ہے، تیار شدہ AAB نہیں۔ اس میں GlobalStudio AI کا موبائل انٹرفیس، 30 اسٹوڈیوز، text-AI backend starter، اور video/dubbing کے لیے settings/workflow screens شامل ہیں۔

## موجودہ حقیقت

- Text AI صرف تب کام کرے گا جب Node.js backend deploy ہو، `OPENAI_API_KEY` سرور پر سیٹ ہو، اور HTML میں `API_BASE_URL` کو آپ کے HTTPS backend URL پر سیٹ کیا جائے۔
- Text-to-video، video translation/dubbing، lip-sync، voice cloning، image editing اور دیگر specialist media operations **ابھی حقیقی طور پر implement نہیں ہیں**۔ UI میں workflow settings موجود ہیں، مگر یہ خود ویڈیو render یا آواز تبدیل نہیں کرتیں۔ ان کے لیے الگ media providers، سرور-side integration، صارف کی رضامندی اور ٹیسٹنگ درکار ہے۔
- Tokens/spin/daily rewards local prototype ہیں، محفوظ server-side balance نہیں۔ انہیں حقیقی ادائیگی یا قیمتی entitlements کے لیے استعمال نہ کریں۔
- کسی بھی ایپ کے Google Play پر منظور ہونے، برقرار رہنے، یا کبھی ہیک نہ ہونے کی ضمانت ممکن نہیں۔ یہ پروجیکٹ Play review کے لیے تیار/تصدیق شدہ قرار نہیں دیا جا رہا۔

## Android Studio سے AAB بنانے کا طریقہ

1. کمپیوٹر پر تازہ Android Studio اور Android SDK Platform 35 نصب کریں؛ JDK 17 استعمال کریں۔
2. اس فولڈر کو Android Studio میں کھولیں اور Gradle Sync مکمل ہونے دیں۔
3. پہلے debug build چلا کر حقیقی Android فون پر ٹیسٹ کریں۔
4. `Build → Generate Signed Bundle / APK → Android App Bundle` منتخب کریں۔
5. نیا upload key محفوظ جگہ پر backup کریں اور signed `.aab` بنائیں۔
6. Play Console سے پہلے package ID، app icon، screenshots، support email، privacy policy، Data safety، content rating، permissions اور تمام AI/media provider کے استعمال کی جانچ مکمل کریں۔

## Backend setup

1. Node.js 20+ نصب کریں۔
2. `backend/` میں `npm install` چلائیں۔
3. `.env.example` کی نقل `.env` میں بنائیں اور `OPENAI_API_KEY` سیٹ کریں۔ کلید کبھی HTML یا Android app میں نہ رکھیں۔
4. Backend کو HTTPS کے ساتھ deploy کریں اور access control، abuse limits، data retention/deletion، privacy disclosures اور provider billing limits ترتیب دیں۔
5. `index.html` میں `API_BASE_URL` کو deploy شدہ HTTPS backend origin پر سیٹ کریں، پھر app rebuild کریں۔

## کارکردگی اور سکیورٹی

- یہ WebView starter ہے؛ کمزور نیٹ ورک، فون کی صلاحیت، اور provider response time سے رفتار متاثر ہو سکتی ہے۔
- Production launch سے پہلے authentication، server-side token ledger، rate limits، request validation، secure secrets، error handling، media upload limits، deletion/export controls، اور Android/Play policy review درکار ہیں۔
- یہ ZIP source code ہے؛ اس میں compiled، signed یا tested AAB شامل نہیں۔

## موبائل سے ٹیسٹ APK بنانا (Cloud build)

اس ZIP میں `.github/workflows/build-apk.yml` موجود ہے۔ اس سے GitHub Actions کے ذریعے کمپیوٹر کے بغیر debug APK بن سکتا ہے، مگر build کو چلانے کے لیے GitHub اکاؤنٹ اور انٹرنیٹ درکار ہیں:

1. ZIP کو فون میں extract کریں۔
2. GitHub پر نیا repository بنائیں، پھر extracted پروجیکٹ کی تمام فائلیں (بشمول `.github` فولڈر) اپ لوڈ کریں۔ بہتر ہے repository کو private رکھیں، اور کبھی API keys یا `.env` فائل اپ لوڈ نہ کریں۔
3. repository میں **Actions** کھولیں، **Build Android APK** workflow منتخب کریں اور **Run workflow** دبائیں۔ اگر workflow ابھی نظر نہ آئے تو پہلے `main` branch میں تمام فائلیں commit کریں۔
4. Build مکمل ہونے کے بعد run کے صفحے پر **Artifacts** میں `GlobalStudioAI-debug-APK` ڈاؤن لوڈ کریں، ZIP extract کریں، پھر `app-debug.apk` فون پر انسٹال کریں۔ نامعلوم ذرائع سے APK انسٹال کرنے کی Android اجازت درکار ہو سکتی ہے۔
5. یہ debug/test APK ہے، Google Play کی ریلیز کے لیے signed production AAB نہیں۔

نوٹ: Cloud build کی کامیابی GitHub workflow، dependency downloads، اور project compile ہونے پر منحصر ہے۔ موجودہ میڈیا فیچرز میں سے کئی ابھی provider integration کے بغیر فعال نہیں ہیں؛ README کے "موجودہ حقیقت" حصے کو ضرور پڑھیں۔
