# Deep Translate VN

Công cụ dịch game **RPG Maker MV/MZ** và **Ren'Py** chạy hoàn toàn trên trình duyệt, giao diện visual novel màu pastel, giữ nguyên mã game và tag khi dịch.

Trang live: https://lxvip.neocities.org/

> **Lưu ý:** Bản Ren'Py trong repo này được giữ nguyên ở bản cũ ổn định (có Xuất TXT / Nhập TXT). Các cải tiến mới nhất chỉ áp dụng cho bản RPG Maker MV/MZ.

## File trong repo

| File | Mô tả |
| --- | --- |
| `index.html` | Trang chủ, chọn công cụ dịch |
| `huongdan.html` | Hướng dẫn sử dụng, danh sách mã được bảo vệ, hướng dẫn Batch dịch |
| `dich-rpg-maker-mv-mz.html` | Tool dịch RPG Maker MV/MZ, chỉ nhận file `.json` |
| `dich-renpy.html` | Tool dịch Ren'Py, chỉ nhận file `.rpy` |

Không có KiriKiri. Hai tool tách riêng, không gộp chung.

## Cách dùng nhanh

1. Mở `index.html`, hoặc mở thẳng tool cần dùng.
2. Kéo thả file game vào tool:
   - RPG Maker: nhiều file `.json` cùng lúc, ví dụ `Map001.json`, `CommonEvents.json`, `System.json`.
   - Ren'Py: nhiều file `.rpy` cùng lúc.
3. Dịch từng dòng bằng tay, hoặc dùng **Dịch tự động bằng Model** để dịch tất cả dòng chưa dịch.
4. Xuất file đã dịch. Tên file xuất giữ đúng tên gốc, không thêm hậu tố.

Xem hướng dẫn đầy đủ trong `huongdan.html`.

## Dịch tự động bằng Model

- Ngôn ngữ nguồn mặc định: Tự động. Ngôn ngữ đích mặc định: Tiếng Việt.
- Nhà cung cấp: **Google Translate** (miễn phí), **Index Translate** (miễn phí, 2B / 9B / 35B-A3B), **Gemini Flash** (dùng key của người dùng, lưu trong trình duyệt).
- Prompt dịch dùng cho Gemini và Index Translate, không dùng cho Google Translate. Prompt có thể sửa, lưu tên, tải lại, xóa, khôi phục mặc định.
- **Batch dịch:** số dòng mỗi lần gọi, mặc định 20, tối đa 50, nên để 10–20. Batch cao dễ bị Google chặn 429, mất tag, tốn quota.
- RPG Maker có thêm **Số luồng song song** cho Index và Gemini, mặc định 3, từ 1 đến 5. Google Translate dịch tuần tự.

## Giữ nguyên mã game

Mã game và tag **không được dịch, không được đổi, không được mất, không được thêm mới**:

- RPG Maker: `\C[n]`, `\N[n]`, `\V[n]`, `\I[n]`, `\P[n]`, `\G`, `\S[n]`, `\dac`, `\nc`, và các mã điều khiển khác.
- Ren'Py: `{b}`, `{i}`, `{s}`, `{u}`, `{color}`, `{size}`, `{w}`, `{p}`, `{nw}`, `{fast}`, biến `[...]`, `%s`, `%d`, `%(name)s`, `%%`.

Tool trích mã/tag ra trước khi gửi cho model, model không bao giờ thấy mã thật, sau khi dịch tool khôi phục mã về đúng vị trí. Nếu mã bị thiếu, tool tự chèn lại vào vị trí tương ứng. Nếu mã bị thừa hoặc bị đổi, tool xóa / sửa lại thành đúng mã gốc.

Danh sách mã được bảo vệ đầy đủ chỉ có trong `huongdan.html`.

## Phát hiện Dịch lỗi (chỉ bản RPG Maker)

Một dòng đã dịch được tính là **Dịch lỗi** khi có ít nhất một lỗi sau, kèm lý do cụ thể cho từng dòng:

