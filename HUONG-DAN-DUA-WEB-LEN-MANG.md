# Đưa NoraStudies.com lên mạng

Hướng dẫn từng bước, từ máy của chị tới lúc gõ `norastudies.com` là ra trang.

Đường đi gồm bốn chặng: cài phần mềm, đẩy mã nguồn lên GitHub, bật GitHub Pages, rồi trỏ tên miền mua ở iNET về đó. Làm lần đầu mất khoảng một buổi. Từ lần thứ hai trở đi, mỗi lần sửa nội dung chỉ mất ba câu lệnh.

---

## Chặng 0 · Cài bốn thứ, làm một lần duy nhất

**Quarto.** Tải bản Windows tại <https://quarto.org/docs/get-started/>. Cài xong, mở PowerShell gõ `quarto --version`. Ra một dãy số là được.

**Git.** Tải tại <https://git-scm.com/download/win>. Trong lúc cài, mọi bước cứ để mặc định, bấm Next tới hết. Cài xong gõ `git --version`.

**Tài khoản GitHub.** Đăng ký tại <https://github.com>. Tên tài khoản nên gọn và dễ nhớ, ví dụ `norathai`, vì nó sẽ xuất hiện trong đường dẫn tạm của website.

**Khai báo tên cho Git.** Mở PowerShell, gõ hai dòng, thay bằng thông tin của chị:

```powershell
git config --global user.name "Nguyen Thai Ha"
git config --global user.email "nguyenthaiha1212@gmail.com"
```

---

## Chặng 1 · Chạy thử tại máy trước đã

Trước khi đưa lên mạng, xem thử ở máy cho chắc.

Mở PowerShell, chuyển vào thư mục dự án:

```powershell
cd "D:\Claude-Workspace\Website NoraStudies.com"
quarto preview
```

Trình duyệt tự mở ở địa chỉ `localhost:4444`. Chị bấm qua đủ mười trang, xem hoa có hiện không, các nút có bấm được không. Xong thì quay lại PowerShell bấm `Ctrl + C` để dừng.

Nếu preview chạy tốt, dựng bản chính thức:

```powershell
quarto render
```

Lệnh này tạo ra thư mục `_site`, bên trong là toàn bộ website ở dạng HTML tĩnh. Đây chính là thứ sẽ được đưa lên mạng.

---

## Chặng 2 · Đẩy mã nguồn lên GitHub

### 2.1 Tạo kho chứa trên GitHub

Vào <https://github.com/new> và điền:

| Ô | Điền gì |
|---|---|
| Repository name | `norastudies` |
| Mô tả | Website NoraStudies |
| Public hay Private | Chọn **Public**. GitHub Pages ở gói miễn phí chỉ chạy với kho công khai |
| Add a README file | Bỏ trống, không tick |
| Add .gitignore | Bỏ trống |
| Choose a license | Bỏ trống |

Bấm **Create repository**. GitHub hiện ra một trang có sẵn vài câu lệnh, chị cứ để đó, phần dưới đây sẽ dùng tới.

### 2.2 Tạo file .gitignore

Trong thư mục dự án, tạo một file tên `.gitignore` (có dấu chấm ở đầu), nội dung:

```
_site/
.quarto/
*.docx
~$*
```

File này báo cho Git biết đừng đẩy thư mục `_site` lên, vì GitHub sẽ tự dựng lại. Nó cũng loại các file Word và file tạm của Word ra khỏi kho.

### 2.3 Đẩy lần đầu

Trong PowerShell, vẫn đứng ở thư mục dự án:

```powershell
git init
git add .
git commit -m "Khoi tao website NoraStudies"
git branch -M main
git remote add origin https://github.com/TEN-TAI-KHOAN/norastudies.git
git push -u origin main
```

Thay `TEN-TAI-KHOAN` bằng tên tài khoản GitHub của chị.

Lần đầu chạy `git push`, một cửa sổ hiện lên bảo chị đăng nhập GitHub. Chọn **Sign in with your browser**, đăng nhập, cho phép, rồi quay lại PowerShell. Từ lần sau nó nhớ luôn.

Đẩy xong, mở lại trang kho trên GitHub, chị sẽ thấy toàn bộ file nằm ở đó.

### 2.4 Từ lần thứ hai trở đi

Mỗi lần sửa nội dung, chỉ cần ba dòng:

```powershell
git add .
git commit -m "Sua noi dung trang hoc phi"
git push
```

Câu trong ngoặc kép là ghi chú cho chính chị, viết gì cũng được, nhưng nên viết không dấu để tránh lỗi hiển thị trong lịch sử.

---

## Chặng 3 · Bật GitHub Pages và cho nó tự dựng

