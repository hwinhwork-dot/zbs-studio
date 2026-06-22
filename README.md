<div align="center">

# 🟦 ZBS Sandbox Studio

### Trình soạn & tự kiểm duyệt mẫu tin ZNS trước khi gửi duyệt

*Nhập thông tin → tự sinh JSON → đối chiếu bộ quy tắc → AI kiểm duyệt → gửi CTV ZBS.*

### ▶️ Dùng thử ngay (bản online): **[zbs-studio-nguyenhoangminh.vercel.app](https://zbs-studio-nguyenhoangminh.vercel.app/)**

***Tôi xây công cụ này với một mong muốn giản dị: để mỗi mẫu tin của bạn được duyệt ngay từ lần gửi đầu tiên.***

</div>

---

## 📑 Mục lục

1. [ZBS Sandbox Studio là gì và tôi xây nó để giải quyết điều gì](#1-zbs-sandbox-studio-là-gì-và-tôi-xây-nó-để-giải-quyết-điều-gì)
2. [Sơ đồ luồng xử lý dữ liệu — và quan trọng nhất: khi nào AI vào cuộc](#2-sơ-đồ-luồng-xử-lý-dữ-liệu--và-quan-trọng-nhất-khi-nào-ai-vào-cuộc)
3. [Hướng dẫn sử dụng theo từng bước](#3-hướng-dẫn-sử-dụng-theo-từng-bước)
4. [Bộ ba lớp kiểm duyệt: Tự động · AI · CTV](#4-bộ-ba-lớp-kiểm-duyệt-tự-động--ai--ctv)
5. [Năm case mẫu — mỗi case tôi muốn chứng minh một điều](#5-năm-case-mẫu--mỗi-case-tôi-muốn-chứng-minh-một-điều)
6. [Giới hạn ký tự và quy tắc theo Tag](#6-giới-hạn-ký-tự-và-quy-tắc-theo-tag)
7. [Câu hỏi thường gặp](#7-câu-hỏi-thường-gặp)
8. [Cách mở file và ghi chú kỹ thuật](#8-cách-mở-file-và-ghi-chú-kỹ-thuật)

---

## 1. ZBS Sandbox Studio là gì và tôi xây nó để giải quyết điều gì

Tôi xin bắt đầu bằng câu chuyện thực tế. Khi một doanh nghiệp muốn gửi tin nhắn ZNS (Zalo Notification Service) qua nền tảng ZBS, mẫu tin đó luôn phải đi qua một vòng kiểm duyệt của Zalo. Vấn đề là rất nhiều mẫu bị **từ chối đi từ chối lại** chỉ vì những lỗi tưởng nhỏ: để số điện thoại trong nội dung thay vì đặt ở nút bấm, thiếu mã đơn hàng để chứng minh đã phát sinh giao dịch, dùng đường link dẫn vào nhóm, hay một lỗi chính tả lọt qua mắt người soạn. Mỗi lần bị từ chối là một lần chờ đợi, một lần làm lại, và một lần lỡ mất thời điểm gửi tin.

**ZBS Sandbox Studio là công cụ tôi xây ra để chặn những lỗi đó ngay từ lúc bạn còn đang soạn**, thay vì để bạn phát hiện sau khi đã gửi đi và bị trả về. Bạn không cần biết JSON là gì, không cần đọc tài liệu kỹ thuật dày cộp. Bạn chỉ cần điền thông tin vào một biểu mẫu trực quan, và tôi sẽ lo phần còn lại.

Tôi muốn bạn ghi nhớ **mạch xương sống** của công cụ này, vì mọi thứ khác đều xoay quanh nó:

> ### 🧭 Bạn nhập dữ liệu → Tôi tự sinh ra JSON → Tôi tra cứu bộ quy tắc ZBS → AI kiểm duyệt ngữ nghĩa → Mẫu được gửi cho CTV ZBS

Nói cách khác, tôi đứng giữa **bạn** và **bộ phận kiểm duyệt của Zalo (CTV ZBS)**, đóng vai một người soát lỗi tận tâm: tôi đọc mẫu tin của bạn bằng đúng bộ luật mà Zalo dùng, chỉ cho bạn thấy chỗ nào chưa ổn bằng ngôn ngữ dễ hiểu, và chỉ để mẫu "đi tiếp" khi nó đã đủ sạch sẽ.

**Cụ thể thì tôi giúp bạn những việc sau:**

- **Soạn mẫu mà không cần chạm vào code.** Bạn chọn loại mẫu, điền tiêu đề, nội dung, chèn các biến cá nhân hóa dạng `<ten_bien>`, thêm bảng thông tin và nút bấm — tất cả qua các ô nhập quen thuộc.
- **Xem trước y như thật.** Mọi thứ bạn gõ hiện ngay lên một màn hình Zalo mô phỏng ở bên phải, để bạn biết khách hàng sẽ nhìn thấy gì.
- **Tự dịch sang JSON chuẩn ZBS.** Phần JSON này chạy ngầm ở backend; tôi có hiển thị nó ra cho bạn xem chỉ nhằm mục đích kiểm tra logic, còn khi triển khai thật mục này sẽ được ẩn đi.
- **Tự kiểm duyệt theo đúng luật.** Tôi đối chiếu mẫu của bạn với khoảng 75 quy tắc của ZBS (phân loại Tag, yêu cầu tham số định danh, quy định về logo, nút bấm, văn phong, ngành nghề đặc biệt…), cộng thêm một lớp AI đọc hiểu ngữ nghĩa.
- **Quản lý các mẫu đã gửi.** Sau khi gửi, bạn có thể xem lại từng mẫu, và nếu mẫu nào còn cảnh báo thì thu hồi để chỉnh sửa, với đúng những ô bị lỗi được tô đỏ.

---

## 2. Sơ đồ luồng xử lý dữ liệu — và quan trọng nhất: khi nào AI vào cuộc

Tôi biết phần này là phần bạn quan tâm nhất, nên tôi sẽ trình bày bằng sơ đồ để bạn nhìn một cái là hiểu. Tôi tách thành ba sơ đồ: bức tranh tổng thể, thời điểm AI thực sự "nhảy ra" làm việc, và vòng đời của một mẫu tin.

### 2.1. Bức tranh tổng thể: dữ liệu của bạn đi qua những chặng nào

Sơ đồ dưới đây mô tả trọn vẹn hành trình của một mẫu tin, từ lúc bạn gõ chữ đầu tiên cho đến khi nó nằm trên bàn của chuyên viên kiểm duyệt ZBS.

```mermaid
flowchart TD
    A["👤 Bạn nhập thông tin<br/>(tiêu đề, nội dung, bảng, nút bấm…)"] --> B["⚙️ Tôi tự sinh JSON<br/>theo đúng schema ZBS"]
    B --> C["📺 Xem trước trên màn hình Zalo mô phỏng<br/>(cập nhật theo thời gian thực)"]
    B --> D{"📚 Tôi tra cứu BỘ QUY TẮC ZBS<br/>(kiểm tra tự động, chạy tức thì)"}
    D -->|"Lỗi nghiêm trọng"| E["🔴 Vi phạm — gần như chắc chắn bị từ chối"]
    D -->|"Điểm cần lưu ý"| F["🟡 Cảnh báo — nên xem lại"]
    D -->|"Không thấy lỗi"| G["🟢 Tạm đạt"]
    E --> H{"🤖 Lớp AI kiểm duyệt ngữ nghĩa<br/>(chạy khi bạn bấm Gửi duyệt)"}
    F --> H
    G --> H
    H --> I["📋 Tôi tổng hợp kết quả cuối:<br/>Đạt · Cần xem lại · Không đạt"]
    I -->|"Mẫu sạch"| J["✅ Gửi cho CTV ZBS kiểm duyệt"]
    I -->|"Còn lỗi"| K["✏️ Thu hồi & chỉnh sửa lại<br/>(ô lỗi được tô đỏ)"]
    K --> A
    J --> L["🏁 CTV ZBS tiếp nhận & duyệt cuối cùng"]
```

Bạn hãy để ý: phần **tra cứu bộ quy tắc** chạy ngay lập tức và liên tục trong lúc bạn soạn, còn phần **AI** thì có một thời điểm xuất hiện rất cụ thể — tôi sẽ làm rõ ngay bên dưới.

### 2.2. Khi nào AI thực sự vào cuộc

Đây là điều tôi muốn nhấn mạnh, vì nhiều người hiểu nhầm rằng AI chạy suốt. Thực tế không phải vậy. Tôi chia việc kiểm duyệt thành **hai nhịp** rất khác nhau:

```mermaid
flowchart TD
    subgraph NHIP1["🔄 NHỊP 1 — Mỗi lần bạn gõ phím (tức thì, hoạt động ngoại tuyến)"]
        direction LR
        R1["Tự sinh JSON"] --> R2["Kiểm tra định dạng tham số,<br/>giới hạn ký tự, cấu trúc"]
        R2 --> R3["Đối chiếu các quy tắc<br/>'đếm được' của ZBS"]
    end

    subgraph NHIP2["🤖 NHỊP 2 — Chỉ khi bạn bấm nút 'Gửi duyệt'"]
        direction TB
        S1["Chạy lại TOÀN BỘ kiểm tra tự động"] --> S2{"Môi trường có kết nối AI không?"}
        S2 -->|"Có (mở trong Claude.ai)"| S3["🤖 AI ĐỌC HIỂU NGỮ NGHĨA mẫu tin:<br/>• Thiếu mã định danh giao dịch?<br/>• Văn phong mê tín / thổi phồng?<br/>• Link có ý dẫn vào nhóm/cộng đồng?<br/>• Lỗi chính tả, đánh máy?<br/>• Chủ tài khoản có khớp doanh nghiệp?"]
        S2 -->|"Không (chạy ngoại tuyến)"| S4["Bỏ qua AI một cách an toàn,<br/>chỉ dùng kết quả tự động"]
        S3 --> S5["Gộp kết quả tự động + AI"]
        S4 --> S5
    end

    NHIP1 -->|"Bạn soạn xong và bấm Gửi duyệt"| NHIP2
    S5 --> OUT["📋 Hiển thị kết quả kiểm duyệt cho bạn"]
```

Tôi giải thích bằng lời cho thật rõ:

- **Nhịp 1 — Lớp kiểm tra tự động (deterministic):** Lớp này chạy *âm thầm và tức thì* mỗi khi bạn thay đổi bất cứ thứ gì. Nó bắt những lỗi có thể "đo đếm" được một cách chắc chắn: số điện thoại hay đường link nằm trong nội dung, tham số viết sai định dạng, vượt quá giới hạn ký tự, thiếu tham số định danh, dùng link rút gọn hay link nhóm… Vì nó không cần internet, nên lúc nào nó cũng hoạt động.

- **Nhịp 2 — Lớp AI ngữ nghĩa:** Lớp này **chỉ thức dậy đúng một thời điểm: khi bạn bấm nút "Gửi duyệt"**. Lý do là AI cần đọc và *hiểu ý nghĩa* của cả mẫu tin — việc này tốn kém hơn và không nên chạy theo từng phím gõ. AI sẽ soi những thứ mà máy móc thuần túy khó bắt: liệu nội dung có thực sự chứng minh được đã có giao dịch hay không, văn phong có mang tính mê tín/thổi phồng không, một đường link trông "bình thường" nhưng thực chất lại mời vào nhóm, hay một lỗi chính tả tinh vi như "KÍCH HỌA" lẽ ra phải là "KÍCH HOẠT".

> **Một lưu ý trung thực để bạn không bỡ ngỡ:** Lớp AI chỉ kết nối được khi công cụ chạy trong môi trường Claude.ai. Nếu bạn mở file ngoại tuyến, AI sẽ tự ẩn đi và báo nhẹ nhàng, còn **toàn bộ lớp kiểm tra tự động vẫn chạy đầy đủ** — nên mẫu của bạn vẫn được soát rất kỹ.

### 2.3. Vòng đời của một mẫu tin

Cuối cùng, để bạn hình dung một mẫu tin "sống" như thế nào từ lúc sinh ra đến lúc được duyệt, tôi vẽ thêm sơ đồ trạng thái này:

```mermaid
stateDiagram-v2
    [*] --> Soan
    Soan: Soạn (template trắng)
    KiemDuyet: Kiểm duyệt
    DaGui: Đã gửi
    CanSua: Cần sửa
    Soan --> Soan: Sửa và kiểm tra tự động liên tục
    Soan --> KiemDuyet: Bấm Gửi duyệt
    KiemDuyet --> DaGui: Không có vi phạm
    KiemDuyet --> CanSua: Còn cảnh báo hoặc vi phạm
    DaGui --> [*]: CTV ZBS tiếp nhận
    CanSua --> Soan: Thu hồi và chỉnh sửa lại
    DaGui --> Soan: Xem lại mẫu đã gửi
```

---

## 3. Hướng dẫn sử dụng theo từng bước

Tôi thiết kế công cụ theo lối "dắt tay từng bước", nên bạn cứ đi tuần tự là được.

### Bước 0 — Xem hướng dẫn mở màn

Ngay khi bạn mở công cụ, tôi sẽ cho chạy **một popup hướng dẫn tự động** gồm bốn chặng (giới thiệu → chọn loại & mục đích → soạn nội dung → kiểm duyệt → theo dõi mẫu đã gửi). Thanh tiến trình ở trên cùng sẽ tự chạy từ xám sang xanh để bạn biết nó đang tự trình chiếu. Ở chặng cuối, tôi mời bạn **tích vào ô xác nhận đã đọc và tuân thủ nguyên tắc** rồi mới bấm "Bắt đầu sử dụng". Đây là chủ ý của tôi: tôi muốn chắc chắn bạn đã nắm luật chơi trước khi bắt đầu.

Sau khi bạn bấm bắt đầu, tôi mở ra **một mẫu hoàn toàn trắng** — chưa có thông tin nào được điền sẵn — và màn hình điện thoại bên phải hiện logo Zalo cùng lời chào, như một tờ giấy trắng đang chờ bạn.

### Bước 1 — Thông tin template

Ở bước này, bạn khai báo những thông tin nền:

- **Mã định danh trang doanh nghiệp (OA ID)** — *bắt buộc.* Đây là mã số của Official Account của bạn.
- **Chọn loại mẫu nội dung** — Tuỳ chỉnh, Xác thực (OTP), Đánh giá, Yêu cầu chi trả, hoặc Voucher. Mỗi loại sẽ mở ra đúng những khối phù hợp.
- **Chọn mục đích gửi tin (Tag)** — Cấp độ 1 (Giao dịch), Cấp độ 2 (Chăm sóc), hoặc Cấp độ 3 (Hậu mãi). Đây là thông tin quyết định bộ luật áp dụng, nên tôi để riêng và không nhét vào JSON.
- **Hình ảnh đầu mẫu** — bạn dán link **Logo** (*bắt buộc* nếu dùng logo) hoặc chọn Ảnh slider.

> Tôi đặt quy tắc: **bạn phải điền OA ID và link logo thì mới được sang bước sau.** Nếu bạn cố bấm "Tiếp tục" khi còn thiếu, màn hình sẽ rung nhẹ và ô đang thiếu sẽ hiện viền đỏ kèm dòng nhắc.

### Bước 2 — Nội dung tin nhắn

Đây là phần ruột của mẫu tin:

- **Tiêu đề** và **Văn bản mô tả chính** — *bắt buộc.* Bạn chèn biến cá nhân hóa dạng `<customer_name>`, `<order_code>`… để Zalo tự điền dữ liệu thật khi gửi.
- **Bảng thông tin (map_info)** — bảng hai cột Nhãn – Giá trị. Tôi đặt quy tắc: **nếu đã có hàng thì mỗi hàng phải điền đủ cả hai cột; còn nếu bạn không cần bảng thì cứ để trống không thêm hàng nào.** Hàng nào điền dở dang mà cố bấm tiếp, tôi sẽ nhắc *"Vui lòng điền đầy đủ thông tin hoặc xoá hàng."*
- **Chi tiết Voucher / Yêu cầu thanh toán** — với loại Voucher hoặc Yêu cầu chi trả, bạn bắt buộc điền giá trị và mã (hoặc số tiền và ngân hàng).

Mỗi ô đều có **bộ đếm ký tự thời gian thực** (ví dụ `15/30`) và **tự khóa khi chạm giới hạn** đúng theo quy định của từng loại tham số — tôi sẽ nói kỹ ở [mục 6](#6-giới-hạn-ký-tự-và-quy-tắc-theo-tag).

### Bước 3 — CTA & Gửi template

- **Nút tương tác (CTA)** — không bắt buộc phải có, nhưng **nếu bạn đã thêm nút thì phải điền đủ:** chọn "Mở liên kết" thì phải dán link, chọn "Gọi điện" thì phải nhập số điện thoại.
- **Tích xác nhận** đã đọc nguyên tắc, rồi bấm **Gửi duyệt**.

Lúc này tôi chạy nhịp kiểm duyệt thứ hai (gồm cả AI nếu có), rồi mở bảng kết quả: nếu sạch, bạn sẽ thấy popup báo gửi thành công kèm nút **"Xem lại mẫu đã gửi duyệt"**; nếu còn lỗi, tôi liệt kê từng điểm bằng ngôn ngữ dễ hiểu để bạn quyết định gửi tiếp hay quay về sửa.

### Bước 4 — Theo dõi các mẫu đã gửi

Mọi mẫu bạn đã gửi đều được lưu ở mục **"Mẫu đã gửi duyệt"**. Bạn bấm vào một mẫu để xem trạng thái:

- Mẫu **không có cảnh báo** → tôi hiện thông báo *"Template của bạn đã được gửi tới bộ phận CTV kiểm duyệt thành công."*
- Mẫu **còn cảnh báo** → tôi đưa ra hai lựa chọn: *Quay về màn hình chính* hoặc *Thu hồi & chỉnh sửa lại*. Nếu bạn chọn thu hồi, tôi sẽ nạp lại đúng dữ liệu của mẫu đó và **tô đỏ những ô từng vi phạm** để bạn sửa cho nhanh.

Bạn có thể bấm **"Tạo mới Template"** bất cứ lúc nào (cạnh nhãn JSON) để làm trống toàn bộ và bắt đầu lại từ đầu.

---

## 4. Bộ ba lớp kiểm duyệt: Tự động · AI · CTV

Tôi muốn bạn hiểu rằng mẫu tin của bạn được soát qua **ba lớp bổ trợ cho nhau**, chứ không phải một lớp duy nhất. Mỗi lớp bắt một kiểu lỗi khác nhau:

| Lớp | Khi nào chạy | Bắt loại lỗi nào | Ví dụ |
|---|---|---|---|
| **1. Tự động (deterministic)** | Tức thì, mỗi khi bạn gõ | Lỗi "đếm được" chắc chắn | SĐT/link trong nội dung, sai định dạng tham số, vượt giới hạn ký tự, link nhóm, link rút gọn, thiếu tham số định danh |
| **2. AI ngữ nghĩa** | Khi bấm Gửi duyệt (nếu có AI) | Lỗi cần *hiểu ý nghĩa* | Nội dung không chứng minh được giao dịch, văn phong mê tín/thổi phồng, link "ngụy trang" mời vào nhóm, lỗi chính tả, chủ tài khoản không khớp |
| **3. CTV ZBS (con người)** | Sau khi bạn gửi | Phán quyết cuối cùng & hồ sơ pháp lý | Đối chiếu giấy phép ngành nghề, quyền sở hữu logo/thương hiệu, văn bản ủy quyền… |

Tôi xin nói thẳng về **giới hạn**: có những quy tắc mà tự bản thân nội dung mẫu tin không thể tự chứng minh — ví dụ logo có đúng là của doanh nghiệp bạn không, hay tài khoản nhận tiền có đúng chủ sở hữu OA không. Với những trường hợp đó, tôi chỉ có thể **"phát hiện và nhắc bạn kiểm tra thủ công"**, còn quyết định cuối cùng thuộc về chuyên viên CTV ZBS. Tôi không giả vờ rằng mình duyệt thay được Zalo — tôi giúp bạn vượt qua càng nhiều rào cản càng tốt *trước khi* tới tay họ.

---

## 5. Năm case mẫu — mỗi case tôi muốn chứng minh một điều

Ở khu **"Các case Template mẫu"** trên đầu công cụ, tôi đặt sẵn năm ví dụ. Đây không phải năm mẫu ngẫu nhiên — tôi chọn chúng có chủ đích, mỗi case là một "bằng chứng sống" cho một tình huống kiểm duyệt điển hình. Bạn bấm vào một thẻ là toàn bộ biểu mẫu được điền tự động, rồi bạn thử bấm Gửi duyệt để xem tôi phản ứng thế nào.

| # | Case mẫu | Loại · Tag | Kết quả kỳ vọng | Tôi muốn chứng minh điều gì |
|---|---|---|:---:|---|
| 1 | **Cước Viễn Thông — VNTT** | Yêu cầu chi trả · Tag 1 | 🟢 Đạt | Một mẫu **làm đúng chuẩn**: đủ tên khách hàng + mã hợp đồng, có bảng thông tin rõ ràng, link thanh toán hợp lệ. Đây là hình mẫu để bạn noi theo. |
| 2 | **Mã xác thực OTP — RentA** | Xác thực · Tag 1 | 🟢 Đạt | Mẫu OTP **tối giản và được miễn** yêu cầu tham số định danh giao dịch. Tôi muốn cho bạn thấy không phải mẫu nào cũng cần bảng thông tin dày đặc. |
| 3 | **Báo cáo Toyota** | Tuỳ chỉnh · Tag 2 | 🟡 Cần sửa | Lỗi **để số điện thoại ("0901…") thẳng trong nội dung** thay vì đưa vào nút bấm, lại **thiếu mã giao dịch**. Đây là lỗi phổ biến bậc nhất. |
| 4 | **Báo cáo Fastie** | Tuỳ chỉnh · Tag 2 | 🟡 Cần sửa | Mẫu **chỉ có tên + các con số doanh thu**, không có một mã định danh giao dịch nào (mã đơn, mã hợp đồng…). Tôi muốn chứng minh: "đủ số lượng tham số" chưa chắc là "đủ chứng minh giao dịch". |
| 5 | **Khuyến mãi PJP** | Voucher · Tag 3 | 🔴 Không đạt | Lỗi **nặng**: nút CTA dùng link điều hướng vào nhóm/cộng đồng — điều mà ZBS cấm tuyệt đối. |

Tôi xin diễn giải sâu hơn vì sao năm case này quan trọng:

- **Case 1 và 2 là "ánh sáng dẫn đường".** Chúng cho bạn thấy một mẫu *đạt* trông như thế nào, để bạn có chuẩn mực mà đối chiếu. Riêng case OTP còn dạy bạn một ngoại lệ thú vị: mẫu xác thực được miễn yêu cầu tham số định danh, vì bản thân mã OTP đã gắn với một phiên giao dịch cụ thể.

- **Case 3 và 4 là hai cái bẫy tinh vi nhất.** Cả hai đều trông "có vẻ đủ thông tin", nhưng lại trượt ở chỗ *chất lượng* của thông tin. Toyota mắc lỗi đặt sai vị trí (số điện thoại phải nằm ở nút bấm, không phải trong nội dung). Fastie mắc lỗi sâu hơn: có tên khách và một loạt số liệu doanh thu, nhưng không có gì xác định "đã phát sinh giao dịch cụ thể nào". Đây chính xác là lý do thật khiến rất nhiều mẫu báo cáo định kỳ bị từ chối ngoài đời, và tôi muốn bạn nhận ra cái bẫy này trước khi vấp phải.

- **Case 5 là lằn ranh đỏ.** Link dẫn vào nhóm/cộng đồng trên mạng xã hội là vi phạm nghiêm trọng và gần như chắc chắn bị chặn. Tôi để case này để bạn thấy rõ sự khác biệt giữa "cảnh báo nên xem lại" (màu vàng) và "vi phạm phải sửa" (màu đỏ).

Khi bạn lần lượt bấm thử năm case và đọc kết quả tôi trả ra, bạn sẽ dần "thấm" được tư duy kiểm duyệt của ZBS — và đó mới là giá trị thật mà tôi muốn trao cho bạn.

---

## 6. Giới hạn ký tự và quy tắc theo Tag

### 6.1. Giới hạn ký tự cho từng tham số

Mỗi loại tham số có một giới hạn ký tự riêng theo quy định ZBS. Tôi áp đúng giới hạn này vào từng ô (ô giá trị trong bảng sẽ tự đổi giới hạn theo tên tham số bạn nhập), và hiển thị bộ đếm `đã nhập / tối đa` ngay tại ô đó:

| Nhóm thông tin | Tên tham số tiêu biểu | Giới hạn |
|---|---|:---:|
| Tên khách hàng | `customer_name` | 30 |
| Số điện thoại | `phone_number` | 15 |
| Địa chỉ | `address` | 80 |
| Mã số (đơn/KH/hợp đồng…) | `product_code`, `order_code` | 30 |
| Trạng thái giao dịch | `transaction_status` | 30 |
| Thông tin liên hệ | `contact` | 50 |
| Giới tính / Danh xưng | `personal_title` | 5 |
| Tên sản phẩm / thương hiệu | `product_name` | 100 |
| Số lượng / Số tiền | `amount` | 20 |
| Thời gian | `time` | 20 |
| Nội dung chuyển khoản | `bank_transfer_note` | 90 |

Ngoài ra: **Tiêu đề** tối đa 100, **Nội dung** tối đa 400, **OA ID** tối đa 20, **link nút bấm (CTA)** tối đa 200 ký tự.

### 6.2. Ba mức Tag và ý nghĩa

| Tag | Tên | Dùng cho nội dung gì |
|:---:|---|---|
| **1** | Giao dịch | Liên quan trực tiếp tới một giao dịch cụ thể: mã xác thực, biến động số dư, xác nhận đặt hàng/lịch hẹn, thông báo trạng thái, yêu cầu thanh toán dịch vụ đã dùng |
| **2** | Chăm sóc khách hàng | Cập nhật hoạt động/chính sách, cập nhật tài khoản, khảo sát/đánh giá dịch vụ, quyền lợi khách hàng thân thiết |
| **3** | Hậu mãi | Quảng bá, upsell/cross-sell, mời tải app/kênh, mời tái tục dịch vụ, các chương trình khuyến mãi |

> **Quy tắc vàng tôi luôn nhắc:** nếu nội dung của bạn *bất kỳ chỗ nào* mang tính hậu mãi/khuyến mãi, nó phải gắn **Tag 3** — không có ngoại lệ. Mẫu xác thực luôn là Tag 1, còn mẫu khảo sát/đánh giá luôn là Tag 2. Tôi đã cài sẵn các cảnh báo để nhắc bạn khi loại nội dung và Tag không khớp nhau.

---

## 7. Câu hỏi thường gặp

**Hỏi: Tôi không rành kỹ thuật, JSON kia tôi có cần đụng vào không?**
Hoàn toàn không. Khối JSON chỉ là phần chạy ngầm, tôi hiển thị ra để minh bạch và để kiểm tra logic. Bạn chỉ cần làm việc với biểu mẫu. Khi triển khai thật, mục JSON đó sẽ được ẩn.

**Hỏi: Vì sao tôi gõ một ô mà nó không cho gõ thêm?**
Vì ô đó đã chạm giới hạn ký tự theo quy định ZBS. Bạn nhìn bộ đếm ở góc phải ô (ví dụ `30/30`, hiện màu đỏ) là biết. Bạn cần rút gọn lại nội dung.

**Hỏi: Kết quả "Đạt" ở đây có nghĩa là chắc chắn Zalo sẽ duyệt không?**
Không hẳn. "Đạt" nghĩa là mẫu của bạn đã vượt qua các lớp kiểm tra mà tôi có thể tự động hóa. Quyết định cuối cùng vẫn thuộc về chuyên viên CTV ZBS, đặc biệt với những điều cần hồ sơ pháp lý (giấy phép, quyền sở hữu logo, ủy quyền tài khoản…). Tôi giúp bạn tối đa hóa khả năng được duyệt, chứ không thay thế Zalo.

**Hỏi: Tôi mở bản online trên Vercel mà sao không thấy AI chạy?**
Đó là điều bình thường và đúng như thiết kế. Lớp AI chỉ kết nối được khi công cụ chạy trong môi trường Claude.ai; còn trên bản web công khai (Vercel) hay khi mở file ngoại tuyến, lớp AI sẽ tự ẩn. Tuy vậy, toàn bộ lớp kiểm tra tự động theo bộ quy tắc ZBS vẫn hoạt động đầy đủ, nên mẫu của bạn vẫn được soát rất kỹ về cấu trúc và các quy tắc cứng.

**Hỏi: Tôi lỡ gửi một mẫu còn cảnh báo thì sao?**
Bạn vào mục "Mẫu đã gửi duyệt", bấm vào mẫu đó và chọn "Thu hồi & chỉnh sửa lại". Tôi sẽ nạp lại đúng dữ liệu và tô đỏ những ô cần sửa.

---

## 8. Cách mở file và ghi chú kỹ thuật

### Dành cho người dùng

Cách nhanh nhất là bạn **truy cập bản online** mà tôi đã triển khai trên Vercel:

> 🔗 **https://zbs-studio-nguyenhoangminh.vercel.app/**

Bạn chỉ cần mở đường link này trên trình duyệt bất kỳ (máy tính hay điện thoại đều được) là dùng được ngay, không cần tải gì về, không cần cài đặt gì. Nếu muốn dùng ngoại tuyến, bạn vẫn có thể tải file `zbs_template_studio.html` về và mở trực tiếp bằng trình duyệt — toàn bộ tính năng đều chạy được mà không cần máy chủ.

> **Về lớp AI khi dùng bản online:** lớp kiểm duyệt bằng AI chỉ kết nối được khi công cụ chạy bên trong môi trường Claude.ai. Trên bản web công khai (Vercel) hoặc khi mở file ngoại tuyến, lớp AI sẽ tự ẩn đi, còn **toàn bộ lớp kiểm tra tự động theo bộ quy tắc ZBS vẫn chạy đầy đủ** — nên mẫu của bạn vẫn được soát rất kỹ về cấu trúc, định dạng tham số, giới hạn ký tự và các quy tắc cứng.

---

<div align="center">

**© 2026 ZBS Product Suite · Thiết kế bởi Nguyễn Hoàng Minh**

*Tôi xây công cụ này với một mong muốn giản dị: để mỗi mẫu tin của bạn được duyệt ngay từ lần gửi đầu tiên.*

</div>
