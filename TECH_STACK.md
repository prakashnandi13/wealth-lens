# Tech Stack Reference Guide

## 📱 Core Android Stack

### Language & Framework
- **Kotlin** - Primary programming language
- **Jetpack Compose** - Modern UI toolkit (declarative UI)
- **Android SDK** - Minimum API 24 (Android 7.0), Target API 34+

### Architecture
- **MVVM (Model-View-ViewModel)** - Architecture pattern
- **Clean Architecture** - Separation of concerns (data/domain/presentation layers)
- **Repository Pattern** - Data abstraction layer

### Dependency Injection
- **Hilt** - Dependency injection framework (built on Dagger)

---

## 💾 Data Layer

### Database
- **Room Database** - Local SQLite database wrapper
  - Entities: Transaction, Category, Budget, Receipt, SpendingPattern
  - DAOs: Data Access Objects for queries
  - Migrations: Database version management

### Preferences
- **DataStore** - Modern replacement for SharedPreferences
  - Type-safe preferences storage
  - Coroutines support

### File Storage
- **Internal Storage** - For receipt images
- **External Storage** - Optional backup files

---

## 🤖 AI/ML Components

### OCR (Receipt Scanning)
- **ML Kit Text Recognition** (On-device)
  - Free, no API costs
  - Works offline
  - Good accuracy for printed text

### SMS/Email Parsing
- **Custom Regex Patterns** - Primary method
  - Pattern matching for amounts, merchants, dates
  - No external dependencies
- **String Processing** - NLP-like text extraction
  - Custom algorithms for Indian banking formats

### Machine Learning (Optional - Advanced)
- **TensorFlow Lite** - On-device ML models
  - Merchant categorization
  - Spending pattern recognition
  - Can train models on your computer, deploy to app

### Statistical Analysis
- **Custom Algorithms** - No ML framework needed
  - Moving averages
  - Standard deviation calculations
  - Anomaly detection (statistical methods)
  - Clustering (simple algorithms)

---

## 🎨 UI Components

### Navigation
- **Navigation Component** - Screen navigation
- **Bottom Navigation** - Main app navigation

### Charts & Visualization
- **MPAndroidChart** or **Compose Chart Libraries**
  - Expense breakdown charts
  - Trend visualization
  - Budget progress indicators

### Image Handling
- **Coil** - Image loading library (for Compose)
  - Receipt image display
  - Category icons

### Date/Time
- **Kotlinx DateTime** - Date/time handling
  - Modern date API
  - Timezone support

---

## 📷 Camera & Media

### Camera
- **CameraX** - Modern camera library
  - Easy integration
  - Handles permissions
  - Image capture and preview

### Image Processing
- **Android Bitmap APIs** - Basic image manipulation
- **ML Kit** - OCR processing

---

## ⚙️ Background Processing

### Background Tasks
- **WorkManager** - Background job scheduling
  - Periodic SMS scanning
  - Data synchronization (if needed)
  - Battery-efficient

### Async Operations
- **Kotlin Coroutines** - Asynchronous programming
- **Flow** - Reactive data streams
- **StateFlow** - State management

---

## 🌐 Networking (Optional - Not Used Initially)

### If You Need Cloud Features Later
- **Retrofit** - HTTP client
- **OkHttp** - HTTP client (used by Retrofit)
- **Kotlinx Serialization** - JSON parsing

**Note**: For $0 cost approach, avoid cloud services initially.

---

## 📦 Key Dependencies (Gradle)

```kotlin
// Core Android
implementation("androidx.core:core-ktx:1.12.0")
implementation("androidx.lifecycle:lifecycle-runtime-ktx:2.7.0")

// Compose
implementation("androidx.activity:activity-compose:1.8.2")
implementation(platform("androidx.compose:compose-bom:2024.02.00"))
implementation("androidx.compose.ui:ui")
implementation("androidx.compose.ui:ui-graphics")
implementation("androidx.compose.ui:ui-tooling-preview")
implementation("androidx.compose.material3:material3")

// Navigation
implementation("androidx.navigation:navigation-compose:2.7.6")

// Room Database
implementation("androidx.room:room-runtime:2.6.1")
implementation("androidx.room:room-ktx:2.6.1")
kapt("androidx.room:room-compiler:2.6.1")

// Hilt (DI)
implementation("com.google.dagger:hilt-android:2.48")
kapt("com.google.dagger:hilt-compiler:2.48")
implementation("androidx.hilt:hilt-navigation-compose:1.1.0")

// DataStore
implementation("androidx.datastore:datastore-preferences:1.0.0")

// Coroutines
implementation("org.jetbrains.kotlinx:kotlinx-coroutines-android:1.7.3")

// ML Kit (OCR)
implementation("com.google.mlkit:text-recognition:16.0.0")

// CameraX
implementation("androidx.camera:camera-camera2:1.3.1")
implementation("androidx.camera:camera-lifecycle:1.3.1")
implementation("androidx.camera:camera-view:1.3.1")

// WorkManager
implementation("androidx.work:work-runtime-ktx:2.9.0")

// Image Loading
implementation("io.coil-kt:coil-compose:2.5.0")

// Date/Time
implementation("org.jetbrains.kotlinx:kotlinx-datetime:0.5.0")
```

