# Mi TV nội địa Trung Quốc (loại 65–75 inch) – hướng dẫn nhanh

Repo này là bản sao của [Gathin23/cleanyourtv](https://github.com/Gathin23/cleanyourtv) (giấy phép MIT,
xem [LICENSE](LICENSE)). Tài liệu đầy đủ bằng tiếng Việt: [README.vi.md](README.vi.md). Trang này chỉ gom
những gì cần cho **TV Xiaomi / Redmi bản nội địa (firmware CN, không có Google)**.

> Xiaomi không có mẫu 68 inch; TV "68 inch" thường là bản 65" (hoặc 70"/75"). Hãy xem đúng **mã máy**
> trước: Cài đặt → 关于 / Giới thiệu → 型号 (Model), dạng `MiTV-XXXX0` hoặc `L65M…`.

## 1. Chọn danh sách gần đúng với TV của bạn

| Mã máy | TV | Hệ điều hành | Danh sách |
|---|---|---|---|
| MiTV-ANSP0 | 小米电视5 Pro 65" | Android TV 9 (CN) | [examples/xiaomi-mitv-ansp0](examples/xiaomi-mitv-ansp0/debloat.txt) |
| MiTV-MTEQ0 | TV EA Pro 55" (L55MA-EP) | Android TV 10 (CN) | [examples/xiaomi-mitv-mteq0](examples/xiaomi-mitv-mteq0/debloat.txt) |
| MiTV-ASTP0 / ASTP1 | L55M7-EA / maverick | Android TV 9 (CN) | [astp0](examples/xiaomi-mitv-astp0/debloat.txt), [astp1](examples/xiaomi-mitv-astp1/debloat.txt) |
| MiTV-MFTR0 | freeguy | HyperOS TV 2 (Android 11) | [examples/xiaomi-mitv-mftr0](examples/xiaomi-mitv-mftr0/debloat.txt) |
| MiTV-ASTR0 | Redmi A Pro 55" 2025 | HyperOS 3 (Android 14) | [examples/xiaomi-mitv-astr0](examples/xiaomi-mitv-astr0/debloat.txt) |
| MiTV-MFFU0 | TV S Pro 100 Mini LED 2025 | HyperOS 3 (Android 14) | [examples/xiaomi-mitv-mffu0](examples/xiaomi-mitv-mffu0/debloat.txt) |
| MiTV-MFTP0 | Redmi (alita) | Android TV 9 (CN) | [examples/redmi-mitv-mftp0](examples/redmi-mitv-mftp0/debloat.txt) |
| MiTV-ANTL0 | Redmi (starwars) | Android 5.1.1 | [examples/redmi-mitv-antl0](examples/redmi-mitv-antl0/debloat.txt) – dùng `pm hide` |
| MiTV4-ANSM0 | Mi TV 4S 75" | Android 6 | [docs](docs/devices/xiaomi-mitv4-ansm0.md) – không tắt được gói nào |

Nếu mã máy không có trong bảng, **đừng áp nguyên danh sách khác**; dùng [PROMPT.md](PROMPT.md) để trợ lý
kiểm tra TV thật rồi lập danh sách riêng. Bảng đầy đủ: [examples/README.md](examples/README.md).

## 2. Bật ADB trên TV nội địa

1. Cài đặt → 关于 (Giới thiệu) → bấm OK 7 lần vào 产品型号 / 版本 để bật chế độ nhà phát triển.
2. Cài đặt → 账号与安全 (Tài khoản & bảo mật) → bật **ADB 调试**.
3. Trên máy tính: `adb connect <ip-tv>:5555`, rồi chọn **允许 / Cho phép** trên TV.

Trên hầu hết máy CN, **ADB tự tắt sau mỗi lần khởi động lại** – phải bật lại ở bước 2.

## 3. Áp dụng và hoàn tác

```sh
# Sao lưu + tắt (không gỡ cài đặt)
TV_HOST=<ip-tv> tools/disable.sh examples/<thu-muc-tv>/debloat.txt
# Hoàn tác
TV_HOST=<ip-tv> tools/restore.sh examples/<thu-muc-tv>/debloat.txt com.mitv.tvhome
```

Mở file `debloat.txt` trước và thêm `#` vào dòng nào bạn muốn giữ (ví dụ 投屏 / truyền màn hình, 小爱同学).

## 4. Lưu ý riêng cho Mi TV nội địa

- **Không tắt `com.mitv.tvhome` (PatchWall)** trên Android TV 9/10 CN: TV sẽ ra màn hình đen
  (FallbackHome). Giữ nó bật và đặt launcher mới làm Home mặc định.
- Nhiều máy chỉ chấp nhận Home có tên gói trong danh sách trắng: build launcher với
  `-PlauncherApplicationId=com.ts.launcher` (xem [docs/devices/xiaomi-mitv-aetp0-dofus.md](docs/devices/xiaomi-mitv-aetp0-dofus.md)).
  MFFU0 cần `com.thunderstone.idun.dev`.
- Quảng cáo khi khởi động: thử `settings put system boot_ad_switch 0`.
- Wi‑Fi 5 GHz bị ẩn kênh 100–144 vì vùng mặc định là CN:
  `settings put global service_region VN`, rồi tắt/bật Wi‑Fi.
- Không có Google: YouTube chính thức không chạy, dùng SmartTube. Bàn phím duy nhất thường là Sogou.
- Không bao giờ tắt `mitv.service`, ứng dụng TV/HDMI (`com.xiaomi.mitv.tvplayer`, `com.droidlogic.tvinput`)
  hay `com.xiaomi.mitv.smartpanel` (bảng nút Menu trên HyperOS).
