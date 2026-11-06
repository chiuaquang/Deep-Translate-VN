# Tool dịch Ren'Py — Deep Translate VN

File: `dich-renpy.html`

Tool dịch game Ren'Py, **chỉ nhận file `.rpy`**, không nhận `.json`, không có KiriKiri.

> Bản Ren'Py trong repo này được giữ nguyên ở bản cũ ổn định (có Xuất TXT / Nhập TXT). Các cải tiến mới nhất chỉ áp dụng cho bản RPG Maker MV/MZ, không áp dụng cho bản này.

## Trích xuất

Tool trích từ file `.rpy`:

- Thoại, lời dẫn, lựa chọn trong menu, cặp `old` / `new` trong bản dịch `tl`, chuỗi nhiều dòng trong dấu ngoặc kép ba lần.

## Dịch tự động

Xem phần Dịch tự động bằng Model trong `README.md`.

- Ngôn ngữ nguồn mặc định: Tự động. Ngôn ngữ đích mặc định: Tiếng Việt.
- Nhà cung cấp: Google Translate (miễn phí), Index Translate (miễn phí, 2B / 9B / 35B-A3B), Gemini Flash (dùng key của người dùng, lưu trong trình duyệt).
- Prompt dịch dùng cho Gemini và Index Translate, không dùng cho Google Translate.

## Giữ nguyên tag

Tag Ren'Py không được dịch, không được đổi, không được mất:

`{b}`, `{i}`, `{s}`, `{u}`, `{color}`, `{size}`, `{font}`, `{alpha}`, `{cps}`, `{w}`, `{p}`, `{nw}`, `{fast}`, biến `[...]`, `%s`, `%d`, `%(name)s`, `%%`, `\n`.

Tool trích tag ra trước khi gửi cho model, sau khi dịch khôi phục tag về đúng vị trí. Tag bị thiếu được tự chèn lại vào vị trí tương ứng.

## Lưu tiến độ 7 ngày

Tiến độ dịch tự lưu chỉ trong trình duyệt của người dùng, không gửi lên máy chủ, giữ trong 7 ngày, khóa theo tên file, vị trí dòng và bản gốc. Tải lại cùng một file trong 7 ngày thì tool khôi phục các dòng đã dịch.

Hãy xuất file đã dịch ra thường xuyên làm bản sao lưu, đừng chỉ dựa vào tiến độ trong trình duyệt.

## Xuất / Nhập

- Một file: xuất đúng tên gốc, ví dụ `script.rpy`.
- Nhiều file: xuất thành `Deep Translate VN Renpy.zip`, các file bên trong giữ đúng tên gốc.
- Xuất / Nhập CSV và TXT theo đúng định dạng trong `README.md`.
