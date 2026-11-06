# Tool dịch RPG Maker MV/MZ — Deep Translate VN

File: `dich-rpg-maker-mv-mz.html`

Tool dịch game RPG Maker MV/MZ, **chỉ nhận file `.json`**, không nhận `.rpy`, không có KiriKiri.

## Trích xuất

Tool trích các dòng sau từ file `.json`:

- Thoại 401, văn bản cuộn 405, lựa chọn 102, tên người nói trong 101 (tham số 4), đổi tên 320 / 324 / 325.
- Tên, mô tả, biệt danh, hồ sơ từ Actors, Items, Skills, Weapons, Armors, Enemies, States, Classes, tin nhắn skill, `gameTitle`.
- 53 chuỗi `System.json → terms.messages`, loại **Thông điệp hệ thống**, ví dụ Always Dash, BGM Volume, "Which file would you like to save to?".
- Các mảng trong System: basic, commands, params, armor, element, equip, skill, weapon types, variables, switches.
- Chỉ một số lệnh plugin 357: `TorigoyaMZ_NotifyMessage` / `notify` / `parameters[3].message`, loại **Thông báo plugin**, và `DTextPicture` / `dText` / `parameters[3].text`, loại **Chuỗi hiển thị plugin**. Các trường 357 khác và toàn bộ lệnh 657 không được trích, giữ nguyên khi xuất.
- Tên file tài nguyên (asset) không được dịch.

Khi xuất, lựa chọn 402 được đồng bộ với lựa chọn 102 đã dịch ở cùng chỉ số.

## Dịch tự động

Xem phần Dịch tự động bằng Model trong `README.md`.

Riêng bản RPG Maker có thêm **Số luồng song song** cho Index Translate và Gemini Flash, mặc định 3, từ 1 đến 5. Google Translate dịch tuần tự.

## Giữ nguyên mã, phát hiện Dịch lỗi, thoại xuống dòng, Word wrap, tên game

Xem `README.md`, các phần:

- Giữ nguyên mã game
- Phát hiện Dịch lỗi (chỉ bản RPG Maker)
- Cảnh cáo khi xuất file
- Thoại xuống dòng và Word wrap (chỉ bản RPG Maker)
- Tên nhân vật không dịch (chỉ bản RPG Maker)
- Tên tiếng Nhật phải dịch hết, không dịch nửa vời (chỉ bản RPG Maker)
- Dịch sai ngôn ngữ đích
- Nhận biết tên game theo dự án (chỉ bản RPG Maker)

## Word wrap — hoàn tác tách theo từng game

Bản sao để hoàn tác Word wrap được khóa theo tên game (`gameTitle` trong `System.json`) + bộ file đang mở, giống như tiến độ dịch. Mở game mới thì ghi chú hoàn tác của game cũ (ví dụ 360 dòng, limit 46) không hiện nữa, nút **Xóa Word wrap đã áp dụng** mờ đi, và bấm nút đó ở game mới không làm gì cả, không khôi phục bản dịch của game cũ vào game mới. Quay lại đúng game cũ, đúng bộ file, thì bản sao của game cũ hiện lại và hoàn tác được đúng cho game đó. Tải riêng file trước rồi tải thêm `System.json` sau thì bản sao tạm được chuyển sang khóa theo tên game mới, không mất bản sao.

## Tên nhân vật không dịch

Bảo vệ tên nhân vật chỉ áp dụng cho tên đã ở dạng chữ Latin hoặc đã ở dạng ngôn ngữ đích. Tool tự thu thập tên nhân vật từ `Actors.json` (trường `name`), tên người nói trong lệnh 101 (tham số 4), và các mã `\N[n]` trong thoại, hiện trong khu **“Tên nhân vật không dịch”** dưới khung dịch tự động. Anh có thể thêm tên bằng tay, sửa, xóa từng tên, xóa tất cả; danh sách lưu trong trình duyệt, khóa riêng theo tên game.

Khi dịch bằng Google, Index hay Gemini, tên trong danh sách được thay bằng mã giữ chỗ riêng trước khi gửi cho model và khôi phục nguyên vẹn sau khi dịch, nên tên không bị dịch thành nghĩa đen — ví dụ “Sakura” không thành “hoa anh đào” — và không bị mất. Sau dịch, tool tự chèn lại tên còn thiếu vào đúng vị trí; nếu vẫn thiếu thì gắn cờ Dịch lỗi với lý do **“Thiếu tên nhân vật: \<tên\>”**.

