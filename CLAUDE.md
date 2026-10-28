# Lunar Capital Tool (repo lunar-capital-tool)

Tool cho guild Lunar Capital, chạy trên trình duyệt. Bản làm lại 2026-10-28 theo giao diện archify (preset signal-flow: nền navy lưới chấm, JetBrains Mono, 6 màu ngữ nghĩa, khối và đường nối kiểu sơ đồ). Không có con thú như bản cũ. Bản cũ (9 chế độ, Discord, link `/danhsach` của bot) đã xoá hẳn, xem lịch sử git trước commit thay bản mới.

## Cấu trúc
- `index.html`: một file HTML + CSS + JS, không build, không thư viện. 3 tab đổi bằng `?mode=`:
  - `pass` Vòng quay may mắn: giữ mặt trăng lấy đà, người thắng bốc lúc thả tay (`crypto.getRandomValues`), lực chỉ quyết định số vòng.
  - `team` Chia team: 2–6 đội lệch nhau tối đa 1 người; cặp dính/tách giải bằng `solveTeams` (union-find + quay lui). Mỗi người là một thẻ tên bay theo đường nối, còn trong khối nguồn thì trắng, qua phễu mới đổi màu đội.
  - `gift` Gift code Where Winds Meet: hàng chờ copy (Space/⌘Z), lọc, tự nhập, khối tải app Overlay Copy.
- `gift-code/js/codes-data.js`: danh sách code (`window.WWM_CODES`). **Giữ đúng định dạng này**, vì script cập nhật ghi đè nó.
- `gift-code/scripts/update-codes.js`: lấy code từ codes.yar.gg; `--push` thì commit + push. launchd `com.huytran.wwm-codes.update` chạy 12:00 mỗi ngày, log ở `gift-code/logs/` (không commit).
- `gift-code/index.html`: chuyển link cũ `/gift-code/` sang `?mode=gift`.

## Quy ước
- Lưu localStorage tiền tố `lunar-`. Riêng code đã dùng giữ khoá `wwm.used.v1` của trang gift-code cũ để người dùng không mất tiến trình.
- Mọi thứ hiện ra hay mất đi đều phải có hiệu ứng (anh Hato chê "chưa mượt"): nút thu bề ngang thay vì `hidden`, chữ đổi bằng `fadeText`, ô trống và ô có người cao bằng nhau. Không dùng `background-attachment:fixed` và `backdrop-filter` trên khung lớn vì điện thoại giật.
- Copy dùng `toClipboard()`: mở bằng IP mạng LAN (http) không có `navigator.clipboard` nên phải có đường lùi `execCommand`.

## Kiểm tra
- Tách `<script>` cuối file rồi `new Function(...)` hoặc `node --check` để bắt lỗi cú pháp.
- Browser pane bị ẩn thì `requestAnimationFrame` không chạy. Test hoạt hình bằng Playwright headless `--mute-audio`, lấy từ `~/Documents/TOOLS/TOURNAMENT/node_modules/playwright`.
- Thử trên điện thoại: `python3 -m http.server 5178 --bind 0.0.0.0`, mở `http://<IP máy Mac>:5178`.

## Deploy
GitHub Pages từ branch `main`, thư mục root: https://huytran19.github.io/lunar-capital-tool/
