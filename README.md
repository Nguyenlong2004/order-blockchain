# ⛓ TechChain — Hệ thống Quản lý Đơn hàng Blockchain

---

## 📄 Poster đề tài

![Poster](poster_blockchain1.jpg)

---

## 📋 Giới thiệu đề tài

Hệ thống quản lý đơn hàng trực tuyến ứng dụng công nghệ **Blockchain Ethereum**, cho phép:

- 🛒 **Khách hàng** đặt hàng trực tuyến không cần ví điện tử
- 🔐 **Quản trị viên** xác nhận và ký giao dịch bằng MetaMask
- ⛓ **Blockchain** lưu trữ đơn hàng **vĩnh viễn, bất biến, minh bạch**
- 🔍 **Bất kỳ ai** có thể tra cứu và xác minh qua Etherscan

> ⚠️ **Điểm khác biệt cốt lõi**: Dữ liệu đơn hàng được ghi lên Blockchain — không ai có thể chỉnh sửa hay xóa bỏ, kể cả Admin của hệ thống.

---

## 🏗️ Kiến trúc hệ thống

```
┌─────────────────────────────────────────────────────────┐
│                    FRONTEND LAYER                       │
│         shop.html (Khách)  │  admin.html (Admin)        │
│              HTML + CSS + JavaScript                    │
└─────────────────────┬───────────────────────────────────┘
                      │ HTTP REST API
┌─────────────────────▼───────────────────────────────────┐
│                    BACKEND LAYER                        │
│              Node.js + Express.js                       │
│         routes/orders.js  │  routes/ratings.js          │
└─────────────────────┬───────────────────────────────────┘
                      │ Ethers.js v6
┌─────────────────────▼───────────────────────────────────┐
│                  BLOCKCHAIN LAYER                       │
│           Ethereum Sepolia Testnet                      │
│    OrderManagement.sol  │  RatingContract.sol           │
│         (Bất biến — Minh bạch — Phi tập trung)         │
└─────────────────────────────────────────────────────────┘
```

---

## 🛠️ Công nghệ sử dụng

| Công nghệ | Phiên bản | Vai trò |
|---|---|---|
| **Solidity** | 0.8.19 | Viết Smart Contract |
| **Hardhat** | 2.19.0 | Compile, Test, Deploy |
| **Ethers.js** | 6.9.0 | Kết nối JS với Blockchain |
| **MetaMask** | Extension | Ký giao dịch (Admin) |
| **Node.js** | 22.x LTS | Môi trường Backend |
| **Express.js** | 4.18.x | REST API Server |
| **Ethereum Sepolia** | Testnet | Mạng Blockchain thử nghiệm |
| **Chart.js** | 4.4.1 | Biểu đồ thống kê Admin |
| **QRCode.js** | 1.0.0 | Tạo QR Code phiếu đơn hàng |

---

## 📁 Cấu trúc dự án

```
order-blockchain/
├── contracts/
│   ├── OrderManagement.sol     ← Smart Contract quản lý đơn hàng
│   └── RatingContract.sol      ← Smart Contract đánh giá sản phẩm
│
├── scripts/
│   ├── deploy.js               ← Deploy OrderManagement lên Sepolia
│   └── deploy_rating.js        ← Deploy RatingContract lên Sepolia
│
├── test/
│   └── OrderManagement.test.js ← Unit test (5/5 pass)
│
├── frontend/
│   ├── shop.html               ← Giao diện cửa hàng (Khách hàng)
│   └── admin.html              ← Dashboard quản trị (Admin)
│
├── backend/
│   ├── server.js               ← Express server (port 5000)
│   ├── package.json
│   └── routes/
│       ├── orders.js           ← API quản lý đơn hàng
│       └── ratings.js          ← API đánh giá sản phẩm
│
├── hardhat.config.js           ← Cấu hình Hardhat + Sepolia
├── package.json
├── .env.example                ← Mẫu cấu hình môi trường
└── README.md
```

---

## ⚙️ Hướng dẫn cài đặt và chạy

### Yêu cầu hệ thống

