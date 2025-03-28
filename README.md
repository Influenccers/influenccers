# Influenccers Website - Technical Documentation

## 🌍 Website Structure
```sh
root/
├── source/                    # Core assets
│   ├── assets/
│   │   ├── css/               # Stylesheets
│   │   │   ├── bootstrap.min.css
│   │   │   ├── style.css      # Main CSS (2.4KB)
│   │   │   └── 12+ plugin CSS files
│   │   ├── img/
│   │   │   ├── brand/         # Partner logos (oidict.png, varpet.svg, sooty.svg)
│   │   │   ├── icon/          # UI icons (4.png-9.png)
│   │   │   ├── illustration/  # Hero images (1.jpg-4.jpg, 1-3.png)
│   │   │   └── shape/         # SVG decor (1-18.png)
│   │   ├── js/
│   │   │   ├── main.js        # Core logic
│   │   │   └── 20+ plugins    # (swiper, wow, magnific-popup)
│   │   └── mail/              # PHP mailer
│   │       └── contact.php    # Form handler
│   └── style.css              # Override styles
│
├── *.html                     # 5 key pages
└── README.md                  # This file
```

## 🚀 Hosting Requirements

### 📦 Minimum Stack
| Component  | Requirement              | Notes                          |
|------------|--------------------------|--------------------------------|
| Web Server | Apache 2.4+/Nginx 1.18+ | Enable `mod_rewrite`           |
| PHP        | 7.4+                    | For contact forms only         |
| Storage    | 50MB                    | +5MB/month growth buffer       |

### 🔧 Recommended Config
```nginx
# Nginx Sample
server {
    listen 80;
    server_name influenccers.com www.influenccers.com;
    root /var/www/html;
    
    # Compression
    gzip on;
    gzip_types text/css application/javascript;
    
    # Cache control
    location ~* \.(jpg|jpeg|png|svg)$ {
        expires 365d;
    }
}
```

## 🛠️ Deployment Guide

### 1. Upload Files
```bash
rsync -avz --delete ./ user@server:/var/www/html/ \
--exclude='*.git*' \
--exclude='*.psd'
```

### 2. Set Permissions
```bash
# Apache
chown -R www-data:www-data /var/www/html
find /var/www/html -type d -exec chmod 755 {} \;
find /var/www/html -type f -exec chmod 644 {} \;
```

### 3. Verify Critical Paths
```bash
# Required files
ls -lah /var/www/html/{index,about-us,for_*}.html
ls -lah /var/www/html/source/assets/{css/style.js,js/main.js}
```

## 📮 Contact Form Setup

1. **Edit PHP Config** (`/source/assets/mail/contact.php`)
```php
<?php
$receiving_email = "oliver@influenccers.com";
$subject_prefix = "[Influenccers]";
```

2. **Test Email Functionality**
```bash
php -f /var/www/html/source/assets/mail/contact.php test
```

## 🔄 Maintenance Procedures

### 🖼️ Adding New Images
1. Upload to `/source/assets/img/illustration/`
2. Use relative paths:
```html
<!-- Correct -->
<img src="source/assets/img/illustration/new-image.jpg">

<!-- Avoid -->
<img src="/absolute/path/to/image.jpg">
```

### 🛑 Common Issues
| Symptom                  | Solution                          |
|--------------------------|-----------------------------------|
| Forms not submitting     | Check PHP `mail()` log at `/var/log/mail.log` |
| Broken CSS/JS            | Run `find . -name "*.css" -exec sed -i 's/\.\.\///g' {} \;` |
| Mobile menu stuck        | Verify jQuery loads before `validnavs.js` |

## 🌍 CDN Setup (Optional)
```html
<!-- Replace local paths with CDN -->
<script src="source/assets/js/jquery-3.6.0.min.js"></script>
→
<script src="https://cdn.yourcdn.com/js/jquery-3.6.0.min.js"></script>
```

## 📅 Changelog Template
```markdown
## [YYYY-MM-DD] - v1.0.1
- [Added] New influencer testimonials section
- [Fixed] Mobile menu z-index conflict (#42)
- [Security] Updated jQuery to 3.6.4
```
