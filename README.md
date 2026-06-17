# AC Browser

> ⚠️ **هذا مشروع Flutter لتطبيق Android حقيقي (APK)** — وليس نسخة الويب على Netlify.
> إذا كنت تبحث عن نسخة الويب (`ac_browser_netlify.zip`)، هذا ليس الملف الصحيح لذلك الاستخدام.
> هذا المستودع يُبنى تلقائياً إلى ملف `.apk` عبر GitHub Actions — راجع قسم "البناء التلقائي" أدناه.

**Fast, private and lightweight web browser with built-in ad blocking.**


> Developer: AC | Version: 1.0.0 | Platform: Android

---

## Features

- ✅ Full-featured browser with WebView (InAppWebView)
- ✅ Multiple tabs support (unlimited)
- ✅ Built-in ad blocker (EasyList + EasyPrivacy)
- ✅ Private / Incognito mode
- ✅ Bookmarks manager
- ✅ Browsing history
- ✅ Download manager
- ✅ Dark / Light / System theme
- ✅ Reading mode
- ✅ Page translator (Arabic, English, French + more)
- ✅ Built-in PDF viewer
- ✅ Screenshot tool
- ✅ Search engines: Google, Bing, DuckDuckGo
- ✅ HTTPS / Security indicator
- ✅ Safe browsing warnings
- ✅ Tracker protection
- ✅ Clean homepage with quick access sites

---

## Tech Stack

| Layer | Technology |
|---|---|
| UI | Flutter + Material 3 |
| State Management | Riverpod |
| DI | GetIt |
| Database | SQLite (sqflite) |
| Networking | Dio |
| WebView | flutter_inappwebview |
| Architecture | Clean Architecture |

---

## Project Structure

```
lib/
├── main.dart                        # App entry point
├── ac_browser.dart                  # Barrel exports
├── core/
│   ├── constants/app_constants.dart # App-wide constants
│   ├── di/injection.dart            # Dependency injection setup
│   ├── errors/failures.dart         # Error/failure types
│   ├── router/app_router.dart       # GoRouter navigation
│   ├── theme/app_theme.dart         # Material 3 themes
│   ├── utils/
│   │   ├── extensions.dart          # Dart extensions
│   │   └── url_utils.dart           # URL helpers
│   └── widgets/
│       ├── empty_state.dart         # Reusable empty state
│       └── shimmer_loading.dart     # Loading placeholder
├── database/
│   └── database_helper.dart         # SQLite helper (singleton)
├── services/
│   ├── ad_block_service.dart        # Ad blocking engine
│   ├── bookmark_service.dart        # Bookmark CRUD
│   ├── connectivity_service.dart    # Network status
│   ├── download_service.dart        # Download management
│   ├── history_service.dart         # History CRUD
│   ├── screenshot_service.dart      # Page screenshots
│   ├── security_service.dart        # URL security checks
│   ├── settings_service.dart        # Settings persistence
│   └── app_info_service.dart        # Package info
└── features/
    ├── browser/                     # Core browser feature
    │   ├── data/models/             # Data models
    │   ├── domain/
    │   │   ├── entities/            # Browser tab entity
    │   │   └── usecases/            # Business logic
    │   └── presentation/
    │       ├── providers/           # Riverpod state
    │       ├── screens/             # Browser, Reading, Translate, PDF
    │       └── widgets/             # WebView, AddressBar, Toolbar, Tabs
    ├── bookmarks/                   # Bookmarks feature
    ├── history/                     # History feature
    ├── downloads/                   # Downloads feature
    └── settings/                    # Settings feature
```

---

## البناء التلقائي عبر GitHub Actions (بدون حاسوب)

هذا المستودع يحتوي على ملف `.github/workflows/build-apk.yml` يبني APK تلقائياً عند كل رفع للكود.

