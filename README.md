# Thiệp cưới Hoàng Phong & Hồng Nhung — hướng dẫn deploy

Trang thiệp cưới tĩnh (HTML/CSS/JS thuần, không build step). Nhiệm vụ: **đẩy thư mục này lên GitHub và bật GitHub Pages**. Không cần sửa code, không cần framework.

- Repo đích: `hoangphong2104/wedding`, branch `main`
- Nội dung deploy: **toàn bộ nội dung bên trong thư mục `deploy/`** đặt ở **root của repo** (không lồng thêm một cấp `deploy/`)

## Cấu trúc cần có ở root repo

```
.nojekyll          ← BẮT BUỘC, file rỗng
index.html         ← trang chọn thiệp (link sang 2 bản)
fonts/             ← 8 font .ttf/.otf dùng chung cho cả 2 bản
P-N/
  index.html       ← thiệp nhà trai (Phong & Nhung)
  images/          ← 90+ ảnh/icon
  js/
N-P/
  index.html       ← thiệp nhà gái (Nhung & Phong)
  images/
  js/
```

`P-N/index.html` và `N-P/index.html` tham chiếu font bằng đường dẫn tương đối `../fonts/...`, nên **cấu trúc thư mục phải giữ đúng như trên**, không được làm phẳng hay đổi tên `fonts/`.

## Các bước

1. Clone `hoangphong2104/wedding` (repo hiện đang trống).
2. Copy toàn bộ nội dung bên trong `deploy/` vào root repo.
3. Kiểm tra `.nojekyll` **thật sự tồn tại** ở root. Đây là điểm dễ sai nhất: nhiều công cụ và giao diện web GitHub bỏ qua file ẩn. Không có nó, Jekyll sẽ **bỏ qua mọi file bắt đầu bằng `_`** — trong đó có ảnh nền vân giấy `images/_0026_clear-white-plaster-texture-pattern-copy-11-...png`, làm hỏng nền của cả hai bản thiệp.
4. Commit + push lên `main`.
5. Settings → Pages → Source: **Deploy from a branch** → Branch `main`, folder `/ (root)` → Save.
6. Chờ 1–2 phút rồi kiểm tra 3 link ở mục dưới.

## Link sau khi deploy

| Trang | URL |
| --- | --- |
| Trang chọn thiệp | `https://hoangphong2104.github.io/wedding/` |
| Thiệp nhà trai | `https://hoangphong2104.github.io/wedding/P-N/` |
| Thiệp nhà gái | `https://hoangphong2104.github.io/wedding/N-P/` |

Tên thư mục dùng `P-N` / `N-P` thay vì `P&N` / `N&P` — ký tự `&` phá URL khi gửi qua Zalo/Messenger.

## Tính năng tên khách (đừng làm mất khi deploy)

Mỗi thiệp mở bằng một màn "mở thiệp" đọc tên khách từ query string:

```
https://hoangphong2104.github.io/wedding/P-N/?guest=Anh%20Nguy%E1%BB%85n%20V%C4%83n%20Nam
```

- Nhận `?guest=`, `?ten=`, `?khach=`, `?name=` (lấy giá trị đầu tiên có mặt).
- Không có tên → hiện "Trân trọng kính mời / quý khách", không lỗi, không khoảng trắng lạ.
- Màn mở thiệp tự mở sau 3 giây, hoặc chạm/click vào bất kỳ đâu để mở ngay.
- Toàn bộ logic nằm inline trong từng `index.html` (`<style id="om-cover-css">` + `<div id="om-cover">` + script cuối `<body>`). Không tách file, không thêm dependency.

Cần kiểm tra sau deploy: mở link **có** `?guest=` và link **không có** query, cả hai phải hiển thị đúng.

## Kiểm tra sau khi deploy

- [ ] 3 URL trên đều load, không 404
- [ ] Nền vân giấy hiện đúng (nếu nền trắng trơn → thiếu `.nojekyll`)
- [ ] Font chữ viết tay/script hiện đúng, không rơi về serif hệ thống (nếu rơi → sai đường dẫn `fonts/`)
- [ ] Màn mở thiệp cao đúng 1 màn hình điện thoại, không kéo dài theo cả trang
- [ ] Ảnh trong album và các mục đều load
- [ ] Form RSVP hiển thị và submit được
- [ ] Thử trên điện thoại thật (Safari iOS + Chrome Android), không chỉ trên desktop

## Lưu ý kỹ thuật

- **Không** chạy formatter/minifier lên các `index.html`. File gốc xuất từ trình dựng trang, CSS/JS inline rất dài và nhạy với thay đổi thứ tự.
- **Không** đổi tên file ảnh. Tên có hash và một số bắt đầu bằng `_`; markup tham chiếu chính xác từng tên.
- Không có bước build, không `package.json`, không cần Node. Chỉ là file tĩnh.
- Nếu sau này gắn domain riêng, thêm file `CNAME` ở root và cập nhật lại link chia sẻ.

## File tham khảo (không deploy)

Trong project gốc còn:

- `Preview dien thoai.html` — xem 2 bản thiệp trong khung điện thoại 390×812
- `Preview may tinh.html` — xem trong khung browser 1280×720
- `covers/` — 3 phương án màn mở thiệp đã dựng để so sánh (phương án B được chọn)
- `chu-re-moi/`, `co-dau-moi/` — bản nguồn, giống hệt `deploy/P-N` và `deploy/N-P`

Các file này chỉ để review, **không** đưa lên repo.
