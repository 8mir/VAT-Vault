<div align="center">

<img src="assets/app-icon.png" width="110" height="110" alt="VAT Vault App Icon" />

# VAT Vault — خزنة الضريبة

**Smart VAT Invoice Manager, Vision OCR & Instant iPhone-to-Mac Scanner**  
**نظام ذكي لإدارة فواتير القيمة المضافة مع قراءة ضوئية تلقائية وربط لاسلكي مباشر بين الآيفون والماك**

[![Platform](https://img.shields.io/badge/Platform-macOS%2014%2B%20%7C%20iOS%2017%2B-0A84FF?style=for-the-badge&logo=apple&logoColor=white)](#)
[![Language](https://img.shields.io/badge/Swift-6.0%20%2F%20SwiftUI-F05138?style=for-the-badge&logo=swift&logoColor=white)](#)
[![Privacy](https://img.shields.io/badge/Privacy-100%25%20Local%20Wi--Fi%20%26%20On--Device%20OCR-30D158?style=for-the-badge&logo=shield&logoColor=white)](#)

<br />

<img src="assets/iphone-to-mac-demo.gif" width="100%" alt="VAT Vault iPhone to Mac Demo" />

</div>

---

## 🇬🇧 English Overview

**VAT Vault** is a native **macOS + iOS** companion suite built with **SwiftUI**, **Apple Vision OCR**, and **Bonjour Local Networking**. It eliminates manual receipt entry by letting you scan paper invoices directly with your iPhone camera and beam them over your local Wi-Fi network straight into your Mac's quarterly VAT register—with **zero cloud uploads** and **100% on-device privacy**.

### ✨ Key Highlights

| Feature | Description |
| :--- | :--- |
| **📱 Instant iPhone-to-Mac Sync** | Pair once via QR code (`_vatvault._tcp` Bonjour). Scan single or multi-page receipts on iPhone using `VisionKit` and send them directly to your Mac in one tap. |
| **🔍 On-Device Apple Vision OCR** | Automatically extracts the **Supplier Name**, **Invoice Date**, **Invoice Number**, and **Total Amount (BHD)** as soon as the invoice arrives. |
| **🪟 Floating Liquid Glass UI** | Designed for macOS with a floating curved Liquid Glass sidebar, smooth dark/light mode adaptation, and bilingual **Arabic / English** support (LTR & RTL). |
| **📊 Interactive Quarter Analytics** | Click any tax quarter (`Q1–Q4`) to inspect live visual analytics: a colorful spend distribution donut chart, monthly breakdown bars, and ranked supplier insights. |
| **📑 Executive PDF Reports** | Export branded A4 PDF reports featuring customizable headers, company logos, itemized invoice registers, appended receipt pages, and supplier analytics charts. |
| **💾 Flexible JSON & PDF Backups** | Export lightweight JSON backups, full JSON backups with embedded original PDFs, or standalone `.pdf` invoice archives organized by date and supplier. |

### 🚀 How iPhone-to-Mac Scanning Works (3 Simple Steps)

1. **Pair via QR Code**: On your Mac, click **Connect iPhone** (`ربط iPhone`) and start the local receiver. Scan the displayed QR code once from the **VAT Vault iPhone** app.
2. **Scan Any Receipt**: Tap **Scan Invoice** (`مسح فاتورة`) on your iPhone. The built-in document camera automatically crops, perspective-corrects, and converts the receipt into a crisp PDF.
3. **Automatic Filing & Analytics**: Within a second, the invoice appears on your Mac under **Under Review** with its supplier, date, invoice number, and VAT amount already extracted and ready for approval.

---

## 🇧🇭 نظرة عامة بالعربية

**VAT Vault (خزنة الضريبة)** هو نظام متكامل لتطبيقات **الماك والآيفون** مبني بلغة **Swift** وواجهة **SwiftUI** مع تقنيات **Apple Vision OCR** والاتصال المحلي المباشر **Bonjour**. صُمم التطبيق لإنهاء عناء إدخال الفواتير يدوياً؛ إذ يتيح لك تصوير أي فاتورة ورقية بكاميرا الآيفون وإرسالها لاسلكياً عبر شبكة الـ Wi-Fi المحلية لتصل فوراً إلى جهاز الماك، حيث تُقرأ بياناتها تلقائياً وتُصنف ضمن الربع الضريبي المناسب **بخصوصية تامة 100% على جهازك ودون رفع أي ملفات إلى السحابة**.

### ✨ أبرز المميزات

| الميزة | الوصف |
| :--- | :--- |
| **📱 إرسال فوري من الآيفون إلى الماك** | اربط الآيفون بالماك لمرة واحدة عبر مسح رمز **QR**، ثم صوّر الفاتورة ليتم تحويلها إلى PDF وإرسالها في أجزاء من الثانية عبر الشبكة المحلية. |
| **🔍 قراءة ضوئية ذكية (Vision OCR)** | استخراج تلقائي لاسم **الشركة/المورد**، **تاريخ الفاتورة**، **رقم الفاتورة**، و**المبلغ الإجمالي والضريبة** فور وصول الفاتورة. |
| **🪟 واجهة Liquid Glass عائمة وعصرية** | شريط جانبي عائم بحواف منحنية مستوحى من تصميم **Apple Liquid Glass** مع دعم كامل للغتين **العربية والإنجليزية** والوضعين الليلي والنهاري. |
| **📊 إحصائيات ورسوم بيانية تفاعلية لكل ربع** | عند الضغط على أي ربع ضريبي، تظهر في الجزء الأيمن لوحة إحصائيات بيانية ملونة تشمل حلقة توزيع الإنفاق، المقارنة الشهرية، وترتيب الشركات الأعلى إنفاقاً. |
| **📑 تقارير PDF احترافية جاهزة للطباعة** | تصدير تقارير ربع سنوية بصيغة A4 تتضمن شعار الشركة، جدول الفواتير التفصيلي، صور الفواتير المرفقة، وصفحات التحليل البياني للشركات. |
| **💾 نسخ احتياطي مرن (JSON + PDF)** | خيارات متعددة للنسخ الاحتياطي تشمل تصدير البيانات السريع (JSON)، أو تضمين ملفات الـ PDF الأصلية، أو استخراج جميع الفواتير كملفات `.pdf` مستقلة في مجلد. |

### 🛠️ خطوات التشغيل والبناء (Getting Started)

1. **تطبيق الماك (`VAT`)**:
   - افتح المشروع `VAT/VAT.xcodeproj` في برنامج **Xcode 16+**.
   - اختر هدف التشغيل **My Mac** ثم اضغط **Run (⌘R)**.
2. **تطبيق الآيفون (`VAt Iphone`)**:
   - افتح المشروع `VAt Iphone/VAt Iphone.xcodeproj` في **Xcode 16+**.
   - شغّل التطبيق على جهاز الـ iPhone المتصل بنفس شبكة الـ Wi-Fi الخاصة بالماك، وامسح رمز الـ QR من نافذة **ربط iPhone** في تطبيق الماك.

---

## 📂 Repository Structure — هيكلة المشروع

```text
├── VAT/                        # تطبيق الماك الرئيسي (macOS App - SwiftUI, Vision OCR, PDFKit, Local HTTP Receiver)
│   ├── VAT.xcodeproj
│   └── VAT/
│       ├── VATApp.swift
│       ├── ContentView.swift
│       └── Assets.xcassets/
├── VAt Iphone/                 # تطبيق الآيفون المرافق (iOS Companion App - VisionKit Document & QR Scanner)
│   ├── VAt Iphone.xcodeproj
│   └── VAt Iphone/
│       ├── VAtIphoneApp.swift
│       ├── ContentView.swift
│       ├── ScannerViews.swift
│       └── Assets.xcassets/
└── assets/                     # الوسائط التوضيحية للعرض في GitHub (GIF, Preview, Icons)
    ├── iphone-to-mac-demo.gif
    ├── workflow-preview.png
    └── app-icon.png
```
