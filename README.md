# Contract Review Agent — Hợp Đồng Giao Khoán
**GreenNode Claw-a-thon 2026** | Team: Zion Legal Tech

---

## Giới thiệu

Agent tự động rà soát hợp đồng giao khoán (CTV) theo đúng framework pháp lý Việt Nam. Người dùng upload file `.docx` → AI phân tích → trả kết quả chi tiết gồm 2 bảng đánh giá.

**Agent giải quyết vấn đề gì?**
- Mỗi lần rà soát hợp đồng mất 30–45 phút nếu làm tay
- Dễ bỏ sót rủi ro pháp lý quan trọng (lao động trá hình, thuế, BHXH)
- Agent tự động hóa toàn bộ trong <60 giây, chuẩn hóa theo 10 luật áp dụng

---

## Cài đặt & Chạy (dành cho non-tech)

### Bước 1: Cài đặt các công cụ cần thiết

1. **Python 3.11+**: https://www.python.org/downloads/
   - Khi cài, tick vào ô "Add Python to PATH"
   
2. **Docker Desktop**: https://docs.docker.com/get-started/introduction/get-docker-desktop/

3. **Git CLI**: https://git-scm.com/install/windows

### Bước 2: Lấy API Key của Anthropic

1. Truy cập https://console.anthropic.com
2. Tạo tài khoản hoặc đăng nhập
3. Vào "API Keys" → "Create Key"
4. Copy key (dạng: `sk-ant-...`)

### Bước 3: Chạy agent trên máy (local)

Mở Terminal (Windows: Win+R → gõ `cmd`), chạy lần lượt:

```bash
# Di chuyển vào folder agent
cd contract-review-agent

# Set API key (thay YOUR_KEY_HERE bằng key của bạn)
# Windows:
set ANTHROPIC_API_KEY=YOUR_KEY_HERE

# Mac/Linux:
export ANTHROPIC_API_KEY=YOUR_KEY_HERE

# Cài thư viện
pip install -r requirements.txt

# Chạy agent
python app.py
```

Mở trình duyệt và truy cập: **http://localhost:8080**

### Bước 4: Deploy lên GreenNode AgentBase

```bash
# Bước 4a: Clone bộ skill AgentBase vào folder hiện tại
git clone https://github.com/vngcloud/greennode-agentbase-skills .agentbase

# Bước 4b: Thêm IAM credential của GreenNode vào agent
# (thực hiện theo hướng dẫn của AgentBase skill)

# Bước 4c: Build Docker image
docker build -t contract-review-agent .

# Bước 4d: Chạy AgentBase skill để deploy lên cloud
# (AI sẽ hướng dẫn 9 bước còn lại tự động)
```

---

## Cách sử dụng

1. Mở http://localhost:8080 (hoặc link AgentBase sau khi deploy)
2. Kéo thả hoặc chọn file hợp đồng `.docx`
3. Nhấn **"Bắt đầu rà soát"**
4. Chờ ~30–60 giây
5. Đọc kết quả gồm:
   - **KẾT QUẢ 1**: Tóm tắt điều hành + mức độ rủi ro tổng thể
   - **KẾT QUẢ 2**: Bảng chi tiết từng điều khoản

---

## Cấu trúc file

```
contract-review-agent/
├── app.py              # Server chính + giao diện web
├── system_prompt.txt   # Toàn bộ framework pháp lý (Phần A–E)
├── requirements.txt    # Thư viện Python cần thiết
├── Dockerfile          # Để deploy lên AgentBase
└── README.md           # File này
```

---

## Troubleshooting

**Lỗi "ANTHROPIC_API_KEY not found"**
→ Chưa set API key. Chạy lại lệnh `set ANTHROPIC_API_KEY=...` (Windows) hoặc `export ANTHROPIC_API_KEY=...` (Mac/Linux)

**Lỗi "pip not found"**
→ Python chưa được thêm vào PATH. Cài lại Python và tick "Add to PATH"

**Lỗi đọc file .docx**
→ Chạy: `pip install python-docx`

**Port 8080 đã bị dùng**
→ Đổi PORT trong `app.py` từ `8080` sang `8081` hoặc số khác

---

## Use Case Description (để nộp cuộc thi)

Agent tự động rà soát hợp đồng giao khoán (CTV/Cộng tác viên) cho Công ty Cổ phần Zion — giải quyết nỗi đau mỗi hợp đồng mất 30–45 phút rà soát tay và dễ bỏ sót rủi ro. Agent áp dụng framework pháp lý chuẩn gồm 10 luật Việt Nam, phát hiện 4 nhóm vấn đề (điều khoản khác mẫu, lỗi văn bản, chỗ trống chưa điền, rủi ro dữ liệu cá nhân), phân loại rủi ro 3 mức độ và đề xuất phương án xử lý + chiến thuật đàm phán — toàn bộ trong dưới 60 giây.