---

## 🏗️ Project Structure

```
app/
├── data/
│   ├── local/
│   │   ├── database/          # Room database
│   │   ├── dao/               # Data Access Objects
│   │   └── entities/          # Database entities
│   ├── repository/            # Repository implementations
│   └── models/                # Data models
├── domain/
│   ├── usecase/               # Business logic use cases
│   └── models/                # Domain models
├── presentation/
│   ├── ui/
│   │   ├── screens/           # Compose screens
│   │   ├── components/        # Reusable UI components
│   │   └── theme/             # App theme
│   └── viewmodel/             # ViewModels
├── ai/
│   ├── sms/                   # SMS parsing logic
│   ├── ocr/                   # Receipt OCR
│   ├── analysis/              # Behavioral analysis
│   └── prediction/            # Budget predictions
└── util/                      # Utility classes
```

---

## 🔒 Permissions Required

### AndroidManifest.xml
```xml
<!-- SMS Reading -->
<uses-permission android:name="android.permission.READ_SMS" />
<uses-permission android:name="android.permission.RECEIVE_SMS" />

<!-- Camera -->
<uses-permission android:name="android.permission.CAMERA" />

<!-- Storage (for receipts) -->
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" 
    android:maxSdkVersion="32" />

<!-- Internet (optional, for future cloud features) -->
<uses-permission android:name="android.permission.INTERNET" />
```

---

## 📊 Database Schema Overview

### Transaction Table
- id (Primary Key)
- amount (Double)
- merchant (String)
- date (Long - timestamp)
- categoryId (Foreign Key)
- type (Enum: INCOME/EXPENSE)
- notes (String?)
- createdAt (Long)

### Category Table
- id (Primary Key)
- name (String)
- icon (String)
- color (Int)
- isDefault (Boolean)

### Budget Table
- id (Primary Key)
- categoryId (Foreign Key)
- amount (Double)
- period (Enum: MONTHLY/WEEKLY)
- startDate (Long)

### Receipt Table
- id (Primary Key)
- transactionId (Foreign Key)
- imagePath (String)
- extractedText (String?)
- extractedAmount (Double?)

### SpendingPattern Table
- id (Primary Key)
- patternType (String)
- description (String)
- frequency (Int)
- detectedDate (Long)

---

## 🎯 Development Priorities

### Must Have (MVP)
1. ✅ Transaction CRUD operations
2. ✅ Category management
3. ✅ Basic dashboard
4. ✅ SMS parsing (basic)
5. ✅ Receipt scanning (basic OCR)

### Should Have
1. ✅ Behavioral analysis
2. ✅ Budget predictions
3. ✅ Financial coach chat

### Nice to Have
1. ⚠️ Advanced ML models
2. ⚠️ Email parsing
3. ⚠️ Cloud backup (adds cost)

---

## 💡 Cost Optimization Tips

1. **Use On-Device ML**: ML Kit works offline, no API costs
2. **Local Processing**: All parsing done on-device
3. **No Cloud Services**: Avoid Firebase, AWS, etc. initially
4. **Simple Algorithms**: Use statistics over complex ML when possible
5. **Efficient Storage**: Compress receipt images, optimize database

---

## 🚀 Getting Started Checklist

- [ ] Install Android Studio
- [ ] Create new Android project
- [ ] Set minimum SDK to 24
- [ ] Enable Kotlin
- [ ] Add Compose support
- [ ] Set up project structure
- [ ] Add core dependencies (Room, Hilt, etc.)
- [ ] Set up database schema
- [ ] Create basic navigation

---

**This tech stack ensures $0 cost while providing all required features!**
