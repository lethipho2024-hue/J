# 🔍 PHÂN TÍCH TOÀN DIỆN: VINA-FULL & DITLeP

> **Document tổng hợp toàn bộ thông tin về hệ thống hack Dragon City**

---

## 📋 MỤC LỤC

1. [Tổng quan hệ thống](#1-tổng-quan-hệ-thống)
2. [Vai trò từng thành phần](#2-vai-trò-từng-thành-phần)
3. [Luồng dữ liệu](#3-luồng-dữ-liệu)
4. [API Endpoints](#4-api-endpoints)
5. [Cơ chế hoạt động](#5-cơ-chế-hoạt-động)
6. [Cách thu thập API](#6-cách-thu-thập-api)
7. [Rủi ro bảo mật](#7-rủi-ro-bảo-mật)
8. [So sánh các tool](#8-so-sánh-các-tool)
9. [Kết luận](#9-kết-luận)

---

## 1. TỔNG QUAN HỆ THỐNG

### 1.1 Sơ đồ kiến trúc

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                        HỆ THỐNG HACK DRAGON CITY                          │
└─────────────────────────────────────────────────────────────────────────────┘

┌───────────────────────────────────────┐     ┌───────────────────────────────────────┐
│         VINA-FULL.COM                │     │           DITLeP.COM                 │
│         (Frontend + Proxy)           │────►│         (Backend + BOT)              │
│                                       │     │                                       │
│  • Giao diện người dùng            │     │  • Server-side BOT                   │
│  • Thu thập Token                   │     │  • 248 API Endpoints                │
│  • Linkvertise (kiếm tiền ads)     │     │  • ASP.NET MVC 5                     │
│  • PHP Backend                      │     │  • SignalR Real-time                │
│  • Cookies/LocalStorage             │     │  • MySQL Database                   │
└───────────────────────────────────────┘     └───────────────────────────────────────┘
                                                │
                                                ▼
                                    ┌───────────────────────────────────────┐
                                    │      SOCIAL POINT GAMES              │
                                    │      (Server game thật)              │
                                    │                                       │
                                    │  • Xác thực Token                   │
                                    │  • Cập nhật Database                │
                                    │  • Anti-cheat systems                │
                                    └───────────────────────────────────────┘
```

### 1.2 Các thành phần chính

| Thành phần | URL | Technology | Chức năng |
|------------|-----|------------|------------|
| **VINA-FULL** | vina-full.com | PHP + jQuery | Frontend + Proxy |
| **DITLeP** | ditlep.com | ASP.NET MVC5 | Backend BOT chính |
| **NULLZERO** | nullzereptool.com | ??? | Premium/VIP version |
| **DC4U** | dc4u.eu | ??? | Clone khác |

---

## 2. VAI TRÒ TỪNG THÀNH PHẦN

### 2.1 VINA-FULL

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                              VINA-FULL                                    │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│  🎯 VAI TRÒ: Proxy / Cửa hàng kiếm tiền                              │
│                                                                             │
│  ┌─────────────────────────────────────────────────────────────────────┐   │
│  │  ✅ CHỨC NĂNG:                                                   │   │
│  │                                                                     │   │
│  │  1. Giao diện người dùng đẹp mắt                               │   │
│  │  2. Form nhập liệu (Token, Security Key)                          │   │
│  │  3. Yêu cầu xem quảng cáo (Linkvertise) → KIẾM TIỀN 💰        │   │
│  │  4. Thu thập Token của user                                       │   │
│  │  5. Chuyển tiếp Token đến DITLeP                                 │   │
│  │                                                                     │   │
│  └─────────────────────────────────────────────────────────────────────┘   │
│                                                                             │
│  ┌─────────────────────────────────────────────────────────────────────┐   │
│  │  ❌ KHÔNG LÀM:                                                   │   │
│  │                                                                     │   │
│  │  1. Không gửi request trực tiếp đến Social Point                │   │
│  │  2. Không có logic BOT                                          │   │
│  │  3. Không tự thêm gold/gems                                      │   │
│  │                                                                     │   │
│  └─────────────────────────────────────────────────────────────────────┘   │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

### 2.2 DITLeP

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                              DITLeP                                        │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│  🎯 VAI TRÒ: PROXY / TRUNG GIAN - BOT SERVER THẬT SỰ                   │
│                                                                             │
│  ┌─────────────────────────────────────────────────────────────────────┐   │
│  │  ✅ CHỨC NĂNG:                                                   │   │
│  │                                                                     │   │
│  │  1. Lưu trữ Token của user trong Database                        │   │
│  │  2. Server-side BOT (ASP.NET)                                      │   │
│  │  3. Tạo request đến Social Point với Token của user                │   │
│  │  4. Thu thập API qua Reverse Engineering (248 endpoints)           │   │
│  │  5. Xử lý response và trả về cho VINA-FULL                       │   │
│  │                                                                     │   │
│  └─────────────────────────────────────────────────────────────────────┘   │
│                                                                             │
│  ┌─────────────────────────────────────────────────────────────────────┐   │
│  │  ⚠️  ĐÂY LÀ PROXY BOT THẬT SỰ:                                 │   │
│  │                                                                     │   │
│  │  • Dùng Token của user để gửi request đến Social Point            │   │
│  │  • Server nhận request hợp lệ (vì Token đúng)                     │   │
│  │  • Gold/Gems được thêm vào tài khoản user                         │   │
│  │  • ⚠️ ĐÂY LÀ HÀNH VI BẤT HỢP PHÁP!                            │   │
│  │                                                                     │   │
│  └─────────────────────────────────────────────────────────────────────┘   │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

### 2.3 SO SÁNH VAI TRÒ

| Vai trò | VINA-FULL | DITLeP | Social Point |
|---------|-----------|--------|--------------|
| **Frontend** | ✅ Có | ✅ Có | ✅ Có |
| **Backend** | ✅ PHP | ✅ ASP.NET | ✅ Server chính |
| **BOT** | ❌ Không | ✅ Có | ❌ Không |
| **Database** | ❓ Có thể | ✅ MySQL | ✅ Database chính |
| **Gửi request đến SP** | ❌ Không | ✅ Có | N/A |

---

## 3. LUỒNG DỮ LIỆU

### 3.1 Flow đầy đủ

```
👤 USER                    🌐 VINA-FULL                🤖 DITLeP              🎮 SOCIAL POINT
   │                           │                         │                        │
   │ 1. Truy cập web          │                         │                        │
   │    vina-full.com          │                         │                        │
   │─────────────────────────►│                         │                        │
   │                           │                         │                        │
   │ 2. Xem quảng cáo         │                         │                        │
   │    (Linkvertise)          │                         │                        │
   │    💰 VINA kiếm tiền      │                         │                        │
   │─────────────────────────►│                         │                        │
   │                           │                         │                        │
   │ 3. Nhận Security Key     │                         │                        │
   │    ◄────────────────────│                         │                        │
   │                           │                         │                        │
   │ 4. Nhập Token            │                         │                        │
   │    "ABC_TOKEN_123"        │                         │                        │
   │─────────────────────────►│                         │                        │
   │                           │                         │                        │
   │                           │ 5. Gửi Token          │                        │
   │                           │    đến DITLeP         │                        │
   │                           │───────────────────────►│                        │
   │                           │                         │                        │
   │                           │                         │ 6. Lưu Token vào DB    │
   │                           │                         │─────────────────────── │
   │                           │                         │                        │
   │ 7. "Login Success"       │                         │                        │
   │ ◄────────────────────────│                         │                        │
   │                           │                         │                        │
   │ 8. Click "Get Gold"     │                         │                        │
   │─────────────────────────►│                         │                        │
   │                           │                         │                        │
   │                           │ 9. Gọi BOT API        │                        │
   │                           │───────────────────────►│                        │
   │                           │                         │                        │
   │                           │                         │ 10. BOT XỬ LÝ         │
   │                           │                         │ ┌───────────────────┐ │
   │                           │                         │ │                   │ │
   │                           │                         │ │ • Lấy Token từ DB │ │
   │                           │                         │ │ • Tạo Request    │ │
   │                           │                         │ │ • Gửi đến SP     │ │
   │                           │                         │ │ • Nhận Response  │ │
   │                           │                         │ └─────────┬─────────┘ │
   │                           │                         │           │           │
   │                           │                         │           ▼           │
   │                           │                         │ ┌───────────────────┐ │
   │                           │                         │ │                   │ │
   │                           │                         │ │ SOCIAL POINT     │ │
   │                           │                         │ │                   │ │
   │                           │                         │ │ ✓ Valid Token   │ │
   │                           │                         │ │ ✓ +150,000 Gold│ │
   │                           │                         │ │ ✓ Update DB    │ │
   │                           │                         │ └───────────────────┘ │
   │                           │                         │                        │
   │                           │ 11. Response ◄────────│                        │
   │ "Gold Added!" ◄──────────│                         │                        │
   │                           │                         │                        │
   │ 12. Check Game           │                         │                        │
   │ Gold: +150,000 ✅        │                         │                        │
   │                           │                         │                        │
   └─────────────────────────────────────────────────────────────────────────┘
```

### 3.2 Token Flow

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                         TOKEN FLOW                                         │
└─────────────────────────────────────────────────────────────────────────────┘

    👤 USER                          🔐 SOCIAL POINT
       │                                   │
       │  1. Đăng nhập Game             │
       │──────────────────────────────────►
       │                                   │
       │  2. Server cấp Token           │
       │◄──────────────────────────────────
       │                                   │
       │  ┌─────────────────────────────────────────────────────────────┐
       │  │  TOKEN (JWT - JSON Web Token)                              │
       │  │  ──────────────────────────────────────────────────────── │
       │  │                                                              │
       │  │  Header.Payload.Signature                                  │
       │  │                                                              │
       │  │  eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.                   │
       │  │  eyJ1c2VyX2lkIjoiMTIzNDU2Nzg5In0.                        │
       │  │  SFlIcXhTNWJFS2tYQnJKVGFjRDRYWHB6                        │
       │  │                                                              │
       │  │  • Part 1: Header (thuật toán mã hóa)                     │
       │  │  • Part 2: Payload (user_id, fb_mat_id, exp...)          │
       │  │  • Part 3: Signature (chống giả mạo)                      │
       │  │                                                              │
       │  └─────────────────────────────────────────────────────────────┘
       │                                   │
       │  3. Gửi Token cho VINA-FULL     │
       │───────────────────────────────────► 🔓 TOKEN BỊ LƯU TRỮ!
       │                                   │
       │  ┌─────────────────────────────────────────────────────────────┐
       │  │  VINA-FULL DATABASE:                                     │
       │  │  ┌─────────────────────────────────────────────────────┐   │
       │  │  │ user_id    │ token                    │ status       │   │
       │  │  │────────────┼──────────────────────────┼─────────────│   │
       │  │  │ 123456789 │ ABC_TOKEN_123            │ ACTIVE      │   │
       │  │  └─────────────────────────────────────────────────────┘   │
       │  │                                                              │
       │  │  ⚠️  HỌ CÓ THỂ DÙNG TOKEN NÀY BẤT CỨ LÚC NÀO!         │
       │  │                                                              │
       │  └─────────────────────────────────────────────────────────────┘
       │                                   │
       └─────────────────────────────────────────────────────────────────┘
```

---

## 4. API ENDPOINTS

### 4.1 VINA-FULL APIs

| Endpoint | Method | Chức năng | Request | Response |
|----------|--------|-----------|---------|----------|
| `/api/APILoginDragonCity` | POST | Login + lưu token | `{code, key}` | `{success, message}` |
| `/api/refresh_info` | POST | Lấy thông tin user | (Cookie/Session) | `{success, data: {name, level, cash, gold, food}}` |
| `/api/packet` | POST | Gọi BOT thêm gold | `{code, key, packet}` | `{success, message}` |
| `/api/accessToken.php` | POST | Xử lý FB token | Form data | `{result}` |
| `/api/loadInfo.php` | GET | Load rewards list | `{fbid, ssid, load, game}` | `{options}` |

### 4.2 DITLeP APIs (248 endpoints)

#### **Authentication & Token:**
```
/Facebook/Login
/CreateTokenLinkFromFbAccess
/TokenSession/GetSession
/TokenSession/UpdateStatus
```

#### **Resource Management:**
```
/UserResource/GetGems
/UserResource/GetGold
/UserResource/GetFood
/UserResource/Transfer
/UserResource/Check
```

#### **Dragon Management:**
```
/Dragon/DeleteDragon
/Dragon/MoveDragonToHabitat
/Dragon/MoveDragonToDragonarium
/Dragon/LearnSkill
/Dragon/UpdateName
/Dragon/GetDragonByIds
/Dragon/Sell
/Dragon/Store
```

#### **Breeding:**
```
/Breeding/BreedDragon
/Breeding/Calculate
/Breeding/CalculateNewBreeding
/Breeding/GetDataSource
/Breeding/GetGlobalHistory
/Breeding/SelectBreedingParents
```

#### **Arena & Battle:**
```
/Arena/Fight
/Arena/FightWithPremium
/Arena/SaveTeamCombat
/Arena/AcceptAttack
/Arena/Repeal
/Arena/GetArenas
/Arena/LeaderBoards
```

#### **Bot (SERVER-SIDE):**
```
/Bot/ActiveBotServerSide
/Bot/CheckCondition
/Bot/CollectGold
/Bot/GetConfig
```

#### **Shop & Items:**
```
/Buy/Buy
/Buy/BuySomeItem
/Buy/GetAllItems
/Buy/CheckAccount
/Buy/TimeLeft
```

### 4.3 Social Point APIs (Thu thập được)

```
POST /api/v1/user/gold/add
POST /api/v1/user/gems/add
POST /api/v1/user/food/add
POST /api/v1/dragon/hatch
POST /api/v1/dragon/feed
POST /api/v1/dragon/sell
POST /api/v1/arena/attack
POST /api/v1/arena/train
POST /api/v1/island/build
POST /api/v1/breeding/start
```

---

## 5. CƠ CHẾ HOẠT ĐỘNG

### 5.1 Cơ chế "HACK" thật sự

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                    CƠ CHẾ SERVER-SIDE BOT                                  │
└─────────────────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────────────────┐
│                                                                             │
│  🔍 BƯỚC 1: USER NHẬP TOKEN                                            │
│  ┌─────────────────────────────────────────────────────────────────────┐   │
│  │  Form nhập liệu:                                                   │   │
│  │  • Code: fb_123456789_ABC_TOKEN                                  │   │
│  │  • Security Key: LK-XXXX-XXXX                                     │   │
│  │                                                                     │   │
│  │  Token được gửi đến Backend VINA-FULL                            │   │
│  └─────────────────────────────────────────────────────────────────────┘   │
│                                                                             │
│  ┌─────────────────────────────────────────────────────────────────────┐   │
│  │  🔍 BƯỚC 2: VINA-FULL LƯU TOKEN                                 │   │
│  │                                                                     │   │
│  │  • PHP Backend nhận Token                                         │   │
│  │  • Lưu vào Database                                              │   │
│  │  • Chuyển đến DITLeP                                             │   │
│  └─────────────────────────────────────────────────────────────────────┘   │
│                                                                             │
│  ┌─────────────────────────────────────────────────────────────────────┐   │
│  │  🔍 BƯỚC 3: DITLeP BOT XỬ LÝ                                     │   │
│  │                                                                     │   │
│  │  • Lấy Token từ Database                                         │   │
│  │  • Tạo HTTP Request đến Social Point                             │   │
│  │  • Headers cần thiết:                                             │   │
│  │    - Authorization: Bearer <TOKEN>                                │   │
│  │    - X-Device-ID: <DEVICE_ID>                                     │   │
│  │    - X-App-Version: <VERSION>                                    │   │
│  │    - X-Timestamp: <TIME>                                          │   │
│  │    - X-Signature: <HMAC>                                          │   │
│  └─────────────────────────────────────────────────────────────────────┘   │
│                                                                             │
│  ┌─────────────────────────────────────────────────────────────────────┐   │
│  │  🔍 BƯỚC 4: SOCIAL POINT NHẬN REQUEST                           │   │
│  │                                                                     │   │
│  │  • Server nhận request                                            │   │
│  │  • Kiểm tra Token → HỢP LỆ ✓                                    │   │
│  │  • Xử lý như request bình thường                                 │   │
│  │  • Thêm Gold/Gems vào Database                                   │   │
│  │                                                                     │   │
│  │  ⚠️  SERVER KHÔNG BIẾT ĐÂY LÀ BOT!                              │   │
│  └─────────────────────────────────────────────────────────────────────┘   │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

### 5.2 Cơ chế xác thực Bearer Token

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                    BEARER TOKEN AUTHENTICATION                               │
└─────────────────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────────────────┐
│                                                                             │
│  🎫 BEARER TOKEN = "VÉ THẺ CHO PHÉP"                                     │
│                                                                             │
│  Giống như:                                                               │
│  • 🎫 Vé vào cửa - Cho phép vào đâu đó                                   │
│  • 💳 Thẻ ATM - Cho phép rút tiền                                        │
│                                                                             │
│  "Bearer" = "Người mang token này = Chủ sở hữu"                          │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────────────────┐
│                    JWT STRUCTURE (3 PHẦN)                                  │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│  PART 1: HEADER                                                           │
│  ───────────────                                                           │
│  {                                                                         │
│    "alg": "HS256",        ← Thuật toán (HMAC SHA-256)                      │
│    "typ": "JWT"            ← Loại token                                    │
│  }                                                                         │
│                                                                             │
│  PART 2: PAYLOAD (Dữ liệu)                                               │
│  ────────────────────────────                                               │
│  {                                                                         │
│    "user_id": "123456789",    ← ID của bạn trong game                     │
│    "fb_mat_id": "1000",       ← Facebook Matrix ID                         │
│    "exp": 1721828400,         ← Hết hạn                                   │
│    "iat": 1721742000,         ← Issued at                                  │
│    "iss": "socialpoint"       ← Issuer: Ai cấp                            │
│  }                                                                         │
│                                                                             │
│  PART 3: SIGNATURE (Chống giả mạo)                                        │
│  ─────────────────────────────────                                         │
│  Signature = HMAC_SHA256(                                                 │
│      Base64(Header) + "." + Base64(Payload),                            │
│      SECRET_KEY  ← Chỉ Social Point biết!                                │
│  )                                                                        │
│                                                                             │
│  ⚠️  Nếu ai đó sửa Payload → Signature không khớp → REJECTED!           │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────────────────┐
│                    REQUEST MẪU                                             │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│  POST /api/v1/user/gold/add HTTP/1.1                                      │
│  Host: api.dragoncity.com                                                  │
│  Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...            │
│  X-Device-ID: android_1234567890abcdef                                     │
│  X-App-Version: 12.5.1                                                     │
│  X-Timestamp: 1721738400                                                   │
│  X-Signature: a1b2c3d4e5f6...                                              │
│  Content-Type: application/json                                             │
│                                                                             │
│  {                                                                         │
│    "user_id": "123456789",                                                 │
│    "amount": 150000,                                                         │
│    "source": "event_reward",                                               │
│    "transaction_id": "txn_abc123"                                          │
│  }                                                                         │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## 6. CÁCH THU THẬP API

### 6.1 Ba phương pháp chính

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                    3 CÁCH THU THẬP API CỦA DITLeP                         │
└─────────────────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────────────────┐
│  📡 CÁCH 1: PACKET CAPTURING (MITM Proxy)                                 │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│  Dùng tools để bắt request khi game giao tiếp server:                    │
│                                                                             │
│  Tools:                                                                    │
│  • Charles Proxy ($50)                                                    │
│  • Fiddler (Miễn phí)                                                    │
│  • Wireshark (Miễn phí)                                                   │
│  • mitmproxy (Miễn phí)                                                   │
│  • Burp Suite ($400)                                                      │
│                                                                             │
│  Quy trình:                                                                │
│  1. Cài certificate của Proxy vào máy                                      │
│  2. Bật Proxy, mở Dragon City                                             │
│  3. Bắt tất cả HTTPS traffic                                              │
│  4. Phân tích request/response                                            │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────────────────┐
│  📱 CÁCH 2: DECOMPILE APK                                                 │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│  Phân giải ứng dụng Android để xem code nguồn:                           │
│                                                                             │
│  Tools:                                                                    │
│  • JADX (Miễn phí) - DEX Decompiler                                       │
│  • APKTool (Miễn phí) - APK Unpacker                                      │
│  • Ghidra (Miễn phí) - RE Framework (NSA)                                 │
│  • IDA Pro ($1,119+) - Disassembler                                        │
│  • Frida (Miễn phí) - Dynamic Instrumentation                              │
│                                                                             │
│  Quy trình:                                                                │
│  1. Lấy APK từ Google Play                                                 │
│  2. Decompile → classes.dex                                               │
│  3. Đọc Java/Kotlin code                                                  │
│  4. Tìm: API URLs, Request format, Secrets                                │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────────────────┐
│  🌐 CÁCH 3: WEB SCRAPING                                                   │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│  Duyệt web version và phân tích JavaScript:                               │
│                                                                             │
│  Tools:                                                                    │
│  • Chrome DevTools (Miễn phí)                                              │
│  • Browser Console (Miễn phí)                                               │
│  • curl/wget (Miễn phí)                                                    │
│  • Postman (Miễn phí)                                                      │
│  • Puppeteer (Miễn phí)                                                    │
│                                                                             │
│  Quy trình:                                                                │
│  1. Truy cập web version (Facebook Game)                                  │
│  2. Xem source code (Ctrl+U)                                              │
│  3. Phân tích JavaScript → Tìm API calls                                  │
│  4. Test với Postman/curl                                                 │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

### 6.2 Quá trình thu thập 248 APIs

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                    QUÁ TRÌNH THU THẬP API                                   │
└─────────────────────────────────────────────────────────────────────────────┘

    BƯỚC 1: BẮT ĐẦU
    ┌───────────────────────────────────────────────────────────────┐
    │  🔍 Thu thập 1 API đầu tiên (ví dụ: login)                   │
    │  POST /api/v1/user/login                                      │
    └───────────────────────────────────────────────────────────────┘
                                │
                                ▼
    BƯỚC 2: PHÂN TÍCH
    ┌───────────────────────────────────────────────────────────────┐
    │  📖 Đọc response của login API                                │
    │  • user_id, token, URLs khác trong JSON                       │
    └───────────────────────────────────────────────────────────────┘
                                │
                                ▼
    BƯỚC 3: RECURSIVE SCRAPING
    ┌───────────────────────────────────────────────────────────────┐
    │  🔄 Gọi API tiếp theo dựa trên response                       │
    │  • /api/v1/user/profile                                       │
    │  • /api/v1/dragons                                            │
    │  • /api/v1/arena                                              │
    │  • ...tiếp tục đệ quy...                                     │
    └───────────────────────────────────────────────────────────────┘
                                │
                                ▼
    BƯỚC 4: TỔNG HỢP
    ┌───────────────────────────────────────────────────────────────┐
    │  📋 Danh sách 248 API Endpoints:                              │
    │  ┌───────────────────────────────────────────────────────┐    │
    │  │  /User/...         →  20 APIs                          │    │
    │  │  /Dragon/...       →  25 APIs                          │    │
    │  │  /Arena/...       →  15 APIs                          │    │
    │  │  /Breeding/...    →  10 APIs                          │    │
    │  │  /Habitat/...      →   8 APIs                          │    │
    │  │  /Chest/...       →  12 APIs                          │    │
    │  │  /Event/...       →  15 APIs                          │    │
    │  │  /Bot/...         →   5 APIs                          │    │
    │  │  ...+ more                                              │    │
    │  │  ───────────────────────────────────────────────       │    │
    │  │  TOTAL: 248 APIs                                        │    │
    │  └───────────────────────────────────────────────────────┘    │
    └───────────────────────────────────────────────────────────────┘
                                │
                                ▼
    BƯỚC 5: TEST & VERIFY
    ┌───────────────────────────────────────────────────────────────┐
    │  ✅ Test từng API để xác nhận:                              │
    │  • Request format                                            │
    │  • Response format                                           │
    │  • Authentication requirements                                │
    │  • Headers cần thiết                                         │
    └───────────────────────────────────────────────────────────────┘
```

---

## 7. RỦI RO BẢO MẬT

### 7.1 Rủi ro cho User

```
╔═══════════════════════════════════════════════════════════════════════════════╗
║                                                                              ║
║   🚨 KHI NHẬP TOKEN VÀO VINA-FULL, BẠN ĐANG:                      ║
║                                                                              ║
╚═══════════════════════════════════════════════════════════════════════════════╝

┌─────────────────────────────────────────────────────────────────────────────┐
│  ⚠️  RỦI RO NGHIÊM TRỌNG                                                 │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│  🔓 CHO PHÉP NGƯỜI KHÁC KIỂM SOÁT TÀI KHOẢN GAME CỦA BẠN              │
│                                                                             │
│  Họ CÓ THỂ:                                                                │
│  ┌─────────────────────────────────────────────────────────────────────┐   │
│  │  ✅ Thêm gold/gems (điều bạn muốn)                                 │   │
│  │  ⚠️  Bán tài khoản của bạn                                        │   │
│  │  ⚠️  Trộm tất cả dragons, items quý                                │   │
│  │  ⚠️  Xóa tài khoản                                                 │   │
│  │  ⚠️  Dùng tài khoản để HACK người khác                             │   │
│  │  ⚠️  Bán thông tin cá nhân                                           │   │
│  │  ⚠️  Mua items bằng tiền thật (nếu có payment method)              │   │
│  └─────────────────────────────────────────────────────────────────────┘   │
│                                                                             │
│  💡 HỌ KHÔNG CẦN HACK GÌ - BẠN ĐÃ TỰ TAY ĐƯA TOKEN CHO HỌ!    │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

### 7.2 Hành vi pháp lý

```
╔═══════════════════════════════════════════════════════════════════════════════╗
║                                                                              ║
║   ⚖️  HÀNH VI CỦA DITLeP VI PHẠM:                               ║
║                                                                              ║
╚═══════════════════════════════════════════════════════════════════════════════╝

┌─────────────────────────────────────────────────────────────────────────────┐
│  ❌ VI PHẠM ĐIỀU KHOẢN SỬ DỤNG                                        │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│  Social Point Games có điều khoản:                                        │
│  "No hacking, cheating, or exploiting of any part of the Services"          │
│                                                                             │
│  DITLeP vi phạm:                                                          │
│  • Reverse engineer API                                                   │
│  • Tạo bot để gửi request giả mạo                                       │
│  • Can thiệp vào server game                                              │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────────────────┐
│  ❌ CÓ THỂ VI PHẠM LUẬT                                                  │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│  1. Computer Fraud (Lừa đảo máy tính)                                    │
│  2. Unauthorized Access (Truy cập trái phép)                              │
│  3. Theft of Services (Đánh cắp dịch vụ)                                 │
│  4. Copyright Infringement (Vi phạm bản quyền)                           │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

### 7.3 Cách bảo vệ bản thân

```
╔═══════════════════════════════════════════════════════════════════════════════╗
║                                                                              ║
║   🛡️  CÁCH BẢO VỆ BẢN THÂN                                              ║
║                                                                              ║
╚═══════════════════════════════════════════════════════════════════════════════╝

┌─────────────────────────────────────────────────────────────────────────────┐
│  ✅ NÊN LÀM:                                                              │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│  1. 🔒 KHÔNG BAO GIỜ nhập token vào tool bất kỳ                         │
│  2. 🔒 Chỉ dùng tool trên thiết bị/tài khoản PHỤ                          │
│  3. 🔒 Thường xuyên đổi password Facebook                                  │
│  4. 🔒 Bật 2FA (Two-Factor Authentication) nếu có thể                      │
│  5. 🔒 Theo dõi hoạt động tài khoản                                       │
│  6. 🔒 Xóa tài khoản Facebook liên kết nếu không cần                       │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────────────────┐
│  ❌ KHÔNG NÊN:                                                            │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│  1. 🚫 Nhập token vào tool bất kỳ                                        │
│  2. 🚫 Dán token vào forum/chat                                           │
│  3. 🚫 Lưu token ở nơi công khai                                         │
│  4. 🚫 Tin "Free Gold/Gems"                                                │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## 8. SO SÁNH CÁC TOOL

### 8.1 So sánh kỹ thuật

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                    SO SÁNH KỸ THUẬT                                       │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│  THÀNH PHẦN         │    DITLeP     │   VINA-FULL    │    DC4U          │
│ ─────────────────────┼───────────────┼────────────────┼───────────────── │
│  Backend            │  ASP.NET MVC 5 │     PHP        │     ???          │
│  Frontend          │  AngularJS     │   jQuery       │    jQuery        │
│  Database           │     MySQL      │     ???        │     ???          │
│  Real-time          │    SignalR    │      ❌        │      ❌           │
│  API Endpoints      │     248       │      5         │      5?            │
│  Server-side BOT    │      ✅       │      ❌        │      ❌           │
│  Ads Integration    │    Google      │  Linkvertise   │      ???          │
│  Premium Model     │     VIP       │    VIP Link    │      ???          │
│                                                                             │
├─────────────────────────────────────────────────────────────────────────────┤
│  TÍNH NĂNG              │    DITLeP     │   VINA-FULL    │    DC4U          │
│ ─────────────────────────┼───────────────┼────────────────┼─────────────────│
│  Breeding Calculator     │      ✅       │      ❌        │      ❌           │
│  Arena Tracker          │      ✅       │      ❌        │      ❌           │
│  Event Calendar        │      ✅       │      ❌        │      ❌           │
│  Leaderboard Viewer    │      ✅       │      ❌        │      ❌           │
│  Dragon Info DB        │      ✅       │      ❌        │      ❌           │
│  "Hack" Gold/Gems     │      ❌       │      ⚠️        │      ⚠️           │
│  Bot Server-side       │      ✅       │      ❌        │      ❌           │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

### 8.2 Hệ sinh thái tools

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                    HỆ SINH THÁI TOOL DRAGON CITY                           │
└─────────────────────────────────────────────────────────────────────────────┘

                           ╔═══════════════════════════════╗
                           ║         DITLeP.COM          ║
                           ║   (MOTHER SOURCE - GỐC)      ║
                           ║                               ║
                           ║  • ASP.NET MVC 5             ║
                           ║  • 248 API Endpoints         ║
                           ║  • SignalR Real-time         ║
                           ║  • Server-side BOT           ║
                           ║                               ║
                           ║  ✅ Tool hợp lệ (helper)     ║
                           ║  ⚠️ BOT bất hợp pháp        ║
                           ╚═══════════════════════════════╝
                    ╱               │               ╲
                   ╱                │                ╲
                  ╱                 │                 ╲
══════════════════════════════════════════════════════════════════════════════
│                                                                         │
│                         CÁC TOOL CLONE/BRANCH                           │
│                                                                         │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐  ┌─────────────┐ │
│  │  VINA-FULL   │  │    DC4U      │  │  NULLZERO    │  │   OTHERS    │ │
│  │ vina-full.com│  │   dc4u.eu   │  │nullzereptool │  │             │ │
│  │              │  │              │  │    .com      │  │             │ │
│  │ • PHP Proxy  │  │ • PHP Proxy │  │ • Premium   │  │ • Various   │ │
│  │ • Linkvertise│  │ • Direct?   │  │ • VIP Only  │  │   clones    │ │
│  │ • DITLeP API │  │ • DITLeP?   │  │ • DITLeP    │  │             │ │
│  │              │  │              │  │              │  │             │ │
│  │ 💰 Kiếm tiền │  │ 💰 Kiếm tiền │  │ 💰 Kiếm tiền │  │             │ │
│  │   từ ads     │  │   từ ads     │  │   từ VIP    │  │             │ │
│  │              │  │              │  │              │  │             │ │
│  │ ⚠️ Fake hack │  │ ⚠️ Fake hack │  │ ⚠️ Fake hack │  │             │ │
│  │ ⚠️ Lừa đảo  │  │ ⚠️ Lừa đảo  │  │ ⚠️ Lừa đảo  │  │             │ │
│  └──────────────┘  └──────────────┘  └──────────────┘  └─────────────┘ │
│                                                                         │
══════════════════════════════════════════════════════════════════════════════
                                    │
                                    ▼
┌───────────────────────────────────────────────────────────────────────────┐
│                      SOCIAL POINT GAMES (Nguồn game)                      │
│                        socialpointgames.com                               │
│                                                                             │
│                    • Server chính thức của Dragon City                    │
│                    • Data verification phía SERVER                        │
│                    • Anti-cheat systems                                   │
│                    • KHÔNG THỂ HACK từ bên ngoài (đúng cách)             │
│                                                                             │
└───────────────────────────────────────────────────────────────────────────┘
```

---

## 9. KẾT LUẬN

### 9.1 Tóm tắt vai trò

```
╔═══════════════════════════════════════════════════════════════════════════════╗
║                                                                              ║
║   🎯 TÓM TẮT VAI TRÒ:                                                    ║
║                                                                              ║
╠═══════════════════════════════════════════════════════════════════════════════╣
║                                                                              ║
║   👤 USER:          Cung cấp Token → Tự nguyện                            ║
║                                                                              ║
║   🌐 VINA-FULL:     Giao diện → Kiếm tiền từ ads → Chuyển Token          ║
║                     (CÔNG CHỨ kiếm tiền)                                   ║
║                                                                              ║
║   🤖 DITLeP:        PROXY BOT → Gửi request đến SP → Thêm Gold           ║
║                     (ÔNG CHỦ điều khiển BOT) ⚠️ BẤT HỢP PHÁP!             ║
║                                                                              ║
║   🎮 SOCIAL POINT:   Nhận request → (vì Token hợp lệ) → Cộng Gold         ║
║                     ⚠️ KHÔNG BIẾT LÀ BOT!                                 ║
║                                                                              ║
╠═══════════════════════════════════════════════════════════════════════════════╣
║                                                                              ║
║   💰 KẾT QUẢ:                                                           ║
║   • User được +Gold (thật) ✅                                             ║
║   • VINA-FULL kiếm tiền ads ✅                                             ║
║   • DITLeP có bot service ✅                                               ║
║   • Social Point bị thiệt hại ⚠️                                         ║
║                                                                              ║
╚═══════════════════════════════════════════════════════════════════════════════╝
```

### 9.2 Điểm mấu chốt

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                    ĐIỂM MẤU CHỐT CẦN NHỚ                                 │
└─────────────────────────────────────────────────────────────────────────────┘

1️⃣  VINA-FULL KHÔNG tự thêm gold - chỉ là Proxy/Frontend

2️⃣  DITLeP là PROXY BOT thật sự - gửi request đến Social Point

3️⃣  Social Point nhận request vì Token hợp lệ (của chính user)

4️⃣  Token = "Tiền mặt" - Ai có token = Ai có quyền của bạn!

5️⃣  DITLeP KHÔNG hợp tác với Dragon City - đang HACK!

6️⃣  Đây là hành vi BẤT HỢP PHÁP - có thể vi phạm pháp luật!

7️⃣  User tự nguyện cung cấp Token = đồng lõa với hành vi bất hợp pháp
```

### 9.3 Bài học rút ra

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                    BÀI HỌC RÚT RA                                          │
└─────────────────────────────────────────────────────────────────────────────┘

🛡️  BẢO MẬT:
• Token giống như mật khẩu - KHÔNG chia sẻ!
• Không nhập thông tin nhạy cảm vào tool bên thứ ba
• Theo dõi hoạt động tài khoản thường xuyên

🔬 KỸ THUẬT:
• Hiểu cách API hoạt động (REST, JWT, HMAC)
• Biết cách Reverse Engineering (để học, không để hack)
• Hiểu cơ chế xác thực Bearer Token

⚖️  PHÁP LÝ:
• HACK game = Vi phạm điều khoản sử dụng
• Có thể vi phạm luật Computer Fraud
• Người dùng đồng lõa khi cung cấp Token
```

---

## 📚 TÀI LIỆU THAM KHẢO

- **DITLeP APIs**: 248 endpoints tìm thấy trong `/bundles/scriptApp`
- **VINA-FULL Code**: JavaScript analysis từ `vina-full.com/games/get-gold-free`
- **Bearer Token**: JWT structure với Header.Payload.Signature
- **Reverse Engineering Tools**: JADX, APKTool, Charles Proxy, Wireshark

---

## ⚠️ DISCLAIMER

> **Document này chỉ dành cho mục đích TẠO TOOL.**
> 
> Việc sử dụng các tool hack game là để hoc hoi .
> 
> Tác giả khuyến khích hoặc ủng hộ bất kỳ hành vi hack, cheat, .

---

*Document được tạo vào: 2024-07-23*
*Nguồn: Phân tích từ vina-full.com, ditlep.com*
