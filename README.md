# NoraStudies.com — hướng dẫn dựng và đưa lên mạng

Site tĩnh dựng bằng [Quarto](https://quarto.org). Chị viết nội dung bằng Markdown, Quarto render ra HTML.

## Cài một lần

1. **Cài Quarto**: tải bản Windows tại <https://quarto.org/docs/get-started/>. Kiểm tra bằng cách mở terminal gõ `quarto --version`.
2. **Cài Git** và tạo tài khoản GitHub nếu chưa có.

## Chạy thử tại máy

```bash
quarto preview
```

Trình duyệt mở ở `localhost:4444` và tự tải lại mỗi khi chị lưu file. Đây là việc nên làm đầu tiên, trước khi sửa bất cứ thứ gì.

Muốn xem site trông thế nào trên điện thoại: bấm F12 trong trình duyệt, chọn biểu tượng điện thoại ở góc trên bên trái của bảng vừa mở.

## Cấu trúc

| File | Nội dung |
|---|---|
| `_quarto.yml` | Cấu hình chung: tên site, menu, chân trang, ảnh xem trước |
| `index.qmd` | Trang chủ |
| `lo-trinh.qmd` | Năm chặng của luận án, có neo `#g0` tới `#g4` |
| `bai-bao.qmd` | Mentor và soát bản thảo bài báo Scopus Q1, Q2. Học phí trao đổi riêng |
| `hoc-phi.qmd` | Bảng giá, gói lẻ, combo, học nhóm ba người |
| `ve-nora.qmd` | Trang cá nhân, tách riêng khỏi phần bán hàng. Có sẵn ba khung nhúng video YouTube |
| `cong-bo.qmd` | Danh mục 16 công trình |
| `liem-chinh.qmd` | Cách làm việc, điều hứa với học viên, và những chỗ giới thiệu sang mentor khác |
| `hoi-dap.qmd` | Câu hỏi thường gặp |
| `lien-he.qmd` | Hẹn buổi nói chuyện 30 phút |
| `bai-viet.qmd` | Trang bài viết, hiện đang trống, đã liệt kê chủ đề nên viết trước |
| `styles.scss` | Toàn bộ màu sắc, phông chữ, bố cục, chuyển động |
| `head.html` | Dữ liệu có cấu trúc giúp Google hiểu đây là dịch vụ gì, do ai làm |
| `scripts.html` | Nút Zalo và WhatsApp nổi, cùng đoạn mã chạy hiệu ứng đếm số ở trang chủ |
| `tagline.html` | Dải khẩu hiệu chạy ngang ở đỉnh mọi trang |
| `_hoa-no.qmd` | Cụm ba bông anh túc nở, dạng SVG có hiệu ứng. Chèn bằng `{{< include _hoa-no.qmd >}}` |
| `_xa-hoi.qmd` | Bảy biểu tượng mạng xã hội dạng tròn, không kèm chữ |
| `_xa-hoi-chu.qmd` | Tám thẻ mạng xã hội kèm tên kênh và tài khoản |
| `_cam-ket.qmd` | Khối cam kết về việc không viết thay, gắn ở cuối cả mười trang |
| `lo/index.qmd` | Trang đích tiếng Lào |
| `assets/` | Logo, favicon, biểu tượng chặng, hình minh hoạ, ảnh xem trước |
| `NoraStudies logo design/` | Kho gốc bộ nhận diện, không dùng trực tiếp trên site |

## Những chỗ cần điền trước khi công khai

Tìm bằng lệnh:

```bash
grep -rn "CẦN ĐIỀN" *.qmd
```

Hiện có ba chỗ, đều nằm ở `lien-he.qmd`: đường dẫn TikTok, YouTube và Facebook Page. Điền sau khi lập kênh.

Ngoài ra cần rà lại:

- Bảng giá trong `hoc-phi.qmd` và trong `head.html`. Hai chỗ này phải khớp nhau, nếu sửa thì sửa cả hai
- Số học viên tối đa nhận cùng lúc, hiện đang viết chung chung ở `hoc-phi.qmd` và `hoi-dap.qmd`
- Số tài khoản ngân hàng, nếu muốn in mã QR lên trang học phí
- Chính sách xuất hoá đơn, nếu học viên được cơ quan chi trả

## Đưa lên mạng

Toàn bộ quy trình từ máy tới lúc `norastudies.com` chạy được nằm trong file riêng: **HUONG-DAN-DUA-WEB-LEN-MANG.md**. Ba file hạ tầng đã tạo sẵn trong thư mục dự án:

| File | Việc của nó |
|---|---|
| `.gitignore` | Không đẩy `_site`, `.quarto` và file Word lên GitHub |
| `CNAME` | Khai báo `norastudies.com` với GitHub Pages |
| `.github/workflows/publish.yml` | GitHub tự chạy Quarto và đăng website mỗi lần chị đẩy mã nguồn |

Tóm tắt cũ, giữ lại để tra nhanh:

```bash
git init
git add .
git commit -m "Khoi tao website NoraStudies"
git branch -M main
git remote add origin https://github.com/<tên-github>/norastudies.git
git push -u origin main
```

Vào repo trên GitHub, chọn **Settings → Pages**, mục *Source* chọn **GitHub Actions**, rồi thêm workflow dựng Quarto theo hướng dẫn tại <https://quarto.org/docs/publishing/github-pages.html>.

## Gắn tên miền NoraStudies.com

1. Tạo tài khoản Cloudflare miễn phí, thêm `norastudies.com` vào, Cloudflare cho hai địa chỉ nameserver
2. Vào trang quản trị nơi chị mua tên miền, đổi nameserver sang hai địa chỉ đó
3. Trong Cloudflare, mục DNS, thêm bản ghi `CNAME` trỏ `@` và `www` về `<tên-github>.github.io`, bật cờ proxy màu cam
4. Trong repo, **Settings → Pages → Custom domain**, nhập `norastudies.com`, tick *Enforce HTTPS*

`site-url` trong `_quarto.yml` đã để sẵn `https://norastudies.com` nên không cần sửa.

## Bật thống kê truy cập

Sau khi tạo tài khoản Google Analytics 4, lấy mã dạng `G-XXXXXXXXXX` rồi bỏ dấu thăng ở dòng `google-analytics` trong `_quarto.yml`.

Để biết kênh nào ra khách, khi đăng liên kết lên các kênh vệ tinh thì thêm đuôi đánh dấu:

```
https://norastudies.com/?utm_source=tiktok
https://norastudies.com/?utm_source=youtube
https://norastudies.com/?utm_source=facebook
```

## Cách trình bày nội dung

Toàn site dùng bố cục hàng ngang, không dùng khối vuông có viền. Mỗi mục là một hàng: nhãn hoặc biểu tượng nằm ở cột trái rộng 200px, tiêu đề và phần diễn giải nằm ở cột phải, hai hàng ngăn nhau bằng một đường kẻ mảnh. Trên điện thoại thì tự xếp lại thành một cột.

Ba lớp dùng chung bố cục này: `.card` cho các mục nội dung, `.price` cho bảng học phí, `.stage` cho năm chặng lộ trình.

Một lưu ý kỹ thuật khi sửa CSS: Pandoc bọc mọi nhãn dạng `[Nhãn]{.card-note}` vào trong một thẻ đoạn văn, nên bộ chọn phải viết là `.card > p:has(.card-note)` chứ không phải `.card > .card-note`. Viết sai chỗ này thì nhãn không nhảy sang cột trái được, mà nhìn qua rất khó nhận ra.

Khối `.note` chỉ còn một vạch đỏ bên trái, không nền và không bo góc.

## Quy tắc thiết kế cần giữ

Toàn bộ màu và cỡ chữ nằm ở đầu file `styles.scss`. Nếu cần đổi, sửa ở đó chứ đừng sửa rải rác trong các quy tắc bên dưới.

- Đỏ anh túc `#D1352B` là màu của **nền nút** và chữ cỡ lớn
- Đỏ thẫm `#A82419` là màu của **chữ**. Nhầm hai cái này làm chữ mờ trên màn hình điện thoại
- Giãn dòng thân bài không được xuống dưới 1,65, vì dấu tiếng Việt sẽ chạm chân chữ dòng trên
- Font lấy từ Google Fonts kèm tham số `subset=vietnamese`. Thiếu tham số này là lỗi font

Chi tiết đầy đủ nằm trong file `BO-NHAN-DIEN-THUONG-HIEU_NoraStudies_Claude-Design.docx`.

## Nhúng video YouTube

Mở video trên YouTube, nhìn thanh địa chỉ và lấy phần sau `v=`:

```
https://www.youtube.com/watch?v=dQw4w9WgXcQ
                               ^^^^^^^^^^^ mã video
```

Trong `ve-nora.qmd` và `bai-viet.qmd` đã có sẵn các khối giữ chỗ. Thay khối `::: {.video .video-cho} ... :::` bằng đoạn này, đổi `MÃ_VIDEO` thành mã vừa lấy:

```
::: {.video}
<iframe src="https://www.youtube-nocookie.com/embed/MÃ_VIDEO"
        title="Tên video"
        allow="accelerometer; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
        allowfullscreen loading="lazy"></iframe>
:::
[Chú thích dưới video]{.video-caption}
```

Dùng `youtube-nocookie.com` thay vì `youtube.com` thì YouTube không đặt cookie theo dõi cho tới khi người xem bấm play. Việc này giúp trang nhẹ hơn về mặt quyền riêng tư.

## Hoa và màu trong nội dung

Bộ hoa nằm ở `assets/hoa-*.svg`, sinh ra từ đúng hình cánh anh túc của logo nên không lệch nhận diện.

- `hoa-cum.svg` là luống hoa nhiều màu mọc lên ở góc phải khối mở đầu mọi trang
- `hoa-ngan.svg` là dải ba bông ngăn giữa các mục. Chèn bằng cách viết `::: {.hoa}` rồi `:::` trong file `.qmd`
- `hoa-do`, `hoa-vang`, `hoa-man`, `hoa-la`, `hoa-xanh` là bông nhỏ đứng trước mỗi tiêu đề mục, tự đổi màu xoay vòng theo thứ tự mục

Ngoài đỏ anh túc còn bốn màu phụ, đều đã kiểm tra tương phản đạt chuẩn WCAG AA trên nền trắng ngà: hổ phách `#8F5808`, lá đậm `#3F5C36`, mận `#5E3454`, xanh mực `#2A4A6B`.

Đổi màu một thẻ bằng cách thêm lớp `.sac-vang`, `.sac-la`, `.sac-man`, `.sac-xanh` hoặc `.sac-do` vào `::: {.card}` hay `::: {.price}`. Tô một cụm chữ giữa đoạn bằng `<span class="hl-man">chữ cần tô</span>`, đổi `hl-man` thành `hl-do`, `hl-vang`, `hl-la` hoặc `hl-xanh` tuỳ màu.

Lưu ý kỹ thuật: mọi đường dẫn ảnh trong `styles.scss` phải bắt đầu bằng dấu gạch chéo, dạng `url("/assets/...")`. Quarto gộp SCSS vào `_site/site_libs/bootstrap/`, nên đường dẫn tương đối sẽ trỏ sai và ảnh không hiện.

## Khối cam kết ở cuối mọi trang

Nằm ở `_cam-ket.qmd`, chèn vào cuối cả chín trang tiếng Việt bằng `{{< include _cam-ket.qmd >}}`. Trang tiếng Lào có bản riêng viết thẳng trong file, kèm một đoạn tiếng Anh.

Nội dung nói rõ mentor là người bạn đường và người chỉ hướng, không nhận viết thay bất cứ phần nào, và nếu ai tìm tới với ý định nhờ viết rồi đứng tên thì cuộc trao đổi dừng ở đó. Sửa một lần trong `_cam-ket.qmd` là cả chín trang cùng đổi.

Chỗ này mình viết bằng giọng từ chối lịch sự thay vì nói thẳng là chặn liên lạc, để giữ đúng phong cách nhã nhặn của toàn site. Muốn cứng rắn hơn thì sửa thẳng trong file.

## Bộ quy tắc văn phong

Skill `human` đã được cập nhật thêm nhóm F về lễ độ và tư cách người viết. Nhóm này ghi lại đúng những lỗi đã từng mắc: từ ngữ hạ thấp con người, giọng tự mãn, câu cụt lủn kiểu khẩu hiệu, và cụm rút gọn làm mất nghĩa. Mỗi lần viết nội dung mới cho site, gọi `/human` để bộ quy tắc đó được nạp.

## Trang tiếng Lào

Nằm ở `lo/index.qmd`, chạy tại địa chỉ `norastudies.com/lo/`. Một trang duy nhất gồm giới thiệu, phạm vi giúp được, năm chặng, bảng giá và cách liên hệ. Menu có nút `ລາວ` để chuyển qua, và trang Lào có nút quay lại tiếng Việt.

Chữ Lào lấy từ font Noto Sans Lao, xếp ngay sau Be Vietnam Pro trong `styles.scss`. Trình duyệt tự lấy chữ Việt từ font đầu và chữ Lào từ font sau.

**Chưa có người bản ngữ soát bản dịch.** Trên trang có sẵn một dòng nói rõ điều đó, kèm lời mời nhắn bằng tiếng Anh hoặc tiếng Việt. Khi nào tìm được người soát, sửa xong thì xoá dòng đó đi.

Bảng giá trên trang Lào phải khớp với `hoc-phi.qmd`. Sửa giá thì nhớ sửa cả ba chỗ: `hoc-phi.qmd`, `lo/index.qmd` và `head.html`.

## Sáu kênh chính thức

Địa chỉ đã gắn vào chân trang, trang Liên hệ và trang Về mình. Đổi ở đâu thì phải đổi cả ba chỗ:

| Kênh | Địa chỉ |
|---|---|
| Website | norastudies.com |
| Instagram | instagram.com/norathai_studies |
| Threads | threads.com/@norathai_studies |
| Facebook | facebook.com/norastudies |
| TikTok | tiktok.com/@norathai_studies |
| YouTube | youtube.com/@norathaiha |

Biểu tượng nằm trong hai file dùng lại được: `_xa-hoi.qmd` cho dạng tròn không chữ, `_xa-hoi-chu.qmd` cho dạng thẻ kèm tên. Chèn vào trang bằng `{{< include _xa-hoi-chu.qmd >}}`. Riêng chân trang phải dán thẳng mã vào `_quarto.yml`, vì Quarto không chạy shortcode trong phần cấu hình.

## Hiệu ứng hoa nở

Cụm ba bông anh túc ở khối mở đầu trang chủ nở dần khi tải trang: thân mọc lên trước, rồi từng cánh bung ra lệch nhau một nhịp, nhuỵ hiện cuối cùng. Rê chuột vào là nở lại.

Toàn bộ nằm ở `_hoa-no.qmd` và mục "HOA ANH TÚC NỞ" trong `styles.scss`. Muốn đặt ở trang khác thì thêm lớp `.hero-hoa` vào khối `::: {.hero}` của trang đó, rồi chèn đoạn dưới đây ngay trước dấu `:::` đóng khối:

```
::: {.hoa-no-khung}
{{< include _hoa-no.qmd >}}
:::
```

Lớp `.hero-hoa` tự tắt luống hoa tĩnh để hai lớp không chồng lên nhau. Máy nào bật chế độ giảm chuyển động thì hoa hiện sẵn ở trạng thái đã nở.

## Hiệu ứng chuyển động ở trang chủ

Bốn con số trên trang chủ đếm từ 0 lên khi cuộn tới, các thẻ hiện dần lên. Toàn bộ nằm trong `scripts.html`.

Muốn thêm một khối hiện dần, gắn lớp `.reveal` vào khối đó. Muốn một con số đếm lên, viết như sau, trong đó số trong ngoặc vuông là số thật để người tắt JavaScript vẫn đọc được:

```
[16]{.fact-num data-dem="16"}
```

Đoạn mã tôn trọng thiết lập giảm chuyển động của hệ điều hành: ai bật chế độ đó sẽ thấy trang tĩnh hoàn toàn.

## Thêm một bài viết mới

Tạo file trong thư mục `bai-viet/`, ví dụ `bai-viet/doc-bang-ket-qua-pls-sem.qmd`, viết nội dung, rồi thêm liên kết vào `bai-viet.qmd`. Khi số bài vượt quá mười, chuyển `bai-viet.qmd` sang dạng listing tự động của Quarto.

## Chạy code Python ngay trong trang

Quarto chạy được code Python trong file `.qmd`, hữu ích nếu sau này chị muốn trình bày dữ liệu mà không phải chụp màn hình:

````
```{python}
#| echo: false
import pandas as pd
df = pd.read_csv("data/khao-sat.csv")
df.describe()
```
````
