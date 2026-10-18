# ساخت APK «خانه‌یاب» با GitHub (بدون Android Studio)

با این روش، خود GitHub برنامه را روی سرورهایش می‌سازد و امضا می‌کند و فایل APK را به تو می‌دهد.
نیازی به نصب Android Studio، Node یا Java روی کامپیوتر نیست.

> این روش روی سیستم من آزمایش نشده است (اینترنت و اندروید ندارم). اگر ساخت در GitHub خطا داد،
> متن خطا را برایم بفرست؛ معمولاً با یک اصلاح کوچک درست می‌شود.

## ۱) ساخت حساب و مخزن
1. در **github.com** ثبت‌نام کن (ممکن است فیلترشکن لازم باشد).
2. بالا سمت راست **+** ← **New repository**.
3. اسم: `khaneyab` — حتماً **Private** (خصوصی) را انتخاب کن — بقیه‌ی گزینه‌ها را دست نزن — **Create repository**.

## ۲) آپلود فایل‌ها
1. ZIP را روی کامپیوتر Extract کن.
2. در صفحه‌ی مخزن روی **uploading an existing file** بزن.
3. **همه‌ی فایل‌ها و پوشه‌های داخل پوشه‌ی بازشده** را بکش و رها کن (`www`، `assets`، `scripts`، `package.json`، `capacitor.config.json`، `README-FA.md`، `.gitignore`).
   - خود پوشه‌ی بیرونی را نکش؛ محتوایش را بکش.
4. پایین صفحه **Commit changes** را بزن.

### فایل اتوماسیون (مهم)
پوشه‌ی `.github` ممکن است موقع کشیدن آپلود نشود (چون نامش با نقطه شروع می‌شود). پس این‌طور بساز:
1. در مخزن: **Add file ← Create new file**
2. در کادر اسم دقیقاً بنویس: `.github/workflows/build-apk.yml` (وقتی `/` را بنویسی پوشه ساخته می‌شود)
3. محتوای فایل `build-apk.yml` (داخل ZIP، پوشه‌ی `.github/workflows`) را کامل کپی و بچسبان.
4. **Commit changes**.

## ۳) ذخیره‌ی کلید امضا (فقط یک بار)
فایل کلید تو `C:\divar-keys\divarmelki.jks` است.
1. کلید ویندوز را بزن، بنویس `PowerShell` و Enter. این دستور را بچسبان و Enter بزن:
   ```
   [Convert]::ToBase64String([IO.File]::ReadAllBytes("C:\divar-keys\divarmelki.jks")) | Set-Clipboard
   ```
   (متن کلید کپی شد؛ چیزی نشان نمی‌دهد.)
2. در GitHub: مخزن ← **Settings ← Secrets and variables ← Actions ← New repository secret**
3. چهار secret بساز:

| Name | Secret (مقدار) |
|---|---|
| `KEYSTORE_BASE64` | همان متن کپی‌شده (Ctrl+V) |
| `KEYSTORE_PASSWORD` | رمز فایل کلید |
| `KEY_ALIAS` | `divarmelki` |
| `KEY_PASSWORD` | رمز کلید |

⚠️ مخزن باید **Private** باشد و این رمزها و فایل کلید را هیچ‌جا دیگر منتشر نکن.

## ۴) ساخت APK
1. تب **Actions** ← در لیست چپ **Build Android APK** ← **Run workflow** ← **Run workflow** (سبز).
2. صبر کن (حدود ۸ تا ۱۵ دقیقه). علامت ✅ سبز یعنی تمام شد.
3. روی همان اجرا کلیک کن؛ پایین صفحه بخش **Artifacts** ← **khaneyab-apk** را دانلود کن.
4. ZIP دانلودشده را باز کن: داخلش `khaneyab-release.apk` است. همین را در بازار و مایکت آپلود کن.

اگر ❌ قرمز شد: روی اجرا کلیک کن، مرحله‌ی قرمز را باز کن و متن آخر آن را برای من بفرست.

## ۵) نسخه‌ی بعدی
- هر بار Run workflow بزنی، شماره‌ی نسخه (versionCode) خودکار بزرگ‌تر می‌شود (لازم برای به‌روزرسانی در فروشگاه).
- برای تغییر در برنامه: فایل‌های داخل `www` را در مخزن عوض کن (Add file ← Upload files) و دوباره Run workflow بزن.

## نکته‌ها
- این بسته‌ی «نسخه‌ی فروشگاه» است: خرید اشتراک ندارد و ثبت آگهی رایگان است (`STORE_BUILD = true` در `www/index.html`).
- فونت و کتابخانه‌ها هنگام ساخت داخل خود APK گذاشته می‌شوند، پس برنامه برای لود شدن به CDN خارجی وابسته نیست.
- نام بسته: `ir.divarmelki.app` (در `capacitor.config.json`). بعد از انتشار در فروشگاه قابل تغییر نیست.
- ساخت محلی (با Android Studio) هم هنوز ممکن است: `npm install` ← `npm run bundle` ← `npm run setup` ← `npm run open`.
