# Hướng dẫn cho Codex

## Bản đồ dự án

- `TikTokBridge/`: Node.js 20+, Express, WebSocket, trang điều khiển trong `public/`, xử lý sự kiện trong `src/`, cấu hình JSON trong `config/`.
- `UnityProject/`: game Unity 6 viết bằng C#, phiên bản bắt buộc `6000.2.10f1`.
- `DJ_MUSIC/`, `DJ_VIDEO/`, `LiveAssets/`: tài nguyên người dùng; kiểm tra giấy phép trước khi thêm tài nguyên bên thứ ba.
- `run.bat`: cài dependency nếu cần, chạy Bridge và mở game/control panel. `build.bat`: build Windows player vào `Build/` bằng Unity đã cài.

## Luồng sự kiện

TikFinity hoặc TikTok Live Connector → chuẩn hóa tại `TikTokBridge/src/tiktok/` → luật Master và lệnh chat tại `src/master/rules.js` → kiểm tra dữ liệu tại `src/security.js` → WebSocket Bridge → `UnityProject/Assets/Scripts/TikTokWebSocketClient.cs` → `TikTokGameController`, `PlayerManager` và hiệu ứng game.

Bridge mặc định chạy trên `127.0.0.1:3000`; game dựng sẵn trông đợi cổng `3000`. Giữ `ALLOW_LAN=0` và bind loopback trừ khi yêu cầu cụ thể cần thay đổi cách triển khai.

## Quy ước thao tác

- Sửa Bridge trong `TikTokBridge/`; dùng `npm ci` khi cần cài dependency. Các lệnh hiện có là `npm test` và `npm run security:smoke`.
- Không chạy test hoặc build nếu người dùng chưa yêu cầu kiểm tra.
- Sửa Unity trong `UnityProject/Assets/`; giữ cặp file `.meta` và GUID ổn định. Đừng sửa dữ liệu Unity sinh ra trong `Library/`, `Temp/` hoặc `Build/` như source.
- Không thêm dependency Unity hoặc thay `package-lock.json` nếu công việc không yêu cầu.
- Với việc cần mở Editor, xem scene hoặc chỉnh asset, ưu tiên skill chính thức `unity-cli`/`unity-pipeline` và điều khiển qua CLI thay vì sửa tay YAML scene/prefab. Luôn ghim lệnh vào `UnityProject` và kiểm tra đúng version `6000.2.10f1`; điều khiển Editor đang mở cần package `com.unity.pipeline`.
- Bí mật cấu hình thuộc `.env` cục bộ; chỉ cập nhật `.env.example` khi thay đổi cấu hình cần được tài liệu hóa.
- Dùng skill `playwright` khi cần thao tác hoặc quan sát Control Panel trong trình duyệt. Dùng `security-best-practices` cho yêu cầu bảo mật rõ ràng.

Các agent chuyên stack nằm trong `.codex/agents/`.