**خطوات الحصول على APK:**
1. ارفع جميع ملفات هذا المشروع إلى مستودع GitHub (بما فيها مجلد `.github` المخفي)
2. اذهب لتبويب **Actions** في صفحة المستودع
3. انتظر اكتمال البناء (علامة ✅ خضراء، يستغرق 5-10 دقائق)
4. اضغط على عملية البناء المكتملة → قسم **Artifacts** → حمّل `ac-browser-apk`
5. فك ضغط الملف المحمّل وثبّت `app-release.apk` على هاتفك

---

## Setup & Installation


### Prerequisites
- Flutter SDK ≥ 3.0.0
- Android Studio / VS Code
- Android device or emulator (API 21+)

### Steps

```bash
# 1. Clone or extract the project
cd ac_browser

# 2. Install dependencies
flutter pub get

# 3. Run on Android
flutter run

# 4. Build release APK
flutter build apk --release

# 5. Build App Bundle (for Play Store)
flutter build appbundle --release
```

---

## Database Schema

### bookmarks
| Column | Type | Notes |
|---|---|---|
| id | INTEGER PK | Auto increment |
| title | TEXT | Page title |
| url | TEXT UNIQUE | Page URL |
| favicon | TEXT | Favicon URL |
| created_at | TEXT | ISO 8601 |

### history
| Column | Type | Notes |
|---|---|---|
| id | INTEGER PK | Auto increment |
| title | TEXT | Page title |
| url | TEXT | Page URL |
| visit_time | TEXT | ISO 8601 |

### downloads
| Column | Type | Notes |
|---|---|---|
| id | INTEGER PK | Auto increment |
| file_name | TEXT | File name |
| file_path | TEXT | Local path |
| url | TEXT | Source URL |
| size | INTEGER | Bytes |
| mime_type | TEXT | MIME type |
| status | TEXT | pending/downloading/completed/failed |
| progress | REAL | 0.0 - 1.0 |
| download_date | TEXT | ISO 8601 |

### settings
| Column | Type | Notes |
|---|---|---|
| id | INTEGER PK | Auto increment |
| key | TEXT UNIQUE | Setting key |
| value | TEXT | Setting value |

---

## Architecture

AC Browser follows **Clean Architecture** with three layers:

1. **Data** — SQLite datasources, data models, repository implementations
2. **Domain** — Entities, repository interfaces, use cases (pure Dart)
3. **Presentation** — Flutter widgets, Riverpod providers, screens

State flows: `UI → UseCase → Repository → DataSource → SQLite/Network`

---

## Ad Blocking

The ad blocker ships with an extensive built-in domain blocklist covering 60+ major ad networks and trackers. On first launch it attempts to fetch full EasyList and EasyPrivacy rules in the background and cache them in SQLite for offline use.

To update rules manually: **Settings → Advanced → Refresh Ad Block Rules**

---

## Privacy Mode

When Private Mode is enabled:
- No history saved
- No cookies persisted after session
- WebView incognito mode enabled
- Dark purple UI indicator
- Private mode badge in address bar

---

## Performance Tips

- Images are lazy-loaded via `cached_network_image`
- Database queries use indexed columns
- Ad block uses in-memory Set for O(1) domain lookup
- WebView instances are reused per tab (Offstage, not unmounted)

---

## Future (v2.0 — AC AI Assistant)

Architecture is prepared for:
- Summarize web pages via Claude API
- Translate articles with AI
- Extract key points
- Read aloud (TTS)
- Generate reading notes

---

## Build for Play Store

1. Generate a keystore:
```bash
keytool -genkey -v -keystore ac_browser.jks -keyAlias acbrowser \
  -keyalg RSA -keysize 2048 -validity 10000
```

2. Configure `android/key.properties`:
```
storePassword=YOUR_STORE_PASSWORD
keyPassword=YOUR_KEY_PASSWORD
keyAlias=acbrowser
storeFile=../ac_browser.jks
```

3. Build App Bundle:
```bash
flutter build appbundle --release
```

4. Upload `build/app/outputs/bundle/release/app-release.aab` to Play Console.

---

*AC Browser — Built with Flutter ❤️*