Tên viết bằng chữ Nhật / Trung / Hàn, ví dụ “美琴”, “村長”, **không** nằm trong danh sách giữ nguyên mà được tách sang nhóm riêng **“Tên sẽ được dịch”** trong cùng khu đó, có nhãn “sẽ được dịch” và chip màu riêng, để gửi cho model dịch hoặc phiên âm sang ngôn ngữ đích — xem mục dưới.

- Lưu tiến độ 7 ngày

## Tên tiếng Nhật phải dịch hết, không dịch nửa vời

Prompt mặc định và prompt ép gửi cho Index Translate và Gemini (không dùng cho Google) có thêm ý: dịch toàn bộ sang ngôn ngữ đích, không để lại bất kỳ chữ tiếng Nhật / Trung / Hàn nào trong bản dịch, kể cả tên nhân vật tiếng Nhật cũng phải dịch hoặc phiên âm sang ngôn ngữ đích, không dịch một nửa Việt một nửa Nhật.

Sau khi dịch bằng Index, Gemini hay Google — từng dòng, theo Batch, dịch hàng loạt, dịch lại dòng lỗi — nếu bản dịch còn chữ Nhật / Trung / Hàn mà đích là tiếng Việt, tool tự dịch lại dòng đó đúng một lần với prompt mạnh hơn, yêu cầu dịch toàn bộ; nếu sau lần dịch lại vẫn còn sót thì giữ cờ Dịch lỗi với lý do **“Còn chữ tiếng Nhật chưa dịch: \<chữ còn lại\>”** (ví dụ “錢湯”) để dịch lại tiếp, và tool không bao giờ tự xóa chữ Nhật khỏi bản dịch mà không dịch.

Ví dụ các dòng từng bị dịch nửa vời: `MapInfos.json` “錢湯内装” → “Nội thất錢湯”, `System.json` switches “美琴EV1” → “美琴EV1” (tên “美琴” không dịch gì cả), “錢湯終了” → “錢湯 kết thúc”, “村長錢湯登場” → “村長錢湯 xuất hiện”, `Tilesets.json` “錢湯内装” → “Nội thất錢湯” — tất cả đều bị gắn cờ vì còn chữ Nhật chưa dịch. “マッサージ終了” dịch thành “Massage kết thúc” vẫn bị tính là chưa dịch đúng, còn “Mát-xa kết thúc” thì không. Tên phiên âm đúng như “Mikoto” (từ “美琴”, vẫn giữ “EV1”) không bị gắn cờ oan.

## Dịch sai ngôn ngữ đích

Khi ngôn ngữ đích đang chọn là tiếng Việt, một dòng đã dịch được tính là Dịch lỗi với lý do **“Dịch sai ngôn ngữ đích: \<từ\>”** nếu bản dịch chứa một từ ngoại lai không phải tiếng Việt sau khi đã bỏ mã game, mã giữ chỗ và tên nhân vật đã bảo vệ — ví dụ từ tiếng Đức như `Physikal`, `Effekt`, phiên âm romaji như `Butsuri`, `Efekuto`, mẫu chữ tiếng Việt không có (`ff`, `ck`, `sch`, `kt`, `pf`, phụ âm đôi, `f`/`j`/`w`/`z`, `ph` + phụ âm, `k` ở cuối từ), hoặc ký tự tiếng Nhật / Trung / Hàn còn nguyên trong bản dịch.

Ví dụ: `Animations.json` “打撃/物理” dịch thành “Đòn đánh/Physikal” (đúng phải là “Vật lý”) và “打撃/エフェクト” dịch thành “Đòn đánh/Effekt” (đúng phải là “Hiệu ứng”) đều bị gắn cờ Dịch lỗi và dịch lại được bằng nút **Dịch lại dòng này** / **Dịch lại tất cả dòng Dịch lỗi**. Tên riêng giữ nguyên như “Yuuto Makabe”, tên nhân vật đã bảo vệ, và thuật ngữ game quen thuộc như HP, MP, Save, Load thì không bị tính là sai.

Chữ Nhật / Trung / Hàn còn nguyên trong bản dịch có lý do riêng: **“Còn chữ tiếng Nhật chưa dịch: \<chữ còn lại\>”** — xem mục “Tên tiếng Nhật phải dịch hết, không dịch nửa vời” ở trên.

## Xuất / Nhập

- Một file: xuất đúng tên gốc, ví dụ `Map001.json`.
- Nhiều file: xuất thành `Deep Translate VN RPG.zip`, các file bên trong giữ đúng tên gốc.
- Xuất / Nhập CSV và TXT theo đúng định dạng trong `README.md`.
