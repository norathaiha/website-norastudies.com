# Liên kết chính NoraStudies

Ngày cập nhật: 08/08/2026

## Năm trang hoạt động chính

| Nền tảng | Liên kết | Ghi chú |
|---|---|---|
| Website | `https://norastudies.com/` | Trung tâm, liên kết ra các kênh khác |
| Instagram | `https://www.instagram.com/norathai_studies/` | Lịch sử tích lũy, người theo dõi cũ |
| Threads | `https://www.threads.com/@norathai_studies` | **Ưu tiên đẩy mạnh** từ 2026 |
| Facebook | `https://www.facebook.com/norastudies` | Page mới (hoặc đổi tên từ tapcardio.online) |
| TikTok | `https://www.tiktok.com/@norathai_studies` | Video ngắn, tiếp cận tìm kiếm |
| YouTube | `https://www.youtube.com/@norathaiha` | Video dài, cơ sở lý thuyết |

---

## Cách nối các trang vào nhau

### Ở mỗi bio/tiểu sử

Mỗi kênh chỉ có chỗ 150 ký tự (Instagram, Threads) hoặc một ô liên kết duy nhất (TikTok, Facebook). Không thể liệt kê hết năm kênh. Cách làm:

**Chính kênh** (Instagram, Threads, TikTok, Facebook):
```
Mentor nghiên cứu khoa học · norastudies.com
```

Liên kết chính luôn là **website**, vì website là nơi có đầy đủ thông tin và link ra bốn kênh khác.

**YouTube**: ở phần giới thiệu kênh (channel description), thêm một dòng với các liên kết khác:

```
Tìm mình ở các nơi khác:
Instagram: https://www.instagram.com/norathai_studies/
Threads: https://www.threads.com/@norathai_studies
Facebook: https://www.facebook.com/norastudies
Website: https://norastudies.com/
```

### Ở website (norastudies.com)

Chân trang hoặc thanh điều hướng phải có liên kết tới các kênh xã hội. Hiện tại file `_quarto.yml` đang có:

```yaml
right: |
  [Zalo](https://zalo.me/84965849113) · [WhatsApp](https://wa.me/84965849113)
```

Thêm vào:

```yaml
right: |
  [Instagram](https://www.instagram.com/norathai_studies/) · 
  [Threads](https://www.threads.com/@norathai_studies) · 
  [Facebook](https://www.facebook.com/norastudies) · 
  [TikTok](https://www.tiktok.com/@norathai_studies) · 
  [Zalo](https://zalo.me/84965849113) · [WhatsApp](https://wa.me/84965849113)
```

Hoặc tách riêng thành một phần "Theo dõi" ở chân trang.

---

## Chiến lược đẩy mạnh Threads

Threads là nền tảng mới, thuật toán thích nội dung mới. Người dùng Threads hiện tại chủ yếu là người đã dùng Twitter/X từ trước, tức có tư duy đọc chữ, không chỉ xem video.

Đối tượng của chị — người làm nghiên cứu, chủ yếu tuổi 25–45 — là đúng đối tượng của Threads.

### Cách làm:

1. **Nội dung**: Viết những chỗ hay sai khi làm luận án, những nhận xét ngắn về phương pháp, những câu hỏi thường gặp. Mỗi bài 300–400 ký tự, kèm một ảnh.

2. **Tần suất**: Mỗi tuần 3–5 bài. Không cần nhiều, nhưng phải đều.

3. **Liên kết**: Mỗi bài kết thúc bằng "Muốn nói chuyện chi tiết: norastudies.com" hoặc "Inbox để hẹn 30 phút tư vấn: [link Threads inbox]".

4. **Tương tác**: Trả lời bình luận trong vòng 12 giờ. Nhân xét bài của những người làm nghiên cứu. Follow hashtag #luanvan #luanansinhien #phuongphapnghiencuu.

5. **Liên kết ngược**: Ở Instagram Stories, chia sẻ bài từ Threads. Ở website, gói gọn một bài Threads dài thành một post blog kèm link "Thảo luận đầy đủ ở Threads".

---

## Tập lệnh kiểm tra liên kết

Mỗi tháng một lần, đi vòng và kiểm tra:

- [ ] Instagram bio có link website không
- [ ] Threads bio có link website không
- [ ] Facebook about section có link website không
- [ ] TikTok bio có website không
- [ ] YouTube channel description có link tới bốn kênh khác không
- [ ] Website chân trang có link tới năm kênh không
- [ ] Tất cả liên kết đều hoạt động (không 404)
- [ ] Facebook URL là `facebook.com/norastudies` (hoặc handle mới nếu đã đổi)

---

## Lưu ý kỹ thuật

**Facebook URL**: khi đổi tên/username page, đường dẫn cũ chết ngay. Sửa ở mọi chỗ đã dán, bao gồm:
- Website `_quarto.yml`
- Instagram link trong bio (nếu có)
- Threads (nếu có)
- YouTube description

**Threads và Instagram đồng bộ**: sửa Instagram là Threads tự đổi (nếu để nguyên đồng bộ). Nếu tách riêng thì phải nhớ sửa hai chỗ.

**Hashtag thống nhất**: Mặc dù mỗi kênh có cách viết riêng, nhưng hashtag và tên thương hiệu `#NoraStudies` phải giống nhau ở mọi chỗ để AI của nền tảng hiểu đây là một người/brand duy nhất.