GitHub sẽ tự chạy Quarto mỗi lần chị đẩy mã nguồn lên. Chị không phải tự dựng và đẩy thư mục `_site` nữa.

### 3.1 Tạo file workflow

Trong thư mục dự án, tạo đường dẫn thư mục `.github/workflows/` rồi tạo file `publish.yml` bên trong, với nội dung:

```yaml
name: Dung va dang website

on:
  push:
    branches: [main]
  workflow_dispatch:

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: pages
  cancel-in-progress: false

jobs:
  dung:
    runs-on: ubuntu-latest
    steps:
      - name: Lay ma nguon
        uses: actions/checkout@v4

      - name: Cai Quarto
        uses: quarto-dev/quarto-actions/setup@v2

      - name: Dung website
        run: quarto render

      - name: Chuan bi dang
        uses: actions/configure-pages@v5

      - name: Dong goi
        uses: actions/upload-pages-artifact@v3
        with:
          path: _site

  dang:
    needs: dung
    runs-on: ubuntu-latest
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    steps:
      - name: Dang len GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

### 3.2 Bật Pages trong cài đặt kho

Vào kho trên GitHub, chọn tab **Settings**, cột trái chọn **Pages**.

Ở mục **Build and deployment**, phần **Source**, chọn **GitHub Actions**. Không chọn *Deploy from a branch*.

### 3.3 Đẩy file workflow lên

```powershell
git add .
git commit -m "Them workflow dung website"
git push
```

Vào tab **Actions** của kho, chị sẽ thấy một tiến trình đang chạy. Chấm vàng là đang dựng, dấu tích xanh là xong, dấu chéo đỏ là lỗi. Lần đầu mất khoảng ba tới năm phút.

Xong, quay lại **Settings → Pages**, chị thấy một dòng báo website đang chạy tại `https://TEN-TAI-KHOAN.github.io/norastudies/`. Bấm thử vào xem.

::: Lưu ý
Ở giai đoạn này, vì website nằm trong thư mục con `/norastudies/`, một số ảnh có thể chưa hiện, do các đường dẫn trong `styles.scss` đều bắt đầu bằng dấu gạch chéo và đang trỏ về gốc tên miền. Chuyện này tự hết sau khi chị gắn tên miền riêng ở chặng 4, vì lúc đó website nằm ngay ở gốc `norastudies.com`. Chị đừng sửa gì cả, cứ đi tiếp.
:::

---

## Chặng 4 · Trỏ tên miền iNET về GitHub

### 4.1 Khai báo tên miền với GitHub

Trong thư mục dự án, tạo một file tên `CNAME` (viết hoa, không có phần mở rộng), nội dung đúng một dòng:

```
norastudies.com
```

Rồi mở `_quarto.yml`, tìm mục `resources` và thêm `CNAME` vào để Quarto chép file này sang `_site` mỗi lần dựng:

```yaml
project:
  type: website
  output-dir: _site
  resources:
    - assets/
    - CNAME
```

Đẩy lên:

```powershell
git add .
git commit -m "Them ten mien rieng"
git push
```

### 4.2 Khai báo DNS ở iNET

Đăng nhập <https://inet.vn>, vào mục quản lý tên miền, chọn `norastudies.com`, rồi tìm phần **Quản lý DNS** hoặc **Bản ghi DNS**.

Trước khi thêm mới, xoá các bản ghi A và CNAME đang có sẵn cho `@` và `www`. iNET thường đặt sẵn một bản ghi trỏ về trang giữ chỗ của họ, để nguyên thì sẽ xung đột.

Sau đó thêm đúng năm bản ghi này:

| Loại | Tên (Host) | Giá trị (Value) | TTL |
|---|---|---|---|
| A | `@` | `185.199.108.153` | để mặc định |
| A | `@` | `185.199.109.153` | để mặc định |
| A | `@` | `185.199.110.153` | để mặc định |
| A | `@` | `185.199.111.153` | để mặc định |
| CNAME | `www` | `TEN-TAI-KHOAN.github.io.` | để mặc định |

Bốn bản ghi A là bốn máy chủ của GitHub, cần đủ cả bốn để website vẫn chạy khi một máy gặp sự cố. Bản ghi CNAME làm cho `www.norastudies.com` cũng vào được.

Chú ý dấu chấm ở cuối giá trị CNAME. Một số bảng điều khiển yêu cầu có, một số tự thêm. Nếu iNET báo lỗi định dạng, thử bỏ dấu chấm cuối đi.

Nếu ô Tên không nhận ký tự `@`, chị để trống ô đó, hoặc gõ thẳng `norastudies.com`, tuỳ cách iNET đặt.