- Bản dịch chứa mã giữ chỗ chưa khôi phục, ví dụ `ZXQ0QXZ`.
- Bản dịch chỉ còn mã, dấu câu, số, không còn chữ.
- Bản dịch chứa chuỗi lỗi máy: `undefined`, `null`, `[object Object]`.
- Bản dịch quá ngắn bất thường, dưới 20% bản gốc dài trên 20 ký tự.
- Mã bị thiếu, thừa, hoặc bị đổi sau khi đã chèn lại vẫn không đủ.

Chip **Dịch lỗi (n)** lọc đúng các dòng lỗi. Dòng lỗi vẫn tính trong Đã dịch. Trong Khung dịch có hộp cảnh báo liệt kê đủ lý do, kèm nút **Xóa bản dịch lỗi**, nút **Dịch lại dòng này**, và nút **Đánh dấu dịch sai** cho lỗi về nghĩa mà máy không tự phát hiện được.

Nút **Dịch lại tất cả dòng Dịch lỗi** dịch lại đúng các dòng đang lỗi bằng model đang chọn. Nút **Sửa tất cả dòng Dịch lỗi** sửa mã/tag bằng code có sẵn, không dịch lại bằng model.

### Cảnh cáo khi xuất file

Khi bấm **Xuất file đã dịch** mà còn dòng chưa dịch, dòng Dịch lỗi, hoặc dòng Thiếu tag, tool hiện hộp cảnh cáo ghi rõ số dòng từng loại. Bấm **Vẫn xuất file** thì vẫn xuất đúng như cũ. Bấm **Quay lại sửa** thì không xuất gì và chuyển bộ lọc sang Dịch lỗi để sửa nhanh. Cảnh cáo này chỉ áp dụng cho xuất file game, không áp dụng cho Xuất CSV / Xuất TXT.

## Thoại xuống dòng và Word wrap (chỉ bản RPG Maker)

- Mã xuống dòng trong RPG Maker là `\n`. Một đoạn có `\n` ở giữa là **một đoạn duy nhất**, ví dụ:
  `Những ngày tươi sáng và hạnh phúc của các cô gái bắt đầu.\n(Thành tựu Mở đầu)`
  Tool giữ đúng một lần `\n` đó qua trích xuất, dịch, hiển thị, xuất file và nhập TXT, không tách thành hai dòng dịch.
- Ô Bản dịch là ô nhiều dòng. Nút **Chèn xuống dòng** chèn `\n` đúng vị trí con trỏ. Nút **Tách thoại dài thành nhiều dòng** tách bản dịch tại biên từ, mỗi dòng con không quá khoảng 55 ký tự không tính mã.
- **Word wrap:** ô tick mặc định tắt, **Word wrap limit** mặc định 55, từ 20 đến 100. Khi bật, lúc xuất file game tool tự ngắt dòng thoại bằng `\n` sao cho mỗi đoạn con không dài quá limit. Nút **Áp dụng Word wrap cho tất cả dòng đã dịch** ngắt ngay trong tool để xem trước. Nút **Xóa Word wrap đã áp dụng** hoàn tác về đúng bản dịch trước khi ngắt, giữ nguyên `\n` đã có sẵn, rồi anh đổi limit và áp dụng lại được. Word wrap chỉ áp dụng cho dòng thoại, không áp dụng cho Xuất CSV / Xuất TXT.
- **Hoàn tác Word wrap tách theo từng game:** bản sao để hoàn tác Word wrap được khóa theo tên game (`gameTitle` trong `System.json`) + bộ file đang mở, giống như tiến độ dịch. Mở game mới thì ghi chú hoàn tác của game cũ (ví dụ 360 dòng, limit 46) không hiện nữa, nút **Xóa Word wrap đã áp dụng** mờ đi, và bấm nút đó ở game mới không làm gì cả, không khôi phục bản dịch của game cũ vào game mới. Quay lại đúng game cũ, đúng bộ file, thì bản sao của game cũ hiện lại và hoàn tác được đúng cho game đó. Tải riêng file trước rồi tải thêm `System.json` sau thì bản sao tạm được chuyển sang khóa theo tên game mới, không mất bản sao.

