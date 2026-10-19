# AnLai Repo

APT repo cho Sileo / Zebra, phát OmniCar (và các tweak khác). Chạy trên GitHub Pages, mỗi lần push lên `main`
GitHub Actions tự dựng lại `Packages` / `Release` và xuất bản.

URL người dùng thêm: **https://anlai97.github.io/OmniCarRepo/** (đổi trong `repo.conf`).

## Cấu trúc

| Đường dẫn | Nội dung |
|---|---|
| `debs/` | Bỏ file `.deb` vào đây. Giữ vài bản cũ nếu muốn người dùng hạ cấp được. |
| `static/` | Chép nguyên lên web: trang chủ, `CydiaIcon.png`, `sileo-featured.json`, depiction. |
| `static/depictions/<package>/` | `sileo.json` (depiction gốc của Sileo), `index.html` (Zebra / trình duyệt), `icon.png`, `header.png`, `banner.png`, `changelog.md`. |
| `scripts/build.py` | Dựng repo vào `_site/` (chỉ cần Python 3). |
| `repo.conf` | URL gốc, Origin / Label / mô tả của repo. |

`build.py` tự thêm vào `Packages` các trường `Depiction`, `SileoDepiction`, `Icon`, `Header` trỏ về
`depictions/<package>/` (thay `Icon: file:///var/jb/...` trong control, vì đường đó chỉ có sau khi cài).
Trong depiction: `__BASE__` = URL gốc, `__VERSION__` = bản mới nhất trong `debs/`, `__CHANGELOG__` = `changelog.md`.

## Lần đầu

1. Tạo repo **public** `AnLai97/OmniCarRepo` trên GitHub (tên repo quyết định URL `anlai97.github.io/OmniCarRepo/`).
2. `git remote add origin git@github.com:AnLai97/OmniCarRepo.git && git push -u origin main`
3. GitHub > repo > **Settings > Pages > Source: GitHub Actions**. Chạy lại workflow **Publish repo** nếu lần đầu bị lỗi.
4. Mở `https://anlai97.github.io/OmniCarRepo/` trên iPhone > **Add to Sileo**.

## Phát hành bản mới

1. Lấy `.deb` từ GitHub Release của OmniCar (tag `v*`), hoặc từ artifact của CI.
2. Chép vào `debs/`, thêm mục mới ở đầu `static/depictions/com.anlai.omnicar/changelog.md`.
3. Commit + push. Khoảng 1 phút sau Sileo bấm làm mới là thấy bản mới.

Thử trên máy trước khi push: `python scripts/build.py` rồi xem `_site/Packages`.

## Tuỳ chọn

- **Ký Release**: tạo khoá GPG riêng cho repo, thêm secret `GPG_PRIVATE_KEY` (armored) và `GPG_PASSPHRASE`.
  Workflow tự ký `InRelease` + `Release.gpg`. Sileo / Zebra chạy được cả khi không ký.
- **Tên miền riêng** (`repo.<tên miền>`): tạo `static/CNAME` chứa tên miền, đổi `BASE_URL` trong `repo.conf`,
  trỏ DNS CNAME về `anlai97.github.io`. Muốn đếm lượt tải thì đặt sau Cloudflare.
- **Nhánh beta**: tạo repo thứ hai (ví dụ `AnLai97/OmniCarRepo-beta`) dùng chung cấu trúc này.