### 4.3 Khai báo tên miền trong GitHub

Quay lại kho GitHub, vào **Settings → Pages**, mục **Custom domain**, gõ `norastudies.com` rồi bấm **Save**.

GitHub sẽ kiểm tra DNS. Nếu iNET chưa cập nhật kịp, nó báo lỗi. Chị chờ rồi bấm Save lại.

### 4.4 Chờ DNS lan ra

Thay đổi DNS cần thời gian để lan tới các máy chủ trên thế giới. Thường trong vòng một tiếng, chậm nhất là hai mươi tư tiếng.

Kiểm tra bằng cách vào <https://dnschecker.org>, gõ `norastudies.com`, chọn loại bản ghi **A**. Khi nào phần lớn các cột hiện ra bốn địa chỉ IP ở trên là xong.

### 4.5 Bật HTTPS

Sau khi DNS đã lan xong, quay lại **Settings → Pages**, chị sẽ thấy ô **Enforce HTTPS** không còn bị mờ nữa. Tick vào ô đó.

GitHub tự xin và gia hạn chứng chỉ bảo mật, chị không phải trả tiền và cũng không phải làm gì thêm. Nếu ô vẫn mờ, nghĩa là DNS chưa lan xong, chờ thêm rồi quay lại.

---

## Xong rồi thì kiểm lại năm chỗ này

- [ ] `https://norastudies.com` mở được và hiện đúng trang chủ
- [ ] `https://www.norastudies.com` cũng mở được, và tự chuyển về địa chỉ không có `www`
- [ ] Thanh địa chỉ hiện ổ khoá, không báo trang không an toàn
- [ ] Hoa anh túc ở khối mở đầu và biểu tượng mạng xã hội ở chân trang đều hiện
- [ ] Mở trên điện thoại, chữ đọc được và nút Zalo hiện ở góc dưới bên phải

---

## Quy trình sửa nội dung hằng ngày

Từ giờ, mỗi lần chị muốn sửa một câu chữ trên website:

```powershell
cd "D:\Claude-Workspace\Website NoraStudies.com"
quarto preview
```

Sửa file `.qmd` cần sửa, xem trên trình duyệt cho ưng, bấm `Ctrl + C` để dừng preview, rồi:

```powershell
git add .
git commit -m "Sua gia goi G3"
git push
```

Khoảng ba tới năm phút sau, website ngoài mạng tự cập nhật. Chị theo dõi tiến trình ở tab **Actions** của kho.

---

## Khi gặp trục trặc

**Tab Actions hiện dấu chéo đỏ.** Bấm vào tiến trình đó, mở bước bị đỏ ra đọc dòng cuối cùng. Thường là lỗi cú pháp trong một file `.qmd`, và nó ghi rõ tên file cùng số dòng.

**Website chạy nhưng mất hết hoa và màu.** Kiểm tra lại `styles.scss`, mọi đường dẫn ảnh phải bắt đầu bằng dấu gạch chéo, dạng `url("/assets/...")`. Đây là lỗi hay gặp nhất.

**Gõ norastudies.com ra trang giữ chỗ của iNET.** Còn sót một bản ghi DNS cũ chưa xoá. Quay lại bảng DNS, xoá hết bản ghi A và CNAME nào không nằm trong bảng ở mục 4.2.

**GitHub báo tên miền đã bị dùng bởi kho khác.** Vào kho cũ, xoá tên miền ở phần Custom domain của kho đó trước, rồi mới khai báo lại ở kho mới.

**Ô Enforce HTTPS vẫn mờ sau một ngày.** Xoá tên miền khỏi ô Custom domain, bấm Save, chờ một phút, rồi gõ lại và Save lần nữa. Thao tác này buộc GitHub xin lại chứng chỉ.

---

## Hai điều nên làm sau khi website đã chạy

**Khai báo với Google.** Vào <https://search.google.com/search-console>, thêm `norastudies.com`, xác minh quyền sở hữu bằng bản ghi TXT mà Google đưa (thêm bản ghi đó vào DNS ở iNET giống cách làm ở mục 4.2). Sau đó nộp đường dẫn `https://norastudies.com/sitemap.xml`, Quarto tự sinh file này mỗi lần dựng.

**Bật thống kê truy cập.** Tạo tài khoản Google Analytics 4, lấy mã dạng `G-XXXXXXXXXX`, rồi bỏ dấu thăng ở dòng `google-analytics` trong `_quarto.yml` và điền mã vào.

Nguồn tra cứu cho phần DNS: [tài liệu chính thức của GitHub về tên miền riêng](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/managing-a-custom-domain-for-your-github-pages-site).
