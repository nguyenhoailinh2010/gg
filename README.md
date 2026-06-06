# 🔁 GitHub Actions KeepAlive + MiNet Runner

Tool tự động chạy liên tục trên GitHub Actions, không tắt, và thực thi script MiNet.

---

## 📁 Cấu trúc

```
.github/
  workflows/
    keepalive.yml       ← Workflow chính: chạy script + giữ sống 350 phút
    auto-restart.yml    ← Tự trigger lại mỗi 4 giờ
README.md
```

---

## 🚀 Cách dùng

### Bước 1 – Tạo repo GitHub mới
1. Vào https://github.com/new
2. Tạo repo (Public hoặc Private đều được)
3. Upload toàn bộ file này lên

### Bước 2 – Bật GitHub Actions
1. Vào tab **Actions** của repo
2. Nhấn **"I understand my workflows, go ahead and enable them"**

### Bước 3 – Cấp quyền cho workflow tự trigger
1. Vào **Settings → Actions → General**
2. Scroll xuống **Workflow permissions**
3. Chọn **"Read and write permissions"**
4. Tick **"Allow GitHub Actions to create and approve pull requests"**
5. Nhấn **Save**

### Bước 4 – Chạy lần đầu
1. Vào tab **Actions**
2. Chọn workflow **"KeepAlive & MiNet Runner"**
3. Nhấn **"Run workflow"** → **Run workflow**

---

## ⚙️ Cách hoạt động

| Workflow | Lịch chạy | Mô tả |
|---|---|---|
| `keepalive.yml` | Mỗi 5 phút | Chạy script MiNet + loop 350 phút |
| `auto-restart.yml` | Mỗi 4 giờ | Tự trigger lại keepalive |

### Luồng:
```
schedule (5 phút)
      ↓
keepalive.yml chạy
      ↓
Chạy: irm https://dashboard.minet.vn/setup.ps1 | iex
      ↓
Loop giữ sống 350 phút (ping mỗi 4 phút)
      ↓
auto-restart.yml trigger lại → lặp vô tận
```

---

## ⚠️ Lưu ý

- GitHub Actions **miễn phí 2000 phút/tháng** với repo public (vô hạn với public repo)
- Dùng **repo Public** để không tốn quota
- GitHub có thể **tắt schedule** nếu repo không có commit trong 60 ngày → push 1 file nhỏ mỗi tháng để tránh

---

## 🛠️ Tùy chỉnh

Sửa trong file `keepalive.yml`:
- `timeout-minutes: 360` → giới hạn tối đa 1 lần chạy
- `$maxMins = 350` → số phút loop giữ sống
- `$ping = 4` → ping mỗi N phút
