# 🧞‍♂️ خوارزمی یار (Kharazmi Yar)

> دستیار هوش مصنوعی برای تولید ایده‌های خلاقانه و غیرتکراری در **جشنواره نوجوان خوارزمی**

مخصوص دانش‌آموزان متوسطه اول (پایه ۷ تا ۹) سمپاد و همه مدارس.

## ✨ ویژگی‌ها

- 🎯 پوشش کامل ۱۴ محور و ۴۲ زیرمحور جشنواره نوجوان خوارزمی
- 🤖 تولید ایده با هوش مصنوعی (مراحل اجرا، مواد لازم، نکات داوری، راه‌های تقویت)
- 🔐 ورود با ایمیل/رمز + گیت‌هاب + حالت مهمان
- ✉️ **تأیید ایمیل** با کد ۶ رقمی هنگام ثبت‌نام
- 🛡 **فیلتر محتوای نامناسب** روی ورودی کاربر و خروجی هوش مصنوعی
- ⏱ **محدودسازی نرخ** (۲۰ ایده/ساعت/کاربر) برای جلوگیری از سوءاستفاده
- 👑 **پنل ادمین** برای مدیریت کاربران و ایده‌ها (آمار، جستجو، حذف، تغییر نقش)
- 📱 **PWA** — قابل نصب روی گوشی مثل اپ واقعی (آفلاین‌پذیر)
- 💾 ذخیره و مدیریت ایده‌ها
- 🔗 اشتراک‌گذاری ایده با مربی یا هم‌گروهی
- 📚 راهنمای نکات طلایی موفقیت در جشنواره
- 📐 طراحی ریسپانسیو موبایل‌اول + راست‌چین فارسی کامل

## 🛠 تکنولوژی‌ها

- **Next.js 16** (App Router) + TypeScript
- **Tailwind CSS 4** + shadcn/ui
- **Prisma ORM** (SQLite در توسعه، PostgreSQL در تولید)
- **NextAuth.js v4** (احراز هویت امن)
- **Zustand** (مدیریت state)
- **z-ai-web-dev-sdk** (هوش مصنوعی)
- **Framer Motion** (انیمیشن)

## 🚀 راه‌اندازی محلی (Development)

### پیش‌نیازها
- نصب [Node.js 18+](https://nodejs.org)
- نصب [Bun](https://bun.sh): `curl -fsSL https://bun.sh/install | bash`

### مراحل

```bash
# ۱. نصب پکیج‌ها
bun install

# ۲. کپی فایل env
cp .env.example .env
# مقادیر واقعی رو در .env وارد کن (حداقل NEXTAUTH_SECRET)

# ۳. ساخت دیتابیس
bun run db:push

# ۴. اجرای سرور توسعه
bun run dev
```

سایت روی `http://localhost:3000` اجرا می‌شه.

## 📦 انتشار روی Vercel (Production)

### قدم ۱: آپلود روی گیت‌هاب
۱. یک مخزن (Repository) جدید در [github.com](https://github.com) بساز
۲. فایل‌های پروژه رو آپلود کن (بدون `node_modules`، `.next`، `db`)

### قدم ۲: انتشار روی Vercel
۱. به [vercel.com](https://vercel.com) برو و با گیت‌هاب وارد شو
۲. **Add New → Project** → مخزون `kharazmi-yar` رو Import کن
۳. Environment Variables رو وارد کن:

| کلید | مقدار |
|------|-------|
| `DATABASE_URL` | اتصال PostgreSQL (Neon/Supabase) |
| `NEXTAUTH_SECRET` | رشته تصادفی ۶۴ کاراکتری |
| `NEXTAUTH_URL` | `https://آدرس-سایت.vercel.app` |
| `GITHUB_ID` | (اختیاری) OAuth App ID |
| `GITHUB_SECRET` | (اختیاری) OAuth App Secret |
| `NEXT_PUBLIC_GITHUB_ENABLED` | `true` یا `false` |

۴. **Deploy** رو بزن

### قدم ۳: ساخت دیتابیس PostgreSQL
۱. به [neon.tech](https://neon.tech) برو و ثبت‌نام کن
۲. یک پروژه جدید بساز
۳. Connection String رو کپی کن و در `DATABASE_URL` در Vercel بذار
۴. اسکیما رو push کن:
```bash
DATABASE_URL="postgresql://..." bunx prisma db push
```

### قدم ۴ (اختیاری): ورود با گیت‌هاب
۱. به [github.com/settings/developers](https://github.com/settings/developers) برو
۲. **New OAuth App** رو بزن
۳. پر کن:
   - **Homepage URL:** آدرس Vercel
   - **Authorization callback URL:** `https://آدرس-Vercel/api/auth/callback/github`
۴. Client ID و Secret رو بگیر و در Vercel Environment Variables بذار
۵. `NEXT_PUBLIC_GITHUB_ENABLED` رو `true` کن و Redeploy کن

### قدم ۵: تنظیم ادمین و ایمیل

**ادمین:** ایمیل ادمین به‌صورت پیش‌فرض `danielzamani7089@gmail.com` است. برای تغییر:
```
ADMIN_EMAIL="ایمیل-ادمین-شما@gmail.com"
```
وقتی کاربری با این ایمیل ثبت‌نام کنه، خودکار نقش `admin` می‌گیره و بدون نیاز به تأیید ایمیل وارد می‌شه. در منوی پروفایل، گزینه «پنل ادمین» براش ظاهر می‌شه.

**ایمیل تأیید:** در محیط توسعه (بدون `RESEND_API_KEY`)، کد ۶ رقمی تأیید توی لاگ سرور و toast نمایش داده می‌شه. برای ارسال ایمیل واقعی در تولید:
۱. به [resend.com](https://resend.com) برو و ثبت‌نام کن
۲. یک API Key بساز
۳. در Vercel Environment Variables اضافه کن:
```
RESEND_API_KEY="re_xxxxxxxx"
MAIL_FROM="خوارزمی یار <noreply@yourdomain.com>"
```

## 🔐 امنیت

- ✅ رمزها با bcrypt (۱۲ راند) هش می‌شن
- ✅ اعتبارسنجی ورودی با zod
- ✅ نشست‌های JWT با NEXTAUTH_SECRET
- ✅ `.env` از گیت نادیده گرفته می‌شه
- ✅ **تأیید ایمیل** برای کاربران عادی (ادمین معاف است)
- ✅ **محدودسازی نرخ** برای API تولید ایده (۲۰/ساعت)
- ✅ **فیلتر محتوای نامناسب** روی ورودی و خروجی AI
- ✅ پنل ادمین محافظت‌شده با نقش `admin`

### قبل از انتشار:
- [ ] `NEXTAUTH_SECRET` رو با رشته تصادفی عوض کن
- [ ] `DATABASE_URL` رو به PostgreSQL منتقل کن
- [ ] `.env` واقعی رو هرگز commit نکن

## 📜 اسکریپت‌ها

```bash
bun run dev        # اجرای سرور توسعه
bun run build      # بیلد تولید
bun run start      # اجرای نسخه تولید
bun run lint       # بررسی کیفیت کد
bun run db:push    # اعمال اسکیما روی دیتابیس
bun run db:generate # تولید Prisma Client
```

## 📞 پشتیبانی

اگه سوالی داری یا می‌خوای قابلیت جدید اضافه کنی، issue باز کن.

---

ساخته‌شده با ❤️ برای نوجوانان ایران
