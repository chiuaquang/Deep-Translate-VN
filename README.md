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
- Prompt dịch dùng cho Gemini và Index Translate, không dùng cho Google Translate. Prompt có thể sửa, lưu tên, tải lại, xóa, khôi phục mặc định. Prompt mặc định của Index và Gemini có thêm ý: dịch toàn bộ sang ngôn ngữ đích, không để lại bất kỳ chữ tiếng Nhật / Trung / Hàn nào trong bản dịch, kể cả tên nhân vật tiếng Nhật cũng phải dịch hoặc phiên âm sang ngôn ngữ đích, không dịch một nửa Việt một nửa Nhật.
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
- **Còn chữ tiếng Nhật chưa dịch:** khi ngôn ngữ đích đang chọn là tiếng Việt, bản dịch còn chữ tiếng Nhật / Trung / Hàn nguyên văn thì bị gắn cờ với lý do ghi rõ chữ còn lại, ví dụ **“Còn chữ tiếng Nhật chưa dịch: 錢湯”**. Sau khi dịch bằng Index, Gemini hay Google — từng dòng, theo Batch, dịch hàng loạt, dịch lại dòng lỗi — nếu bản dịch còn chữ Nhật / Trung / Hàn, tool tự dịch lại dòng đó đúng một lần với prompt mạnh hơn, yêu cầu dịch toàn bộ; nếu sau lần dịch lại vẫn còn sót thì giữ cờ Dịch lỗi để dịch lại tiếp bằng nút **Dịch lại dòng này** / **Dịch lại tất cả dòng Dịch lỗi**. Tool không bao giờ tự xóa chữ Nhật khỏi bản dịch mà không dịch, vì như vậy sẽ mất nghĩa. Ví dụ: `MapInfos.json` “錢湯内装” dịch thành “Nội thất錢湯”, `System.json` switches “錢湯終了” dịch thành “錢湯 kết thúc” và “村長錢湯登場” dịch thành “村長錢湯 xuất hiện” đều bị gắn cờ vì còn chữ Nhật chưa dịch.
- **Dịch sai ngôn ngữ đích:** khi ngôn ngữ đích đang chọn là tiếng Việt, bản dịch chứa một từ ngoại lai không phải tiếng Việt sau khi đã bỏ mã game, mã giữ chỗ và tên nhân vật đã bảo vệ — ví dụ từ tiếng Đức như `Physikal`, `Effekt`, phiên âm romaji như `Butsuri`, `Efekuto`, mẫu chữ tiếng Việt không có (`ff`, `ck`, `sch`, `kt`, `pf`, phụ âm đôi, `f`/`j`/`w`/`z`, `ph` + phụ âm, `k` ở cuối từ), hoặc ký tự tiếng Nhật / Trung / Hàn còn nguyên trong bản dịch. Lý do ghi rõ đúng từ bị nghi, ví dụ **“Dịch sai ngôn ngữ đích: Physikal”**. Ví dụ: `Animations.json` “打撃/物理” dịch thành “Đòn đánh/Physikal” (đúng phải là “Vật lý”) và “打撃/エフェクト” dịch thành “Đòn đánh/Effekt” (đúng phải là “Hiệu ứng”) đều bị gắn cờ. Tên riêng giữ nguyên như “Yuuto Makabe”, tên nhân vật đã bảo vệ, và thuật ngữ game quen thuộc như HP, MP, Save, Load thì không bị tính là sai.

Chip **Dịch lỗi (n)** lọc đúng các dòng lỗi. Dòng lỗi vẫn tính trong Đã dịch. Trong Khung dịch có hộp cảnh báo liệt kê đủ lý do, kèm nút **Xóa bản dịch lỗi**, nút **Dịch lại dòng này**, và nút **Đánh dấu dịch sai** cho lỗi về nghĩa mà máy không tự phát hiện được.

Nút **Dịch lại tất cả dòng Dịch lỗi** dịch lại đúng các dòng đang lỗi bằng model đang chọn. Nút **Sửa tất cả dòng Dịch lỗi** sửa mã/tag bằng code có sẵn, không dịch lại bằng model.

### Cảnh cáo khi xuất file

Khi bấm **Xuất file đã dịch** mà còn dòng chưa dịch, dòng Dịch lỗi, hoặc dòng Thiếu tag, tool hiện hộp cảnh cáo ghi rõ số dòng từng loại. Bấm **Vẫn xuất file** thì vẫn xuất đúng như cũ. Bấm **Quay lại sửa** thì không xuất gì và chuyển bộ lọc sang Dịch lỗi để sửa nhanh. Cảnh cáo này chỉ áp dụng cho xuất file game, không áp dụng cho Xuất CSV / Xuất TXT.

## Tên nhân vật không dịch (chỉ bản RPG Maker)

Tên nhân vật trong thoại **không được dịch thành nghĩa đen, không được làm mất** — nhưng chỉ với tên đã ở dạng chữ Latin hoặc đã ở dạng ngôn ngữ đích:

- Bảo vệ tên nhân vật chỉ áp dụng cho tên chữ Latin / dạng ngôn ngữ đích, ví dụ “Sakura”, “Yuuto Makabe”, “Mila”. Tên viết bằng chữ Nhật / Trung / Hàn, ví dụ “美琴”, “村長”, **không** nằm trong danh sách giữ nguyên. Khi thu thập tự động, các tên này được tách sang nhóm riêng **“Tên sẽ được dịch”** ngay trong khu “Tên nhân vật không dịch”, có nhãn “sẽ được dịch” và chip màu riêng, để gửi cho model dịch hoặc phiên âm sang ngôn ngữ đích — ví dụ “美琴” thành “Mikoto” hoặc “Mỹ Cầm” — không được để nguyên “美琴” trong bản dịch. Tên phiên âm đúng như “Mikoto” không bị gắn cờ Dịch lỗi oan. “Massage kết thúc” (từ “マッサージ終了”) vẫn bị tính là chưa dịch đúng, còn “Mát-xa kết thúc” thì không.
- Tool tự thu thập tên nhân vật của game đang mở từ `Actors.json` (trường `name` của từng Actor), tên người nói trong lệnh 101 (tham số 4), và các mã `\N[n]` trong thoại, rồi hiện trong khu **“Tên nhân vật không dịch”** ngay dưới khung dịch tự động.
- Anh có thể thêm tên bằng tay (ô nhập + nút Thêm), sửa tại chỗ, xóa từng tên, xóa tất cả. Danh sách chỉnh tay được lưu trong trình duyệt, khóa riêng theo tên game (`gameTitle`), tải lại đúng game thì còn, game khác thì có danh sách khác.
- Khi dịch bằng mọi đường — Google, Index, Gemini, từng dòng, theo lô, Dịch tất cả, dịch hàng loạt, dịch lại dòng lỗi — tên trong danh sách được thay bằng mã giữ chỗ riêng trước khi gửi cho model, rồi khôi phục nguyên vẹn đúng chính tả và viết hoa sau khi dịch. Prompt của Gemini và Index luôn có ý giữ nguyên tên nhân vật, không dịch tên riêng.
- Sau dịch, tool kiểm chứng: nếu bản dịch thiếu một tên mà bản gốc có, tool tự chèn lại tên đó vào vị trí tương ứng (đầu, giữa, cuối câu) và ghi log; nếu vẫn thiếu thì gắn cờ Dịch lỗi với lý do **“Thiếu tên nhân vật: \<tên\>”**.

Ví dụ: câu “hôm nay trời mưa phải không sakura” sau khi dịch vẫn còn nguyên “Sakura”, không thành “hoa anh đào”.

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
