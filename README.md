# Hệ thống Quản lý Đơn hàng Blockchain

## Cấu trúc dự án

```
order-blockchain/
├── contracts/
│   └── OrderManagement.sol     ← Smart Contract chính
├── scripts/
│   └── deploy.js               ← Script deploy lên blockchain
├── test/
│   └── OrderManagement.test.js ← Unit test
├── frontend/
│   └── index.html              ← Giao diện web
├── backend/
│   ├── server.js               ← Express server
│   ├── package.json
│   └── routes/
│       └── orders.js           ← API routes gọi Smart Contract
├── hardhat.config.js
├── package.json
├── .env.example                ← Copy thành .env rồi điền thông tin
└── README.md
```

## Hướng dẫn chạy

### Bước 1 — Cài đặt dependencies

```bash
# Cài dependencies cho Hardhat (Smart Contract)
npm install

# Cài dependencies cho Backend
cd backend && npm install && cd ..
```

### Bước 2 — Cấu hình môi trường

```bash
# Copy file env mẫu
cp .env.example .env

# Mở .env và điền vào:
# SEPOLIA_RPC_URL = lấy từ Alchemy (alchemy.com) - miễn phí
# PRIVATE_KEY     = private key ví MetaMask (KHÔNG chia sẻ với ai)
```

### Bước 3 — Compile và Test Smart Contract

```bash
# Compile
npx hardhat compile

# Chạy test
npx hardhat test
```

### Bước 4 — Deploy Smart Contract

```bash
# Deploy lên Sepolia Testnet
npx hardhat run scripts/deploy.js --network sepolia

# Copy địa chỉ contract in ra, điền vào .env:
# CONTRACT_ADDRESS=0x...
# Đồng thời điền vào frontend/index.html dòng CONTRACT_ADDRESS
```

### Bước 5 — Chạy Backend

```bash
cd backend
npm start
# Backend chạy tại http://localhost:5000
```

### Bước 6 — Mở Frontend

Mở file `frontend/index.html` bằng trình duyệt (dùng Live Server trong VS Code)

---

## API Endpoints

| Method | URL | Chức năng |
|--------|-----|-----------|
| GET | /api/orders | Lấy tất cả đơn hàng |
| GET | /api/orders/:id | Lấy một đơn hàng |
| POST | /api/orders | Tạo đơn hàng mới |
| PUT | /api/orders/:id/status | Cập nhật trạng thái |
| GET | /api/orders/buyer/:address | Đơn hàng theo địa chỉ ví |

## Trạng thái đơn hàng

| Số | Tên | Ý nghĩa |
|----|-----|---------|
| 0 | Pending | Chờ xử lý |
| 1 | Shipping | Đang giao |
| 2 | Completed | Hoàn thành |
| 3 | Cancelled | Đã huỷ |
