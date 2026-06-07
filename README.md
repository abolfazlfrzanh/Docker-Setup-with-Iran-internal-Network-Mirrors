# راهنمای جامع نصب داکر (Docker) با میرورهای داخلی

## پیش‌نیازها
- **سیستم‌عامل:** Ubuntu (یا توزیع‌های مشابه مبتنی بر Debian)
- **دسترسی:** کاربر با دسترسی `sudo`

> **توجه بسیار مهم:** در تمام دستورات زیر، عبارت `jammy` را با **Codename** نسخه سیستم‌عامل خودتون جایگزین کنید.
> اگر نمی‌دونید کد نیم سیستمتون چیه، از جدول انتهای همین صفحه تقلب کنید!

---

## مرحله ۱ — تنظیم میرور داخلی (Apt Mirrors)

### 🔵 ران‌فلیر (Runflare)
```bash
sudo tee /etc/apt/sources.list > /dev/null <<EOF
deb http://mirror-linux.runflare.com/ubuntu jammy main restricted universe multiverse
deb http://mirror-linux.runflare.com/ubuntu jammy-updates main restricted universe multiverse
deb http://mirror-linux.runflare.com/ubuntu jammy-backports main restricted universe multiverse
deb http://mirror-linux.runflare.com/ubuntu jammy-security main restricted universe multiverse
EOF

### 🟣 لیارا (Liara)
bash
sudo tee /etc/apt/sources.list > /dev/null <<EOF
deb http://linux-mirror.liara.ir/repository/ubuntu jammy main restricted universe multiverse
deb http://linux-mirror.liara.ir/repository/ubuntu jammy-updates main restricted universe multiverse
deb http://linux-mirror.liara.ir/repository/ubuntu jammy-backports main restricted universe multiverse
deb http://linux-mirror.liara.ir/repository/ubuntu-security jammy-security main restricted universe multiverse
EOF

### 🟠 آروان‌کلاد (ArvanCloud)
bash
sudo tee /etc/apt/sources.list > /dev/null <<EOF
deb http://mirror.arvancloud.ir/ubuntu jammy main restricted universe multiverse
deb http://mirror.arvancloud.ir/ubuntu jammy-updates main restricted universe multiverse
deb http://mirror.arvancloud.ir/ubuntu jammy-backports main restricted universe multiverse
deb http://mirror.arvancloud.ir/ubuntu jammy-security main restricted universe multiverse
EOF

---

## مرحله ۲ — دور زدن موقت خطاهای Security (اختیاری)


بعضی وقت‌ها میرورهای داخلی تو سینک کردن مخازن امنیتی (Security) بازی درمیارن و ارور می‌دن. اگه موقع آپدیت به مشکل خوردید، فایل لیست مخازن رو باز کنید:

bash
sudo nano /etc/apt/sources.list

و خط مربوط به `security` رو با گذاشتن یک `#` در ابتدای اون کامنت کنید (مثل نمونه زیر):


bash
# deb http://... jammy-security main restricted universe multiverse

---

## مرحله ۳ — آپدیت و نصب پیش‌نیازها

ابزارهای لازم رو نصب کنید:
bash
sudo apt update
sudo apt upgrade -y
sudo apt install -y apt-transport-https ca-certificates curl software-properties-common

---

## مرحله ۴ — نصب داکر

### ۱. اضافه کردن کلید GPG (مخزن داخلی)
bash
curl -fsSL https://archive.ito.gov.ir/docker-ce/linux/ubuntu/gpg | \
  sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

### ۲. اضافه کردن مخزن داکر به سورس‌لیست


bash
echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://archive.ito.gov.ir/docker-ce/linux/ubuntu jammy stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null



### ۳. نصب خودِ داکر
bash
sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io
sudo systemctl restart docker

### ۴. تست نصب

bash
sudo docker run hello-world

---

## مرحله ۵ — تنظیم Docker Image Registry

حالا که داکر نصب شد، برای اینکه بتونید Imageها رو بدون مشکل تحریم و قطعی دانلود کنید، باید رجیستری رو هم روی میرورهای داخلی تنظیم کنید. برای این کار پیشنهاد می‌کنم از مستندات آروان‌کلاد استفاده کنید:

📖 [راهنمای تنظیم Docker Registry در آروان‌کلاد](https://www.arvancloud.ir/fa/dev/docker)

---

## 📋 تقلب‌نامه Codename های اوبونتو

اگه اسم رمز (Codename) نسخه اوبونتوی خودتون رو یادتون رفته، این جدول برای شماست:

| نسخه Ubuntu | Codename |
|-------------|----------|
| 20.04 LTS   | `focal`  |
| 22.04 LTS   | `jammy`  |
| 24.04 LTS   | `noble`  |

> **نکته:** برای پیدا کردن کد نیم سیستم خودتون به صورت خودکار و بی‌دردسر، می‌تونید دستور `lsb_release -cs` رو تو ترمینال اجرا کنید.
`
