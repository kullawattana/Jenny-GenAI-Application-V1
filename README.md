# Jenny Kiosk — GenAI Application

> Flutter-based AI-powered kiosk application for the DEPA Generative AI Hackathon
> Retail self-service platform integrated with AWS Bedrock for intelligent product recommendations

---

## สารบัญ / Table of Contents

- [ภาพรวมโปรเจค](#ภาพรวมโปรเจค)
- [Tech Stack & Libraries](#tech-stack--libraries)
- [โครงสร้างโปรเจค](#โครงสร้างโปรเจค)
- [Architecture Overview](#architecture-overview)
- [Flowchart การทำงาน](#flowchart-การทำงาน)
- [API & Backend](#api--backend)
- [Data Models](#data-models)
- [State Management](#state-management)
- [การตั้งค่าและรันโปรเจค](#การตั้งค่าและรันโปรเจค)

---

## ภาพรวมโปรเจค

**Jenny Kiosk** is a self-service kiosk application for Jaymart retail stores, built with Flutter and supported across iOS, Android, Web, macOS, Linux, and Windows. Users can:

- Search and browse products powered by AI (AWS Bedrock)
- Chat with AI Assistant "Jayne" for personalized product recommendations
- View promotions, verify identity, and manage their shopping cart
- Switch between 2 languages: Thai / English

---

## Tech Stack & Libraries

### Framework & Language

| รายการ | รายละเอียด |
|--------|-----------|
| Framework | Flutter |
| Language | Dart ^3.5.0 |
| Platforms | iOS, Android, Web, macOS, Linux, Windows |

### State Management & DI

| Library | Version | การใช้งาน |
|---------|---------|-----------|
| `flutter_bloc` | 8.1.6 | BLoC pattern สำหรับ state management หลัก |
| `bloc` | 8.1.4 | Core BLoC library |
| `provider` | 6.1.2 | Provider pattern สำหรับ service injection |
| `get_it` | 7.2.0 | Service Locator / Dependency Injection |

### Navigation

| Library | Version | การใช้งาน |
|---------|---------|-----------|
| `go_router` | 12.1.3 | Declarative routing และ deep linking |

### UI & Animations

| Library | Version | การใช้งาน |
|---------|---------|-----------|
| `flutter_chat_ui` | 1.6.15 | Chat interface components |
| `carousel_slider` | 5.0.0 | Banner carousel บน Home |
| `lottie` | 3.1.2 | Lottie animation |
| `auto_size_text` | 3.0.0 | Responsive text sizing |
| `flutter_svg` | 2.0.10 | SVG rendering |

### Networking & Cloud

| Library | Version | การใช้งาน |
|---------|---------|-----------|
| `http` | 1.2.2 | HTTP client สำหรับ REST API |
| `aws_signature_v4` | 0.6.3 | AWS Signature V4 authentication |
| `aws_common` | 0.7.3 | AWS utilities |

### Authentication

| Library | Version | การใช้งาน |
|---------|---------|-----------|
| `google_sign_in` | 6.2.1 | Google OAuth login |
| `flutter_facebook_auth` | 6.0.4 | Facebook login |

### Device Features

| Library | Version | การใช้งาน |
|---------|---------|-----------|
| `flutter_tts` | 4.0.2 | Text-to-Speech |
| `image_picker` | ^1.1.2 | Image selection จาก device |
| `qr_flutter` | 4.1.0 | QR Code generation |
| `connectivity_plus` | 6.0.5 | Network connectivity detection |

### Storage & Security

| Library | Version | การใช้งาน |
|---------|---------|-----------|
| `flutter_secure_storage` | 4.2.1 | Encrypted local storage |
| `shared_preferences` | 2.3.2 | Key-value lightweight storage |
| `flutter_dotenv` | 5.0.2 | Environment variables (.env) |

### Localization

| Library | Version | การใช้งาน |
|---------|---------|-----------|
| `easy_localization` | 3.0.7 | Multi-language support (EN/TH) |
| `easy_localization_loader` | 2.0.2 | Load localization assets |
| `intl` | 0.19.0 | Date/Number formatting |

### Utilities

| Library | Version | การใช้งาน |
|---------|---------|-----------|
| `uuid` | 4.5.1 | UUID generation สำหรับ message ID |
| `url_launcher` | 6.3.1 | เปิด URL ภายนอก |
| `equatable` | 2.0.5 | Value equality comparison |
| `awesome_notifications` | 0.9.3+1 | Push notifications |

---

## โครงสร้างโปรเจค

```
Jenny-GenAI-Application-V1/
├── kiosk/                              # Flutter Application Root
│   ├── lib/
│   │   └── jayne/
│   │       ├── main.dart               # Entry Point
│   │       ├── jayne_getit_dependencies.dart  # Dependency Injection Setup
│   │       │
│   │       ├── blocs/                  # State Management (BLoC/Cubit)
│   │       │   ├── application_bloc/
│   │       │   │   ├── application_cubit.dart   # Global app state
│   │       │   │   └── application_state.dart
│   │       │   └── product_bloc/
│   │       │       ├── product_cubit.dart        # Product & cart state
│   │       │       └── product_state.dart
│   │       │
│   │       ├── view/                   # Screen Pages
│   │       │   ├── kiosk/              # Kiosk Mode Screens
│   │       │   │   ├── welcome_kiosk_page.dart
│   │       │   │   ├── home_page.dart
│   │       │   │   ├── product_search_page.dart
│   │       │   │   ├── product_information_page.dart
│   │       │   │   ├── promotion_page.dart
│   │       │   │   ├── verification_success_page.dart
│   │       │   │   ├── shopping_cart_page.dart
│   │       │   │   ├── ai_assistant_page.dart
│   │       │   │   └── thank_you_page.dart
│   │       │   └── chatbot/            # Chatbot Mode Screens
│   │       │       ├── splash_screen_page.dart
│   │       │       ├── welcome_chatbot_page.dart
│   │       │       ├── login_page.dart
│   │       │       ├── register_page.dart
│   │       │       ├── chat_instruction_page.dart
│   │       │       └── chat_page.dart
│   │       │
│   │       ├── components/             # UI Components (Atomic Design)
│   │       │   ├── atoms/              # Basic elements
│   │       │   │   ├── button.dart
│   │       │   │   ├── base_button.dart
│   │       │   │   ├── primary_button.dart
│   │       │   │   ├── floating_button.dart
│   │       │   │   ├── message_time.dart
│   │       │   │   ├── typing_animation.dart
│   │       │   │   └── widget_spacer.dart
│   │       │   ├── molecules/          # Atom combinations
│   │       │   │   ├── navigation/
│   │       │   │   │   └── app_bottom_navigation_bar.dart
│   │       │   │   ├── message/
│   │       │   │   │   ├── send_message.dart
│   │       │   │   │   ├── receive_message_with_profile_icon.dart
│   │       │   │   │   ├── receive_message_with_time.dart
│   │       │   │   │   └── send_message_box.dart
│   │       │   │   └── events/
│   │       │   │       └── avatar_circle.dart
│   │       │   └── organisms/          # Complex sections
│   │       │       ├── chat_panel_body.dart
│   │       │       └── loader_organism.dart
│   │       │
│   │       ├── repository/             # Data & API Layer
│   │       │   └── service_provider.dart  # REST API calls
│   │       │
│   │       ├── model/                  # Data Models
│   │       │   ├── request/
│   │       │   │   ├── bot_request.dart
│   │       │   │   └── summary_request.dart
│   │       │   └── response/
│   │       │       └── bot_response.dart
│   │       │
│   │       ├── controllers/            # Stream Controllers
│   │       │   ├── send_message_stream.dart
│   │       │   └── notification.dart
│   │       │
│   │       ├── router/                 # Navigation
│   │       │   ├── router_page.dart
│   │       │   └── routes_name.dart
│   │       │
│   │       ├── providers/              # State Providers
│   │       │   └── chatbot_scroll_page_provider.dart
│   │       │
│   │       ├── enhances/               # Utility Enhancements
│   │       │   ├── responsive_text.dart
│   │       │   ├── responsive_image.dart
│   │       │   ├── responsive_element_functions.dart
│   │       │   ├── condition.dart
│   │       │   └── focus_cleaner.dart
│   │       │
│   │       ├── common/                 # Shared Constants & Styles
│   │       │   ├── app_styles.dart
│   │       │   ├── theme_color.dart
│   │       │   ├── default_color.dart
│   │       │   ├── action.dart
│   │       │   └── message_type.dart
│   │       │
│   │       ├── layouts/                # Layout Wrappers
│   │       │   ├── jayne_scaffold_layout.dart
│   │       │   ├── popup_layout.dart
│   │       │   ├── popup_container.dart
│   │       │   ├── background_gradient.dart
│   │       │   ├── image_filter_blur.dart
│   │       │   └── widget_size.dart
│   │       │
│   │       ├── language/               # Localization
│   │       │   ├── language_handler.dart
│   │       │   └── th_custom_intl.dart
│   │       │
│   │       ├── secure_storage/         # Encryption & Storage
│   │       │   └── secure_storage_service.dart
│   │       │
│   │       └── utils/                  # Utility Functions
│   │           ├── condition_functions.dart
│   │           └── user_data.dart
│   │
│   ├── assets/
│   │   ├── images/                     # Image & Icon Assets
│   │   └── fonts/
│   │       ├── THSarabun.ttf
│   │       ├── THSarabun_Bold.ttf
│   │       └── THSarabun_Bold_Italic.ttf
│   │
│   ├── translations/                   # i18n Files
│   │   ├── en.json                     # English
│   │   ├── th.json                     # Thai
│   │   └── metadata.json
│   │
│   ├── test/
│   │   └── widget_test.dart
│   │
│   ├── pubspec.yaml                    # Dependencies
│   ├── .env                            # Production Environment
│   └── env-dev                         # Development Environment
│
└── README.md
```

---

## Architecture Overview

```
┌─────────────────────────────────────────────────────────────────────┐
│                         PRESENTATION LAYER                          │
│                                                                     │
│   ┌──────────────────────────────────────────────────────────────┐  │
│   │                    VIEW (Screens / Pages)                    │  │
│   │  WelcomePage → HomePage → ProductSearch / AIAssistant / ...  │  │
│   └──────────────────────────────────────────────────────────────┘  │
│                               ▲ │                                   │
│                         read  │ │ emit events                       │
│                               │ ▼                                   │
│   ┌──────────────────────────────────────────────────────────────┐  │
│   │              STATE MANAGEMENT (BLoC / Cubit)                 │  │
│   │          ApplicationCubit          ProductCubit              │  │
│   │  (Screen size, language, loading)  (Products, cart, filters) │  │
│   └──────────────────────────────────────────────────────────────┘  │
│                               ▲ │                                   │
│                      request  │ │ response                          │
│                               │ ▼                                   │
│   ┌──────────────────────────────────────────────────────────────┐  │
│   │                  REPOSITORY LAYER                            │  │
│   │                   ServiceProvider                            │  │
│   │           (HTTP calls → REST API /chat)                      │  │
│   └──────────────────────────────────────────────────────────────┘  │
└────────────────────────────────────────┬────────────────────────────┘
                                         │ HTTP POST
                                         ▼
┌─────────────────────────────────────────────────────────────────────┐
│                          BACKEND SERVER                             │
│               http://184.72.103.175:5000/chat                       │
│                                                                     │
│   ┌─────────────────────────────────────────────────────────────┐   │
│   │                     Python Backend                          │   │
│   │              (LLM Orchestration Layer)                      │   │
│   └───────────────────────────┬─────────────────────────────────┘   │
│                               │                                     │
│                               ▼                                     │
│   ┌─────────────────────────────────────────────────────────────┐   │
│   │                    AWS Bedrock                               │   │
│   │         (Knowledge Base + LLM Model)                        │   │
│   │   - Product catalog embeddings                              │   │
│   │   - Semantic search & recommendations                       │   │
│   └─────────────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────────────┘
```

### Layer Responsibilities

| Layer | ความรับผิดชอบ |
|-------|--------------|
| **View** | แสดง UI, รับ input จากผู้ใช้, ฟัง state changes |
| **BLoC/Cubit** | จัดการ business logic, state transitions, orchestrate data flow |
| **Repository** | HTTP calls, serialize/deserialize JSON, error handling |
| **Backend** | LLM orchestration, prompt engineering, AWS Bedrock integration |
| **AWS Bedrock** | Vector search บน product catalog, LLM inference |

---

## Flowchart การทำงาน

### 1. Application Startup Flow

```mermaid
flowchart TD
    A([App Start]) --> B[main.dart]
    B --> C[dotenv.load\nLoad .env variables]
    C --> D[JayneGetItDependencies\nRegister services via GetIt]
    D --> E[MultiProvider Setup]
    E --> F[ServiceProvider\nAPI Service]
    E --> G[ChatBotScrollPageProvider\nScroll State]
    F & G --> H[JayneRootApp]
    H --> I[SecureStorageService\nInit encrypted storage]
    I --> J[JayneBlocInjector\nInject ApplicationCubit\n& ProductCubit]
    J --> K[JayneLocalization\nLoad EN/TH translations]
    K --> L[GoRouter Init\nSetup routes]
    L --> M([WelcomeKioskPage\nFirst Screen])
```

---

### 2. Main Navigation Flow (Kiosk Mode)

```mermaid
flowchart TD
    A([Welcome Kiosk Page]) --> B[Home Page]
    B --> C{ผู้ใช้เลือก}
    C --> D[Product Search]
    C --> E[Promotions]
    C --> F[Verification]
    C --> G[AI Assistant\nJayne Chat]

    D --> D1[Filter: Brand/Usage/Price]
    D1 --> D2[ProductCubit\ngetProductOnAWSBedrock]
    D2 --> D3[Product List]
    D3 --> D4[Product Information Page]
    D4 --> D5[Add to Cart]
    D5 --> D6[Shopping Cart Page]
    D6 --> D7[Thank You Page]

    G --> G1[Chat Interface\nflutter_chat_ui]
    G1 --> G2[User types message]
    G2 --> G3[ServiceProvider\nrequestProduct]
    G3 --> G4[Backend API /chat]
    G4 --> G5[AWS Bedrock\nLLM Response]
    G5 --> G6[BotResponse parsed]
    G6 --> G7[SendMessageStream\nupdate UI]
    G7 --> G1
```

---

### 3. AI Chat Message Flow

```mermaid
sequenceDiagram
    participant U as User (UI)
    participant AI as AIAssistantPage
    participant SP as ServiceProvider
    participant BE as Backend Server
    participant AWS as AWS Bedrock

    U->>AI: พิมพ์ข้อความ
    AI->>AI: _handleSendPressed()
    AI->>AI: สร้าง TextMessage\n(UUID, timestamp)
    AI->>AI: _addMessage() → แสดงข้อความ user

    AI->>SP: requestProduct(BotRequest)
    Note over SP: inputText, chatHistory,\nminPrice, maxPrice,\ndiscount, point filters

    SP->>BE: POST /chat (JSON body)
    BE->>AWS: Query Knowledge Base\n(semantic search)
    AWS-->>BE: Retrieved product references
    BE->>AWS: LLM Inference\n(generate response)
    AWS-->>BE: Generated text response
    BE-->>SP: BotResponse JSON

    SP->>SP: Parse JSON → BotResponse
    SP-->>AI: return BotResponse

    AI->>AI: สร้าง bot TextMessage
    AI->>AI: _addMessage() → แสดงข้อความ bot
    AI->>U: UI อัปเดตแสดงคำตอบ
```

---

### 4. Product Search Flow

```mermaid
flowchart TD
    A([Product Search Page]) --> B{เลือก Category Tab}
    B --> |Recommend/Mobile/\nTablet/Notebook/...| C[ProductCubit\ngetProductOnAWSBedrock]

    C --> D[Build BotRequest]
    D --> D1[inputText: category name]
    D --> D2[minPrice / maxPrice]
    D --> D3[minDiscountPc/Value]
    D --> D4[minPoint]
    D1 & D2 & D3 & D4 --> E[ServiceProvider\nPOST /chat]

    E --> F{Response OK?}
    F -->|Success| G[Parse BotResponse]
    G --> H[retrievedReferences\nList of Products]
    H --> I[ProductCubit emits\nProductState with products]
    I --> J[UI rebuilds\nProduct Grid/List]
    J --> K{User selects product}
    K --> L[Product Information Page]
    L --> M[Add to Cart\nShoppingCartInfo]
    M --> N[ProductCubit\nupdateTotalShoppingCartList]

    F -->|Error| O[Show error state]
```

---

### 5. State Management Flow

```mermaid
flowchart LR
    subgraph ApplicationCubit
        AC1[Screen Dimensions\nscreenWidth / screenHeight]
        AC2[Language / Locale\ncurrentSelectLocale]
        AC3[Loading State\ntoggleLoading / loadingKeys]
        AC4[Network Connectivity]
    end

    subgraph ProductCubit
        PC1[Product List\nfrom AWS Bedrock]
        PC2[Filter State\nbrand / usage / price]
        PC3[Shopping Cart\nshoppingCartList]
        PC4[Cart Total\ntotalPrice]
    end

    subgraph View
        V1[Home Page]
        V2[Product Search Page]
        V3[Shopping Cart Page]
        V4[AI Assistant Page]
    end

    V1 --reads--> AC1
    V1 --reads--> AC2
    V2 --reads--> PC1
    V2 --reads--> PC2
    V3 --reads--> PC3
    V3 --reads--> PC4
    V4 --reads--> AC3

    V2 --event--> PC1
    V3 --event--> PC3
    V2 --event--> PC2
    V1 --event--> AC2
```

---

### 6. Dependency Injection (GetIt) Flow

```mermaid
flowchart TD
    A([App Launch]) --> B[JayneGetItDependencies.setup]
    B --> C[Register SecureStorageLanguageService]
    B --> D[Register SecureStorageService]
    B --> E[Register ApplicationCubit]
    B --> F[Register ProductCubit]
    B --> G[Register ServiceProvider]

    C & D & E & F & G --> H[GetIt Container Ready]

    H --> I[Any Widget / BLoC\ncalls JayneGetItDependencies.of<T>]
    I --> J[Singleton instance returned]
```

---

### 7. Localization Flow

```mermaid
flowchart TD
    A([App Startup]) --> B[JayneLocalization Widget]
    B --> C[EasyLocalization.ensureInitialized]
    C --> D[Load translations/en.json\nLoad translations/th.json]
    D --> E[Default locale: TH]

    E --> F{User changes language}
    F --> |เลือก EN/TH flag icon| G[ApplicationCubit\nupdateLocale]
    G --> H[SecureStorageLanguageService\nsave locale]
    H --> I[EasyLocalization\ncontext.setLocale]
    I --> J[UI rebuilds\nwith new locale]

    E --> K[Next app launch]
    K --> L[SecureStorageLanguageService\nread saved locale]
    L --> M[Restore previous language]
```

---

## API & Backend

### Base URL
```
http://184.72.103.175:5000
```

### POST `/chat` — Product Recommendation & Chat

**Request Body:**
```json
{
  "input_text": "แนะนำมือถือราคาไม่เกิน 15000",
  "chat_history": [
    {
      "role": "user",
      "content": "สวัสดี"
    },
    {
      "role": "bot",
      "content": "สวัสดีครับ มีอะไรให้ช่วยไหม"
    }
  ],
  "min_price": 0,
  "max_price": 15000,
  "min_discount_pc": 0,
  "min_discount_value": 0,
  "min_point": 0
}
```

**Response Body:**
```json
{
  "messageType": "product",
  "output": "ขอแนะนำ iPhone 15 ราคา 29,900 บาท...",
  "retrieved_references": [
    {
      "content": "product detail text",
      "metadata": {
        "code": "SKU-001",
        "original_price": 29900.0,
        "discounted_price": 27900.0,
        "discount_%": 6.7,
        "discount_value": 2000.0,
        "point": 279.0,
        "image_0": "https://...",
        "image_1": "https://...",
        "image_2": "https://...",
        "product_url": "https://...",
        "x-amz-bedrock-kb-chunk-id": "...",
        "x-amz-bedrock-kb-data-source-id": "...",
        "x-amz-bedrock-kb-source-uri": "..."
      }
    }
  ]
}
```

---

## Data Models

### BotRequest
```dart
class BotRequest {
  String inputText;           // คำถาม/ข้อความจากผู้ใช้
  List<ChatHistory> chatHistory; // ประวัติการสนทนา
  int minPrice;               // ราคาต่ำสุด
  int maxPrice;               // ราคาสูงสุด
  int minDiscountPc;          // ส่วนลด % ขั้นต่ำ
  int minDiscountValue;       // ส่วนลดเป็นบาทขั้นต่ำ
  int minPoint;               // แต้มสะสมขั้นต่ำ
}

class ChatHistory {
  String role;    // "user" หรือ "bot"
  String content; // ข้อความ
}
```

### BotResponse
```dart
class BotResponse {
  String output;                             // ข้อความตอบกลับจาก AI
  String messageType;                        // ประเภทข้อความ
  List<RetrievedReferences> retrievedReferences; // รายการสินค้า
}

class RetrievedReferences {
  String content;
  Metadata metadata;
}

class Metadata {
  String code;
  double originalPrice, discountedPrice;
  double discountPc, discountValue, point;
  String image0, image1, image2;
  String productUrl;
}
```

### ProductInfo & ShoppingCartInfo
```dart
class ProductInfo {
  List<String> images;
  String imageUrl, brandName, color, detail;
  double price;
}

class ShoppingCartInfo {
  String imageUrl, brandName, storage, color;
  int quantity;
  double price;
}
```

---

## State Management

### ApplicationCubit — Global App State

| State Property | Type | คำอธิบาย |
|---------------|------|---------|
| `screenWidth` | double | ความกว้างหน้าจอ |
| `screenHeight` | double | ความสูงหน้าจอ |
| `currentSelectLocale` | Locale | ภาษาที่เลือก (en/th) |
| `toggleLoading` | bool | สถานะ loading overlay |
| `loadingKeys` | Map | Granular loading per feature |

### ProductCubit — Product & Cart State

| State Property | Type | คำอธิบาย |
|---------------|------|---------|
| `productList` | List<ProductInfo> | รายการสินค้าจาก API |
| `selectedBrand` | String | Brand filter |
| `selectedUsage` | String | Usage filter |
| `minPrice` / `maxPrice` | int | Price range filter |
| `shoppingCartList` | List<ShoppingCartInfo> | รายการในตะกร้า |
| `totalPrice` | double | ราคารวม |

---

## Design System

### Color Palette

| Group | Color | Hex |
|-------|-------|-----|
| Primary | Red | `#B8232F` |
| Primary | Yellow | `#FFCE00` |
| Background | White | `#FFFFFF` |
| Text | Black | `#000000` |

### Typography

| Style | Size | Weight |
|-------|------|--------|
| Header | 21px | Bold |
| Title1 | 18px | Bold |
| Title2 | 16px | SemiBold |
| Body1 | 16px | Regular |
| Caption | 14px | Regular |
| Small | 12px | Regular |

**Font Family:** THSarabunPSK (ไทย Sarabun)

### Responsive Breakpoints

| Breakpoint | Width |
|-----------|-------|
| XS | 320px |
| S | 375px |
| L | 430px |
| XL | 820px |

---

## การตั้งค่าและรันโปรเจค

### Prerequisites
- Flutter SDK >= 3.5.0
- Dart SDK >= 3.5.0
- Xcode (สำหรับ iOS/macOS)
- Android Studio (สำหรับ Android)

### Installation

```bash
# 1. Clone repository
git clone <repository-url>
cd Jenny-GenAI-Application-V1/kiosk

# 2. Install dependencies
flutter pub get

# 3. Setup environment variables
cp env-dev .env
# แก้ไขค่าใน .env ตามต้องการ

# 4. Run application
flutter run

# รันบน platform ที่ต้องการ
flutter run -d ios
flutter run -d android
flutter run -d macos
flutter run -d web
```

### Environment Variables (`.env`)

```env
IMAGE_PATH=assets/images
TRANSLATION_PATH=translations
```

### Build

```bash
# Build for iOS
flutter build ios --release

# Build for Android
flutter build apk --release
flutter build appbundle --release

# Build for Web
flutter build web --release

# Build for macOS
flutter build macos --release
```

---

## Route Map

| Route | Page | คำอธิบาย |
|-------|------|---------|
| `/` | WelcomeKioskPage | หน้าต้อนรับ |
| `/home` | HomePage | หน้าหลัก เมนูหลัก 4 รายการ |
| `/product_search` | ProductSearchPage | ค้นหาสินค้าด้วย AI |
| `/product_information` | ProductInformationPage | รายละเอียดสินค้า |
| `/promotion` | PromotionPage | หน้าโปรโมชั่น |
| `/verification_success` | VerificationSuccessPage | ยืนยันตัวตนสำเร็จ |
| `/shopping_cart` | ShoppingCartPage | ตะกร้าสินค้า |
| `/ai_assistant` | AIAssistantPage | แชทกับ Jayne AI |
| `/thank_you` | ThankYouPage | หน้าขอบคุณ |

---

## Component Architecture (Atomic Design)

```
components/
├── atoms/          # Building blocks
│   ├── Button variants (Base, Primary, Floating)
│   ├── TypingAnimation (แสดงระหว่าง AI กำลังตอบ)
│   ├── MessageTime
│   └── WidgetSpacer
│
├── molecules/      # Atom combinations
│   ├── message/    (SendMessage, ReceiveMessage)
│   ├── navigation/ (AppBottomNavigationBar)
│   └── events/     (AvatarCircle)
│
└── organisms/      # Complex UI sections
    ├── ChatPanelBody   (Full chat panel)
    └── LoaderOrganism  (Loading states)
```

---

*Built for DEPA Generative AI Hackathon — Powered by Flutter & AWS Bedrock*
