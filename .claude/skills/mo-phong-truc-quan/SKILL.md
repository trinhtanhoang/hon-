---
name: mo-phong-truc-quan
description: Dùng mỗi khi Hon nhờ mô phỏng hoặc hỏi một hệ vật lý, kỹ thuật hoạt động thế nào (dòng điện trong mạch, mạch RC/RLC, động cơ, cánh tay robot, cơ cấu cơ khí, sóng, dao động, bộ điều khiển PID, cảm biến...), kể cả khi chỉ hỏi ngắn kiểu "dòng điện chạy sao", "tay robot quay thế nào". Ngoài phần giải thích bằng lời, dựng một trang web mô phỏng tương tác (canvas, thanh trượt, số đo cập nhật trực tiếp) và đăng thành Artifact ngay trong cuộc chat.
---

# Mô phỏng trực quan trong chat

Hon muốn mỗi lần mô phỏng hay giải thích một hiện tượng kỹ thuật thì trong chat có luôn một trang web mô phỏng chạy được, nhìn là hiểu. Lời giải thích trong chat vẫn giữ, trang mô phỏng đi kèm.

Mẫu tham khảo: `examples/xuong-mo-phong.html` (mạch nối tiếp có electron chạy, cánh tay robot 2 khớp có động học thuận và ngược). Đọc file này trước khi viết trang mới để giữ cùng kiểu trình bày và cách tổ chức code.

## Khi nào dùng

- Hon nói "mô phỏng", "cho xem", "trực quan", "chạy thế nào", "hoạt động ra sao".
- Câu hỏi về một hệ có trạng thái thay đổi theo thời gian hoặc theo tham số: mạch điện, máy điện, tay máy, cơ cấu truyền động, hệ điều khiển, dao động, sóng, trường điện từ.
- Không dùng cho câu hỏi tra cứu thuần (giá linh kiện, định nghĩa một từ) hay khi Hon nói rõ chỉ cần trả lời bằng lời.

## Trang mô phỏng phải có

1. Vật lý thật. Mọi chuyển động và số đo tính từ công thức đúng (định luật Ôm, Kirchhoff, phương trình vi phân giải bằng bước thời gian, động học thuận/ngược...). Không làm hoạt hình "cho đẹp" tách rời công thức.
2. Tham số chỉnh được bằng thanh trượt, có đơn vị thật (V, Ω, A, cm, °, rad/s) và khoảng giá trị hợp lý với đồ án sinh viên.
3. Bảng số đo cập nhật trực tiếp, định dạng số kiểu Việt Nam (dấu phẩy thập phân, `toLocaleString('vi-VN')`).
4. Mở ra là đang chạy ở trạng thái điển hình, không phải khung trống chờ nhập.
5. Có tương tác trực tiếp trên hình khi hợp lý: bấm công tắc, kéo điểm đích, kéo vật.
6. Có một tình huống "thử phá" để học: đèn cháy khi quá công suất, robot không với tới điểm ngoài vùng làm việc, mạch cộng hưởng khi đúng tần số...
7. Khối giải thích ngay dưới mô phỏng: công thức dùng (đặt trong khối chữ đơn cách), giả thiết đơn giản hóa, và chỗ nào hình được phóng đại so với thực tế (ví dụ tốc độ electron).
8. Nhiều hiện tượng liên quan thì chia tab trong cùng một trang.

## Kỹ thuật

- Trước khi viết trang: gọi Artifact `action: "quickstart"` với `intent: "other"` (hoặc nạp skill `artifact-design` nếu không có quickstart) và làm đúng hợp đồng trang ở đó.
- Vẽ bằng Canvas 2D với `requestAnimationFrame`; dùng hệ tọa độ logic rồi co giãn theo bề rộng khung, nhân `devicePixelRatio` cho nét.
- Màu khai báo bằng biến CSS cho cả sáng và tối; canvas đọc lại biến bằng `getComputedStyle` khi đổi giao diện.
- Chạy được ở bề rộng điện thoại (khoảng 360 px): khung vẽ co theo `aspect-ratio`, chữ trên canvas không nhỏ hơn 11 px thật, cột điều khiển xuống dưới khung vẽ.
- Tôn trọng `prefers-reduced-motion`: chạy chậm lại hoặc nhảy thẳng tới trạng thái cuối.
- Không cần thư viện cho mô phỏng 2D. Mô phỏng 3D thì nạp three.js bản UMD từ cdnjs, ghim đúng phiên bản.
- Chữ trên trang và trong chat theo skill `viet-khong-mui-ai`.

## Quy trình

1. Trả lời câu hỏi bằng lời ngắn gọn trong chat như bình thường.
2. Xác định hệ cần mô phỏng, công thức, tham số và một tình huống "thử phá".
3. Viết trang HTML vào thư mục nháp (scratchpad), tách phần tính toán thành hàm thuần.
4. Kiểm tra một lần: `node --check` cho đoạn script, và chạy hàm tính với một bộ số đã biết đáp án (ví dụ U = 12 V, R = 24 Ω thì I = 0,5 A).
5. Đăng bằng Artifact, đưa link trong chat kèm 2 đến 3 dòng hướng dẫn: kéo gì, bấm gì, nên thử gì.
6. Hon muốn chỉnh thì sửa file cũ và đăng lại cùng đường dẫn để giữ link.
