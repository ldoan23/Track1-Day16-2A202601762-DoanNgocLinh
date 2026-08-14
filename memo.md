# Memo Teardown — Seedance (ByteDance)

**Nhóm:** Nhóm bàn đầu **Thành viên:** Phạm Tiến Đại, Đoàn Ngọc Linh, Tuấn Anh

**Vì sao chọn sản phẩm này:** Seedance là mô hình AI tạo video đang dẫn đầu benchmark thị trường (vượt Kling, Runway, Veo khi ra mắt 2.0), nhưng cũng vừa trải qua khủng hoảng bản quyền lớn với Hollywood chỉ trong vài ngày ra mắt — một case study hiếm có đủ cả tín hiệu tăng trưởng, tranh cãi pháp lý, và pivot chiến lược rõ rệt trong chưa đầy 12 tháng.

---

## §1. Timeline các cập nhật lớn

| Thời điểm   | Cập nhật                                                                                                                                                                                                                                    | Context lúc đó                                                                                                                                         | Nguyên lý                                                                                                                                                |
| ----------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **06/2025** | Seedance 1.0 ra mắt — hợp nhất T2V+I2V, multi-shot storytelling trong 1 model ([nguồn](https://seed.bytedance.com/en/seedance) · [tech report](https://arxiv.org/abs/2506.09113))                                                           | Kling, Runway, Sora đã định hình chuẩn video AI; ByteDance có kho video ngắn khổng lồ từ TikTok/Douyin nhưng chưa có model riêng đủ mạnh để cạnh tranh | **x10, không x1.1** — dùng chính data proprietary khổng lồ (video ngắn từ hệ sinh thái Douyin/TikTok) làm moat huấn luyện mà đối thủ phương Tây không có |
| **12/2025** | Seedance 1.5 Pro — thêm sinh audio-video đồng thời (dual-branch DiT), điều khiển camera điện ảnh ([nguồn](https://seed.bytedance.com/en/public_papers/seedance-1-5-pro-a-native-audio-visual-joint-generation-foundation-model))            | Veo 3 (Google) đã native audio-video, tạo chuẩn mới "video có tiếng"; Seedance 1.0 chỉ tạo được hình câm                                               | **Định nghĩa lại "tốt"** — video AI không còn là "hình đẹp" mà phải "đồng bộ audio-visual" mới coi là production-ready                                   |
| **02/2026** | Seedance 2.0 — hợp nhất Pro/Lite thành 1 model, nhận 4 modality đầu vào, đứng đầu Artificial Analysis Video Arena ([nguồn](https://seedance2-video.com/seedance-2-0-release-notes))                                                         | Cuộc đua benchmark khốc liệt (Kling 3.0, Sora 2, Veo 3.1); cần vị trí #1 để bán vào doanh nghiệp/API                                                   | Gộp nhiều biến thể thành **1 model đa năng** → giảm chi phí vận hành, tăng tốc vòng lặp cải tiến                                                         |
| **02/2026** | Phản ứng khủng hoảng bản quyền — cam kết thêm rào chắn, tạm chặn tạo khuôn mặt thật & nhân vật có IP ([nguồn](https://www.cnbc.com/2026/02/16/bytedance-safegaurds-seedance-ai-copyright-disney-mpa-netflix-paramount-sony-universal.html)) | Disney, MPA, Paramount, Warner Bros., Netflix đồng loạt gửi thư cease-and-desist ngay trong tuần ra mắt 2.0, tố "vi phạm bản quyền quy mô lớn"         | **Moat phòng thủ, không phải tăng trưởng** — đánh đổi một phần "x10 khả năng" để giữ quyền vận hành ở thị trường phương Tây                              |
| **03/2026** | Tích hợp "Dreamina Seedance" vào CapCut, triển khai theo giai đoạn ra nhiều thị trường quốc tế ([nguồn](https://techcrunch.com/2026/03/26/bytedances-new-ai-video-generation-model-dreamina-seedance-2-0-comes-to-capcut/))                 | CapCut đã có hàng trăm triệu người dùng edit video toàn cầu; Seedance cần kênh phân phối ngoài Trung Quốc                                              | **Wrapper/moat qua kênh phân phối có sẵn** — cắm engine vào app đã có network effect khổng lồ                                                            |
| **07/2026** | Seedance 2.5 — tạo clip one-take 30 giây, tham chiếu tới 50 asset (ảnh/video/audio), chỉnh sửa theo timestamp ([nguồn](https://technode.com/2026/07/31/bytedance-launches-seedance-2-5-video-generation-model/))                            | Nhu cầu kể chuyện dài hơn cho quảng cáo/phim ngắn vượt giới hạn 15s; đối thủ vẫn giới hạn ở clip ngắn rời rạc                                          | **x10 tiếp theo** — mở rộng use case từ "tạo clip" sang "sản xuất nội dung có kịch bản"                                                                  |

**Vì sao chọn những mốc này:** Loại các bản patch tốc độ suy luận/benchmark nhỏ lẻ vì đó là tinh chỉnh kỹ thuật, không phải quyết định định hướng sản phẩm. Giữ mốc khủng hoảng bản quyền (02/2026) dù không phải "tính năng" vì phản ứng của ByteDance (chặn deepfake, cam kết rào chắn) là một quyết định sản phẩm thực sự làm thay đổi cách model hoạt động. Chọn CapCut integration thay vì các thoả thuận phân phối nhỏ khác vì quy mô người dùng đủ lớn để rút ra một pattern chiến lược, không phải nhiễu ngẫu nhiên.

---

## §2. Tệp user & JTBD

|                                   | Early adopters                                                                                                                                                                          | Tệp hiện tại                                                                                                                                                                                                                                    |
| --------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Đặc điểm**                      | AIGC creator (AI-Generated Content creator) / prosumer kỹ thuật ở Trung Quốc — đã hiểu prompt engineering, aspect ratio, reference frame; dùng Jimeng/Doubao trực tiếp bằng tiếng Trung | 3 nhánh: (A) SMB owner/freelancer không rành kỹ thuật, vào qua wrapper đơn giản hoá; (B) agency/studio chuyên nghiệp, chọn Seedance cho use case cụ thể bên cạnh Kling/Runway; (C) team marketing/training nội bộ doanh nghiệp                  |
| **JTBD chính**                    | Previsualize (dựng hình trước) cảnh phim nhiều shot phức tạp để trình bày ý tưởng cho đoàn phim, không cần thuê diễn viên/bối cảnh thật                                                 | (A) Có video sản phẩm 15s chất lượng chuyên nghiệp trong vài phút, giá bằng ly cà phê; (B) Đạt output cinematic + audio đồng bộ cho brand film mà không phải quay thật; (C) Tạo video đào tạo/demo từ tài liệu/ảnh có sẵn, không cần ê-kíp quay |
| **Trước đó họ làm bằng cách nào** | Vẽ storyboard tay + animatic thủ công, hoặc tự quay thử bằng điện thoại để test blocking                                                                                                | (A) Thuê videographer freelance $500–2.000/clip; (B) Tự quay thật hoặc dùng riêng lẻ Runway/Kling; (C) Thuê production house làm video corporate hoặc dùng slide+voice-over kiểu cũ                                                             |

**Dịch chuyển tệp:** hai cột mốc ở §1 gây ra dịch chuyển — **Seedance 2.0 (02/2026)** (viral moment với clip celebrity mashup → nhận diện đại chúng, 500.000 video tạo ra ngay tuần đầu, kéo theo các nền tảng wrapper phục vụ SMB/freelancer) và **CapCut integration (03/2026)** (đưa engine vào tay hàng trăm triệu casual editor đã quen CapCut, không cần biết Seedance là gì). Trước hai mốc này, Seedance gần như chỉ tồn tại trong vòng tròn kỹ thuật ở Trung Quốc; sau đó mới lan ra đại chúng và doanh nghiệp quốc tế.

**Switching cost (map 4 forces):**

- **Push** (đẩy rời cách cũ): chi phí thuê ekip quay cao ($500–2.000/clip), tốc độ sản xuất chậm, ngân sách agency bị ép làm nhiều brand film hơn với chi phí thấp hơn.
- **Pull** (kéo về Seedance): chất lượng benchmark dẫn đầu, giá rẻ, tích hợp sẵn trong CapCut, hệ thống multimodal reference giữ đúng ý tưởng sáng tạo.
- **Anxiety** (ngần ngại): rủi ro pháp lý bản quyền/deepfake, API doanh nghiệp chưa mở rộng đầy đủ (agency Nexia xác nhận "API planned Q3 2026"), lo output "trông giả".
- **Habit** (giữ ở cách cũ): agency đã quen workflow Runway/Kling, đội sản xuất truyền thống vẫn quen quay thật khi cần độ tin cậy tuyệt đối.

Lực giữ chân mạnh nhất hiện nay là **Habit qua lớp vỏ CapCut** đối với nhóm mass-market (nhánh A) — switching cost ở đây thấp và mỏng manh, vì họ chưa từng "chọn" Seedance mà chỉ dùng cái có sẵn trong app. Ngược lại, nhóm agency chuyên nghiệp (nhánh B) chỉ được giữ chân bởi **Pull thuần túy** (chất lượng benchmark), chưa có lock-in thật sự — nếu đối thủ vượt benchmark, nhóm này sẽ chuyển ngay.

---

## §3. Ba dự đoán hướng đi (6–12 tháng tới)

**Dự đoán 1** _(loại: mở rộng tính năng)_

- **Dự đoán:** Seedance sẽ mở API cho studio/agency vượt ra ngoài CapCut/Dreamina, đi kèm bộ công cụ xác thực bản quyền/watermark ẩn để cấp phép sử dụng nhân vật/IP hợp pháp.
- **Lập luận:** khủng hoảng bản quyền 02/2026 (§1) buộc ByteDance cam kết rào chắn; đồng thời agency (§2, nhánh B) đã công khai phàn nàn API "chỉ qua CapCut/Dreamina, planned Q3 2026" — nhu cầu mở API đến từ chính user hiện tại, không phải suy đoán.

**Dự đoán 2** _(loại: mở rộng segment)_

- **Dự đoán:** Seedance sẽ đẩy mạnh vào vertical thương mại điện tử/quảng cáo ngắn, gắn trực tiếp với Douyin Shop/TikTok Shop — video quảng cáo sản phẩm theo template cho người bán hàng.
- **Lập luận:** nhánh A (§2) đã hình thành tự phát qua wrapper bên thứ ba với JTBD rõ ràng là "video sản phẩm rẻ, nhanh"; ByteDance sở hữu sẵn toàn bộ stack thương mại điện tử, nên dồn nhóm SMB đang tăng trưởng tự nhiên vào đúng kênh bán hàng của công ty mẹ là bước mở rộng ít tốn công nhất.

**Dự đoán 3** _(loại: đe dọa Big Tech)_

- **Dự đoán:** Adobe/Google sẽ nhúng sinh video AI thẳng vào công cụ hiện có (Premiere, Google Vids) theo gói subscription, đe doạ giá trị độc lập của Seedance với nhóm agency; Seedance phản ứng bằng cách siết chặt bundling qua CapCut thay vì mở API quá nhanh.
- **Lập luận:** mốc CapCut integration 03/2026 (§1) cho thấy ByteDance đã chủ động chọn chiến lược "bundle vào hệ sinh thái mình sở hữu" thay vì bán API rộng rãi ngay từ đầu; việc Sora bị đóng hoàn toàn cũng cho thấy các nền tảng lớn đang củng cố vào hệ sinh thái riêng thay vì đứng độc lập.

**Dự đoán tự tin nhất:** Dự đoán 1 — vì nó dựa trên 2 nguồn độc lập cùng trỏ về 1 hướng: áp lực pháp lý bên ngoài (§1) và nhu cầu thực tế từ chính user hiện tại (§2). **Giả định nếu sai sẽ làm nó gãy:** nếu ByteDance đánh giá rủi ro pháp lý phương Tây là không đáng theo đuổi và chọn tập trung hoàn toàn vào Trung Quốc/Đông Nam Á, động lực mở API cho agency phương Tây sẽ biến mất — API có thể vẫn mở nhưng chỉ phục vụ thị trường nội địa.

---

## §4. AI Log

| Việc                                                                                                 | AI làm hay nhóm làm?                               | Nhóm kiểm chứng/phán đoán lại thế nào?                                                                                                                                                                               |
| ---------------------------------------------------------------------------------------------------- | -------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Deep research tổng hợp tổng quan Seedance (changelog, kiến trúc AI, model family, quy mô thương mại) | AI (extended research task, nhiều vòng web search) | Chưa đối chiếu độc lập từng con số (ví dụ ARR ~2 tỷ USD) — bản thân báo cáo đã tự gắn caveat rằng số liệu do 36Kr đưa ra và Volcano Engine cho là "phóng đại"; cần nhóm tự kiểm lại trước khi trích dẫn số cứng      |
| Chọn 6 cột mốc đưa vào bảng §1                                                                       | Nhóm                                               | Ban đầu AI **làm nhầm sang sản phẩm Notion** (do đang thực hành với ví dụ khác trong buổi làm việc) — nhóm phát hiện lỗi và yêu cầu làm lại đúng Seedance; sau đó nhóm chưa tranh luận sâu để loại thêm mốc nào khác |
| Đào review/quote thực tế cho §2 (Product Hunt/Reddit-style, G2, wrapper platform, agency review)     | AI (web search)                                    | Mỗi quote chỉ có 1 nguồn duy nhất, chưa đối chiếu chéo với nguồn thứ 2; nhóm chưa tự vào Reddit/Discord thật để xác nhận thêm như gợi ý ban đầu của bài tập                                                          |
| Viết JTBD, switching cost, phân tích 4 forces                                                        | Đại, Tuấn Anh                                      | Nhóm chưa phản biện trực tiếp cách map vào 4 forces — đây là suy luận của AI dựa trên dữ liệu đã đào, chưa qua vòng tranh luận nhóm như quy trình đề ra                                                              |
| Chú thích thuật ngữ (AIGC, prosumer, JTBD, benchmark, lock-in...)                                    | AI                                                 | Nhóm yêu cầu bổ sung vì bản gốc dùng thuật ngữ không giải thích; định nghĩa dựa trên hiểu biết chung của AI, chưa đối chiếu với giáo trình/tài liệu gốc của khoá học                                                 |
| Viết 3 dự đoán §3                                                                                    | Linh                                               | **Chưa có bước nhóm tự viết nháp rồi chất vấn lẫn nhau** như quy trình bài tập yêu cầu — cả 3 dự đoán đều do AI đề xuất trong 1 lượt, nhóm chưa thách thức lập luận trước khi chốt                                   |

**Câu hỏi phản biện — Chỗ nào AI làm thay nhiều nhất, nếu bỏ ra nhóm còn tự giải thích được không?**

AI làm thay nhiều nhất ở **§3 (ba dự đoán)** và phần đào quote/số liệu nền cho §2 — cả hai đều dựa trên thông tin AI tự tìm và tự diễn giải trong một lượt duy nhất, chưa qua vòng "mỗi người viết nháp rồi chất vấn" như quy trình đề ra. Nếu bỏ hai phần này, nhóm hiện tại **chưa chắc tự giải thích lại được** — vì nhóm chưa tự đọc trực tiếp review G2/Reddit/Discord, chưa tự đối chiếu số liệu ARR, và chưa tự tranh luận để chọn ra 3 dự đoán từ nhiều phương án khác nhau. Đây là khoảng trống cần nhóm lấp lại trước khi nộp memo, đúng tinh thần "khai báo trung thực vai trò AI" của bài tập.
