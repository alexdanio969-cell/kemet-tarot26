# Kemet Tarot — hướng dẫn

## Cấu trúc thư mục
- `public/index.html` — toàn bộ giao diện: xáo bài, trải vòng tròn, bốc 3 lá, hít thở 10 giây, lật bài, hiện lời luận.
- `netlify/functions/reading.mts` — hàm máy chủ gọi AI (Claude Haiku 4.5 qua Netlify AI Gateway) để luận bài. Không cần API key.
- `netlify.toml` — cấu hình Netlify: thư mục `public`, thư mục hàm và các header bảo mật.

## Đưa lên Netlify
Vì có hàm máy chủ, **không dùng kéo-thả (Netlify Drop)** được — kéo-thả chỉ đưa file tĩnh lên, phần luận AI sẽ không chạy.
Chọn một trong hai cách:

**Cách 1 — qua GitHub (khuyên dùng, sửa sau này dễ nhất)**
1. Tạo repository mới trên GitHub, tải toàn bộ nội dung thư mục này lên (giữ nguyên cấu trúc).
2. Trên Netlify: *Add new project → Import an existing project → GitHub* → chọn repository.
3. Để trống Build command; Publish directory Netlify tự đọc từ `netlify.toml` (`public`). Bấm Deploy.

**Cách 2 — Netlify CLI trên máy bạn** (cần cài Node.js)
```
npm install -g netlify-cli
cd kemet-tarot
netlify login
netlify deploy --prod
```

## Chi phí AI
- Mỗi lần luận dùng khoảng 600–900 token với Claude Haiku 4.5, tức chỉ vài phần nghìn đô la, Netlify quy ra credit (1 USD = 180 credit).
- Mỗi người bị giới hạn 6 lần luận trong 3 phút (cấu hình `rateLimit` trong `reading.mts`).
- Nên theo dõi mục *Usage* trên Netlify. Khi hết credit hoặc AI lỗi, trang tự chuyển sang bản luận rút gọn theo ý nghĩa từng lá, không bị trắng trang.
- Muốn lời luận sâu hơn: đổi `MODEL` trong `reading.mts` sang `claude-sonnet-4-6` (đắt hơn khoảng 3 lần).

## Chỉnh sửa thường gặp
- Đổi số lượt cho phép: sửa `windowLimit` / `windowSize` (tối đa 180 giây) trong `reading.mts`.
- Đổi giọng văn lời luận: sửa đoạn `SYSTEM` trong `reading.mts`.
- Ý nghĩa xuôi/ngược của 78 lá lấy theo bảng bạn cung cấp. Muốn sửa: đổi dữ liệu `DECK` ở cả `public/index.html` và `netlify/functions/reading.mts` cho khớp nhau.
- Cách AI luận: với mỗi lá, AI giải nghĩa lá (theo xuôi/ngược) rồi nối thẳng vào câu hỏi; phần cuối "Lời sấm truyền" đọc 3 lá như một câu chuyện và gợi ý 1–2 việc làm cụ thể. Giọng văn được dặn phải thấu hiểu, chia sẻ, tránh câu sáo. Muốn chỉnh, sửa đoạn `SYSTEM` trong `reading.mts`.
