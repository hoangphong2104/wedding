# Deploy thiệp cưới lên GitHub Pages

Thư mục này là **nội dung web tĩnh đã hoàn chỉnh** — không cần build, không cần sửa code.
Việc cần làm: đẩy nguyên thư mục này lên GitHub và bật GitHub Pages.

## Thông tin

- Repo đích: `hoangphong2104/wedding` (đang trống)
- Branch: `main`
- Nguồn Pages: branch `main`, folder `/ (root)`

## Nội dung thư mục

```
index.html      trang chọn thiệp (2 link)
.nojekyll       BẮT BUỘC giữ lại
fonts/          font dùng chung cho cả 2 thiệp
P-N/            thiệp nhà trai  (index.html + images/ + js/)
N-P/            thiệp nhà gái   (index.html + images/ + js/)
```

## Ràng buộc quan trọng

1. **Phải có `.nojekyll` ở root.** Nhiều ảnh có tên bắt đầu bằng `_` (ví dụ `_0026_clear-white-plaster-texture-pattern-copy-11-...png`); nếu Jekyll chạy, GitHub sẽ bỏ qua các file đó và thiệp mất ảnh nền. File này là file rỗng, dễ bị mất khi upload bằng kéo–thả trên web.
2. **Giữ nguyên cấu trúc thư mục.** `P-N/index.html` và `N-P/index.html` tham chiếu font theo đường dẫn `../fonts/...`, nên `fonts/` phải nằm cùng cấp với `P-N/` và `N-P/`.
3. **Giữ nguyên tên file, kể cả chữ hoa/thường.** GitHub Pages phân biệt hoa thường; ảnh có phần mở rộng `.JPG`/`.jpg` khác nhau.
4. Không đổi tên `P-N` / `N-P` (tránh ký tự `&` vì làm hỏng link khi gửi qua Zalo/Messenger).
5. Không sửa nội dung HTML/CSS/ảnh — đây là bản đã chốt với khách.

## Các bước

```bash
cd <thư-mục-này>
git init -b main
git add -A                      # kiểm tra .nojekyll đã được add
git commit -m "Wedding invitation site: P-N (nhà trai), N-P (nhà gái)"
git remote add origin git@github.com:hoangphong2104/wedding.git   # hoặc https://
git push -u origin main
```

Sau đó bật Pages (dùng `gh` nếu có):

```bash
gh api -X POST repos/hoangphong2104/wedding/pages \
  -f "source[branch]=main" -f "source[path]=/"
```

Hoặc thủ công: repo → Settings → Pages → Source: *Deploy from a branch* → `main` / `/ (root)` → Save.

## Kiểm tra sau khi deploy

Chờ 1–2 phút, mở và xác nhận cả 3 link trả về 200 và **hiển thị đủ ảnh** (đặc biệt ảnh nền vân giấy):

- https://hoangphong2104.github.io/wedding/
- https://hoangphong2104.github.io/wedding/P-N/
- https://hoangphong2104.github.io/wedding/N-P/

Kiểm tra thêm:
- Mở trên điện thoại: thiệp full màn hình.
- Mở trên máy tính (≥768px): thiệp canh giữa trên nền vân giấy, có bóng đổ hai bên.
- Nút bật/tắt nhạc góc dưới phải hoạt động.
- Cuộn hết trang: album ảnh, đồng hồ đếm ngược, form xác nhận tham dự đều hiển thị.

Nếu ảnh bị lỗi 404 → gần như chắc chắn `.nojekyll` chưa được commit.
