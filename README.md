# Thư viện bài hát thờ phượng — Web app trên Netlify (miễn phí)

Trang web tĩnh hiển thị danh sách bài hát phân theo 4 chủ đề, kèm trang quản trị
`/admin` để **thêm / sửa / xóa bài hát** trong tương lai mà không cần biết lập trình.

- Dữ liệu nằm trong `data/songs.json` (lưu trong Git → có lịch sử, lỡ sai hoàn tác được).
- Không cần database. Không phát sinh chi phí (gói Free của Netlify là giới hạn cứng).
- Thêm bài → Netlify tự build lại → web cập nhật sau khoảng 1 phút.

```
├── index.html        ← trang hiển thị (tự đọc data/songs.json)
├── data/songs.json   ← TẤT CẢ bài hát + chủ đề
├── admin/            ← trang quản trị (Decap CMS)
│   ├── index.html
│   └── config.yml
└── netlify.toml
```

---

## A. Đưa lên mạng lần đầu (làm 1 lần, ~10 phút)

### 1. Đưa code lên GitHub
1. Tạo tài khoản GitHub (nếu chưa có) tại https://github.com
2. Tạo repository mới, ví dụ tên `thu-vien-bai-hat` (để **Public** cho đơn giản).
3. Tải toàn bộ thư mục này lên repo (kéo-thả qua nút **Add file → Upload files**, hoặc dùng Git).

### 2. Kết nối Netlify
1. Tạo tài khoản tại https://www.netlify.com (đăng nhập bằng GitHub là nhanh nhất).
2. **Add new site → Import an existing project → GitHub → chọn repo vừa tạo.**
3. Mục Build: để trống (Build command rỗng, Publish directory là `.`). Bấm **Deploy**.
4. Sau ~1 phút site đã chạy ở địa chỉ kiểu `https://ten-ngau-nhien.netlify.app`.
   Có thể đổi tên ở **Site configuration → Change site name**.

### 3. Bật đăng nhập cho trang quản trị (`/admin`)
Trang `/admin` cần đăng nhập để được quyền ghi vào GitHub:
1. Trong Netlify: **Site configuration → Identity → Enable Identity.**
2. Phần **Registration**, chọn **Invite only** (chỉ người được mời mới vào được — an toàn).
3. Kéo xuống **Services → Git Gateway → Enable Git Gateway.**
4. Tab **Identity → Invite users**, nhập email của bạn (và của người phụ trách bài hát).
5. Mở email lời mời → đặt mật khẩu. Xong.

> Lưu ý: trong file `admin/config.yml` có dòng `branch: main`. Nếu nhánh chính
> của repo tên khác (ví dụ `master`), hãy sửa lại cho đúng.

---

## B. Thêm một bài hát mới (việc làm thường xuyên — rất đơn giản)

1. Mở `https://<tên-site>.netlify.app/admin/`
2. Đăng nhập (email + mật khẩu đã đặt ở bước A.3).
3. Vào **Thư viện bài hát → Danh sách bài hát.**
4. Bấm vào mục **Bài hát**, cuộn xuống cuối, bấm nút **“Add Bài hát”.**
5. Nhập **Tên bài hát** và chọn **Chủ đề** (1–4 hoặc Dịp đặc biệt).
6. Bấm **Publish**.
7. Đợi khoảng 1 phút → mở lại trang chính, bài mới đã xuất hiện đúng chủ đề.

Sửa hoặc xóa bài: cũng vào đúng danh sách đó, chỉnh trực tiếp rồi Publish.

> Cách thủ công (không cần đăng nhập admin): vào repo GitHub, mở `data/songs.json`,
> bấm bút chì để sửa, thêm một dòng `{ "title": "Tên bài", "cat": "2" }`, rồi Commit.
> Netlify cũng sẽ tự build lại.

---

## C. Lưu ý về chi phí (gói Free, cập nhật 2026)

- Gói **Free = 300 credits/tháng**, là **giới hạn cứng**: hết thì site tạm dừng đến
  đầu tháng sau, **không bị tính tiền**. Không có rủi ro hóa đơn bất ngờ trên gói này.
- Mỗi lần publish tốn ~15 credits (~20 lần/tháng); băng thông ~15 GB/tháng.
  Với hội thánh thêm bài thưa thớt và lượng truy cập nhỏ → dư sức dùng.
- Nếu sau này lượng truy cập lớn hơn, có thể chuyển miễn phí sang **Cloudflare Pages**
  (băng thông không giới hạn) — cùng cách deploy từ GitHub, gần như không phải sửa gì.

---

## D. (Tùy chọn) Nếu muốn cập nhật tức thì, không cần build lại
Có thể thay cách lưu Git bằng một database miễn phí (ví dụ **Supabase**) + một
**Netlify Function** để form ghi thẳng vào DB; web đọc trực tiếp từ DB nên không
phải build. Cách này “app” hơn nhưng thêm phần phải bảo trì (DB, bảo mật endpoint).
Với nhu cầu thỉnh thoảng thêm bài, cách lưu trong Git ở trên là gọn và bền hơn.