## Nhận biết tên game theo dự án (chỉ bản RPG Maker)

Tên game lấy từ `System.json`, trường `gameTitle`. Tool hiện thanh **Dự án: \<tên game\>** ngay trên danh sách file.

Tiến độ 7 ngày được khóa theo tên game + tên file + đường dẫn + vị trí + mã băm câu gốc, nên hai game khác nhau dù trùng tên file (`Map001.json`, `CommonEvents.json`) và trùng câu gốc cũng không khôi phục nhầm tiến độ vào nhau.

Nếu tải thêm bộ file của game khác tên vào lúc đang mở một game, tool hiện cảnh báo với hai lựa chọn **Chuyển sang game mới** và **Giữ game cũ**, không tự trộn hai game.

## Lưu tiến độ 7 ngày

Tiến độ dịch tự lưu **chỉ trong trình duyệt của người dùng**, không gửi lên máy chủ, giữ trong 7 ngày.

- RPG Maker: mỗi file có bản lưu riêng, câu gốc chỉ lưu mã băm + độ dài, ghi chia mảnh để không làm hỏng bản cũ khi ghi dở. Bản lưu từ bản tool trước vẫn được đọc và tự chuyển sang dạng mới.
- Ren'Py: lưu theo tên file, vị trí dòng và bản gốc, tương tự.

Tải lại cùng một bộ file, đúng thứ tự, trong 7 ngày thì tool khôi phục các dòng đã dịch và báo đã khôi phục bao nhiêu, còn bao nhiêu dòng chưa dịch.

**Quan trọng:** tiến độ trong trình duyệt chỉ là bản nháp. Hãy xuất file đã dịch ra thường xuyên làm bản sao lưu, đừng chỉ dựa vào tiến độ trong trình duyệt.

## Xuất / Nhập file

### Xuất file đã dịch

- Một file: xuất đúng tên gốc và đuôi gốc, ví dụ `Map001.json`, `script.rpy`.
- Nhiều file: xuất thành một file zip, tên `Deep Translate VN RPG.zip` (RPG Maker) hoặc `Deep Translate VN Renpy.zip` (Ren'Py), các file bên trong giữ đúng tên gốc.

### Xuất / Nhập CSV

Xuất tất cả dòng theo thứ tự danh sách. Nhập lại file `.csv` để điền bản dịch vào đúng dòng, cập nhật thống kê và lưu tiến độ ngay.

### Xuất / Nhập TXT

Định dạng TXT, mỗi dòng văn bản là một khối, đánh số từ 0:

```text
-----0
Michan
-----1
Hi there!
-----2
Mila
-----3
Thank you for saying hello.
```

- **Xuất TXT:** nội dung mỗi khối là bản dịch nếu đã dịch, nếu chưa dịch thì xuất nguyên văn để dịch ngoài rồi nhập lại. Một file thì lấy tên gốc đổi đuôi thành `.txt`, nhiều file thì lấy tên `Deep Translate VN RPG.txt` hoặc `Deep Translate VN Renpy.txt`.
- **Nhập TXT:** chỉ nhận `.txt`, đọc UTF-8, bỏ BOM, xử lý cả `\n` và `\r\n`. Khối số *n* ghi vào dòng ở cùng vị trí trong danh sách hiện tại. Phải nhập lại vào **đúng bộ file và đúng thứ tự** như lúc xuất, nếu không số thứ tự sẽ lệch.

## Chạy local

Không cần cài đặt, không cần máy chủ. Mở file `.html` trực tiếp trong trình duyệt, hoặc chạy một máy chủ tĩnh đơn giản trong thư mục này, ví dụ:

```bash
python3 -m http.server 8000
```

rồi mở `http://localhost:8000/`.

## Giấy phép

Xem file `LICENSE` trong repo.
