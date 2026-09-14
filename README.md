# Bàn chơi giấy — Cờ caro & 2048

Web game tĩnh, không cần máy chủ, không cần cơ sở dữ liệu. Điểm và thành tích lưu bằng
`localStorage` ngay trên máy người chơi, nên hosting nào cũng chạy được miễn là phục vụ file tĩnh.

## Trong thư mục có gì

| File | Việc của nó |
|---|---|
| `index.html` | Toàn bộ trang: landing, menu, cờ caro, 2048 |
| `sw.js` | Service worker — chơi được cả khi mất mạng |
| `manifest.webmanifest` | Cho phép "cài" lên màn hình chính như một app |
| `icon-192.png`, `icon-512.png` | Biểu tượng app |
| `og.png` | Ảnh hiện ra khi dán link vào Zalo, Messenger, Facebook |

Tất cả đường dẫn đều tương đối (`./…`), nên đặt ở thư mục gốc hay thư mục con đều chạy.

## Chạy thử trên máy trước khi đưa lên mạng

Mở thẳng `index.html` cũng chơi được, nhưng service worker chỉ hoạt động qua http. Chạy:

```bash
cd site
python3 -m http.server 8000
```

Rồi mở `http://localhost:8000`.

## Đưa lên mạng — chọn một trong ba cách

### 1. Netlify Drop — nhanh nhất, khoảng một phút

1. Vào `https://app.netlify.com/drop`.
2. Kéo cả thư mục `site` (hoặc file `ban-choi-giay.zip`) thả vào ô giữa trang.
3. Nhận link dạng `ten-ngau-nhien.netlify.app`.
4. **Quan trọng:** site thả ẩn danh sẽ bị xoá sau khoảng một tiếng nếu không đăng ký tài khoản để
   "claim". Đăng ký miễn phí, bấm nhận site, rồi vào *Site settings → Change site name* để đổi thành
   tên dễ nhớ.

Cập nhật sau này: sửa file ở máy, vào trang Deploys của site rồi thả lại thư mục mới.

### 2. GitHub Pages — hợp khi muốn giữ mã nguồn lâu dài

1. Tạo repo mới trên GitHub, ví dụ `ban-choi-giay`, để Public.
2. Upload toàn bộ file trong thư mục `site` lên nhánh `main` (kéo thả ngay trên web GitHub cũng được).
3. Vào **Settings → Pages**, mục *Build and deployment* chọn *Deploy from a branch*, chọn nhánh
   `main`, thư mục `/ (root)`, bấm Save.
4. Đợi 1–2 phút, link sẽ là `https://<tên-tài-khoản>.github.io/ban-choi-giay/`.

Mỗi lần `git push` là tự động cập nhật. Băng thông và dung lượng miễn phí thoải mái cho một trang thế này.

### 3. Cloudflare Pages — nhanh ở Việt Nam, băng thông không giới hạn

1. Đăng ký tài khoản Cloudflare, vào **Workers & Pages → Create → Pages**.
2. Chọn *Upload assets* rồi kéo thư mục `site` vào (hoặc nối với repo GitHub ở cách 2 để tự động
   deploy mỗi lần push).
3. Đặt tên project, bấm Deploy. Link sẽ là `https://<tên-project>.pages.dev`.

Gói miễn phí: request tới file tĩnh không giới hạn, 500 lượt build mỗi tháng, tên miền riêng kèm SSL.

### Tên miền riêng

Cả ba nơi đều cho gắn tên miền riêng miễn phí (chỉ tốn tiền mua tên miền, khoảng 200–300k/năm cho
`.com`). Trong phần DNS, trỏ bản ghi `CNAME` về địa chỉ mà hosting cung cấp.

## Sau khi sửa nội dung

Service worker lưu bản cũ trong máy người chơi. Mỗi lần cập nhật, mở `sw.js` và đổi:

```js
var VERSION = "v1";   // -> "v2", "v3", ...
```

Nếu quên, người chơi cũ sẽ vẫn thấy bản cũ cho tới khi họ xoá cache.

## Ghi chú

- Điểm cao lưu theo từng tên miền. Đổi hosting là bảng điểm bắt đầu lại từ đầu.
- Không có tài khoản, không có máy chủ, nên không có bảng xếp hạng chung giữa nhiều người. Muốn có
  thì cần thêm một backend nhỏ (ví dụ Supabase hoặc Cloudflare Workers + KV).
- Font lấy từ Google Fonts. Nếu muốn trang chạy hoàn toàn offline ngay lần mở đầu, tải file font về
  đặt cạnh `index.html` rồi khai báo bằng `@font-face`.