- Node.js v18+ ([tải tại đây](https://nodejs.org))
- MetaMask extension ([cài tại đây](https://metamask.io))
- VS Code + Live Server extension

### Bước 1 — Clone và cài đặt dependencies

```bash
git clone https://github.com/your-username/order-blockchain.git
cd order-blockchain

# Cài dependencies Hardhat
npm install

# Cài dependencies Backend
cd backend && npm install && cd ..
```

### Bước 2 — Cấu hình môi trường

```bash
cp .env.example .env
```

Mở file `.env` và điền thông tin:

```env
SEPOLIA_RPC_URL=https://ethereum-sepolia-rpc.publicnode.com
PRIVATE_KEY=your_metamask_private_key_here
CONTRACT_ADDRESS=0xf3025a00c93D53432471029502935b65e61a4A74
RATING_CONTRACT_ADDRESS=0x81a50979b6cBc69FFE2dbE7546271e951b3B8202
PORT=5000
```

> ⚠️ **QUAN TRỌNG**: Không bao giờ commit file `.env` lên GitHub. File này đã được thêm vào `.gitignore`.

### Bước 3 — Lấy ETH test (Sepolia Faucet)

Truy cập [https://faucets.chain.link/sepolia](https://faucets.chain.link/sepolia) để nhận ETH test miễn phí.

### Bước 4 — Compile và kiểm thử Smart Contract

```bash
# Compile Solidity
npx hardhat compile

# Chạy unit test
npx hardhat test
```

Kết quả mong đợi:
```
OrderManagement
  ✓ Tạo đơn hàng thành công
  ✓ Cập nhật trạng thái đơn hàng
  ✓ Không cho người khác cập nhật
  ✓ Lấy danh sách đơn theo địa chỉ ví
  ✓ Đếm tổng số đơn hàng

5 passing (592ms)
```

### Bước 5 — Deploy Smart Contract (nếu cần deploy lại)

```bash
# Deploy OrderManagement
npx hardhat run scripts/deploy.js --network sepolia

# Deploy RatingContract
npx hardhat run scripts/deploy_rating.js --network sepolia
```

> 📌 Smart Contract đã được deploy sẵn — xem địa chỉ ở phần bên dưới.

### Bước 6 — Chạy Backend

```bash
cd backend
npm start
# ✅ Backend chạy tại http://localhost:5000
```

### Bước 7 — Mở Frontend

Mở VS Code → Chuột phải vào `frontend/shop.html` → **Open with Live Server**

---

## 🔗 Smart Contract đã deploy

| Contract | Địa chỉ | Etherscan |
|---|---|---|
| OrderManagement | `0xf3025a00c93D53432471029502935b65e61a4A74` | [Xem ↗](https://sepolia.etherscan.io/address/0xf3025a00c93D53432471029502935b65e61a4A74) |
| RatingContract | `0x81a50979b6cBc69FFE2dbE7546271e951b3B8202` | [Xem ↗](https://sepolia.etherscan.io/address/0x81a50979b6cBc69FFE2dbE7546271e951b3B8202) |

---

## 📡 API Endpoints

### Orders API

| Method | Endpoint | Chức năng |
|---|---|---|
| `GET` | `/api/orders` | Lấy tất cả đơn hàng |
| `GET` | `/api/orders/:id` | Lấy một đơn hàng theo ID |
| `POST` | `/api/orders` | Tạo đơn hàng mới |
| `PUT` | `/api/orders/:id/status` | Cập nhật trạng thái |
| `GET` | `/api/orders/buyer/:address` | Đơn hàng theo địa chỉ ví |

### Ratings API

| Method | Endpoint | Chức năng |
|---|---|---|
| `POST` | `/api/ratings` | Thêm đánh giá sản phẩm |
| `GET` | `/api/ratings/product/:name` | Rating của sản phẩm |
| `GET` | `/api/ratings/check/:orderId` | Kiểm tra đã đánh giá chưa |

---

## 📊 Trạng thái đơn hàng

```
Pending (0) ──→ Shipping (1) ──→ Completed (2)
     │
     └──────────────────────────→ Cancelled (3)
```

| Mã | Trạng thái | Ý nghĩa |
|---|---|---|
| 0 | `Pending` | Chờ xử lý |
| 1 | `Shipping` | Đang giao hàng |
| 2 | `Completed` | Hoàn thành |
| 3 | `Cancelled` | Đã huỷ |

---

## ✨ Tính năng hệ thống

### 🛒 Trang cửa hàng (Khách hàng)
- Xem 6 sản phẩm công nghệ với ảnh thật
- Đặt hàng không cần MetaMask
- Xem lịch sử đơn hàng cá nhân
- Tra cứu đơn hàng theo mã Blockchain
- Đánh giá sản phẩm sau khi đơn hoàn thành

### ⚙️ Trang quản trị (Admin)
- Dashboard thống kê: 4 stat cards + biểu đồ Chart.js
- Kết nối MetaMask và ký giao dịch
- Xác nhận / Từ chối đơn hàng chờ
- Tìm kiếm & lọc đơn hàng realtime
- Cập nhật trạng thái đơn hàng on-chain
- In phiếu đơn hàng kèm QR Code Etherscan

---

## 🔒 Tính bảo mật và minh bạch

| Đặc tính | Mô tả |
|---|---|
| **Bất biến** | Đơn hàng đã xác nhận không thể xóa hay sửa |
| **Minh bạch** | Tra cứu công khai trên Etherscan |
| **Phân quyền** | Chỉ Admin ký được giao dịch qua MetaMask |
| **Chống gian lận** | Mỗi đơn chỉ được đánh giá 1 lần |
| **Audit trail** | Mọi thay đổi có dấu vết giao dịch trên chain |

---

## 📄 Giấy phép

Dự án được thực hiện phục vụ mục đích học thuật tại **Trường Đại học Đại Nam**.

---
