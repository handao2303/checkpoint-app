# Video demo Checkpoint

Mã nguồn của video demo trong [`docs/demo/`](../docs/demo/), dựng bằng [HyperFrames](https://github.com/heygen-com/hyperframes) (HTML + GSAP → MP4).

Toàn bộ màn hình trong video là ảnh chụp thật từ [checkpoint-app.space](https://checkpoint-app.space), quay ở khung iPhone 390×844 (DPR 3). Các màn có cuộn (Khám phá, Chi tiết địa điểm, Bảng XH, Hồ sơ) được chụp thành một dải dài `*-strip.jpg` để video cuộn mượt.

## Cấu trúc

```
demo-video/
├── index.html          composition: kịch bản, hiệu ứng, timeline GSAP
├── assets/logo.png     logo app
└── assets/screens/     ảnh chụp từng màn + dải cuộn *-strip.jpg
```

## Chỉnh nội dung

Mọi thứ nằm trong mảng `STEPS` ở `index.html`. Mỗi phần tử là một màn hình:

| Trường | Ý nghĩa |
|---|---|
| `img` | tên ảnh trong `assets/screens/` |
| `dur` | số giây màn hình hiển thị |
| `via` | cách chuyển màn: `push`, `fade`, `sheet` |
| `tap` | toạ độ nút được chạm trước khi rời màn (hệ 390×844) |
| `zoom` | camera zoom vào nút trước khi chạm |
| `strip` | dải nội dung cuộn: khung `rect`, quãng cuộn `to`, thời điểm `at`, thời lượng `d` |
| `cap` | nhãn, tiêu đề, mô tả bên trái |
| `chips` | thẻ tính năng bên phải; `count` để chạy số |

Khi đổi tổng thời lượng, cập nhật cùng lúc hằng `OUTRO` và các thuộc tính `data-duration` / `data-start` của `#root`, `#bg`, `#stage`, `#outro`. Mốc outro bằng `INTRO` cộng tổng `dur` cộng 3,8 giây đoạn kết.

## Xem và render

Cần Node 18+ và ffmpeg.

```bash
cd demo-video
npm run dev       # mở Studio để xem và tua timeline
npm run check     # lint + kiểm tra layout, chuyển động, tương phản
npx --yes hyperframes@0.8.155 render -o renders/checkpoint-demo.mp4
```

Video gốc dài khoảng 61 giây ở 30fps. Bản trong `docs/demo/` được tăng tốc 2x lên 60fps rồi nén về 720p:

```bash
ffmpeg -i renders/checkpoint-demo.mp4 -vf "setpts=0.5*PTS" -r 60 -c:v libx264 -crf 16 -pix_fmt yuv420p -an checkpoint-demo-2x.mp4
```
