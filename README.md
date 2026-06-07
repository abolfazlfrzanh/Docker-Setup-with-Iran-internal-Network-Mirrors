
# راهنمای نصب داکر با میرورهای داخلی

## پیش‌نیاز
سیستم‌عامل: Ubuntu (یا توزیع‌های مشابه Debian-based)

> ⚠️ در تمام دستورات زیر، `jammy` را با **codename** نسخه سیستم‌عاملتان جایگزین کنید.
> (مثلاً `focal` برای Ubuntu 20.04 یا `noble` برای Ubuntu 24.04)

---

## مرحله ۱ — تنظیم میرور داخلی

یکی از میرورهای زیر را انتخاب کنید:

### 🔵 Runflare

sudo tee /etc/apt/sources.list > /dev/null <<EOF
deb http://mirror-linux.runflare.com/ubuntu jammy main restricted universe multiverse
deb http://mirror-linux.runflare.com/ubuntu jammy-updates main restricted universe multiverse
deb http://mirror-linux.runflare.com/ubuntu jammy-backports main restricted universe multiverse
deb http://mirror-linux.runflare.com/ubuntu jammy-security main restricted universe multiverse
EOF

### 🟣 Liara

sudo tee /etc/apt/sources.list > /dev/null <<EOF
deb http://linux-mirror.liara.ir/repository/ubuntu jammy main restricted universe multiverse
deb http://linux-mirror.liara.ir/repository/ubuntu jammy-updates main restricted universe multiverse
deb http://linux-mirror.liara.ir/repository/ubuntu jammy-backports main restricted universe multiverse
deb http://linux-mirror.liara.ir/repository/ubuntu-security jammy-security main restricted universe multiverse
EOF

### 🟠 ArvanCloud

sudo tee /etc/apt/sources.list > /dev/null <<EOF
deb http://mirror.arvancloud.ir/ubuntu jammy main restricted universe multiverse
deb http://mirror.arvancloud.ir/ubuntu jammy-updates main restricted universe multiverse
deb http://mirror.arvancloud.ir/ubuntu jammy-backports main restricted universe multiverse
deb http://mirror.arvancloud.ir/ubuntu jammy-security main restricted universe multiverse
EOF

---

## مرحله ۲ — کامنت کردن خط security

فایل را باز کنید:

nano /etc/apt/sources.list

خط مربوط به security را کامنت کنید:

#deb http://linux-mirror.liara.ir/repository/ubuntu-security jammy-security main restricted universe multiverse

---

## مرحله ۳ — بروزرسانی و نصب پیش‌نیازها


sudo apt update
sudo apt upgrade -y
sudo apt install -y apt-transport-https ca-certificates curl software-properties-common

---

## مرحله ۴ — نصب داکر

### اضافه کردن کلید GPG (مخزن وزارت ارتباطات)

curl -fsSL https://archive.ito.gov.ir/docker-ce/linux/ubuntu/gpg | \
  sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

### اضافه کردن مخزن داکر

echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] \https://archive.ito.gov.ir/docker-ce/linux/ubuntu jammy stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

### نصب داکر

sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io
sudo systemctl restart docker

### تست نصب

sudo docker run hello-world

---

## مرحله ۵ — تنظیم Docker Image Registry

برای اضافه کردن Image Registry داخلی، به مستندات ArvanCloud مراجعه کنید:

📖 [راهنمای Docker Registry در ArvanCloud](https://www.arvancloud.ir/fa/dev/docker)

---

## 📋 خلاصه codename ها

| نسخه Ubuntu | Codename |
|-------------|----------|
| 20.04 LTS   | focal    |
| 22.04 LTS   | jammy    |
| 24.04 LTS   | noble    |
