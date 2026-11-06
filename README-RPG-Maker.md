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
- Nhận biết tên game theo dự án (chỉ bản RPG Maker)
- Lưu tiến độ 7 ngày

## Xuất / Nhập

- Một file: xuất đúng tên gốc, ví dụ `Map001.json`.
- Nhiều file: xuất thành `Deep Translate VN RPG.zip`, các file bên trong giữ đúng tên gốc.
- Xuất / Nhập CSV và TXT theo đúng định dạng trong `README.md`.
