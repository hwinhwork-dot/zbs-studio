# Rule Map — Quy định kiểm duyệt mẫu tin nhắn ZBS

**Mục đích tài liệu:** hệ thống hóa toàn bộ quy tắc mà một template ZBS cần thỏa để qua kiểm duyệt, làm nền cho Bước 2 (chọn rule để tự động hóa) và cho tool validation HTML sẽ build sau.

**Phương pháp:** AI trích xuất bản nháp từ văn bản gốc → PO (review thủ công) đối chiếu lại từng dòng, sửa các điểm bị tổng quát hóa sai hoặc trùng lặp giả. Mỗi rule có ID để trace ngược khi build logic ở Bước 2.

**Tổng quan phạm vi:** 4 tầng quy định, ~75 rule nguyên tử.

| Tầng | Số rule | Áp dụng |
|---|---|---|
| I. Phân loại Tag | 6 | Mọi template |
| II.1 Yêu cầu tổng quan | 41 | Mọi template |
| II.2 Yêu cầu theo Tag | 22 | Theo Tag 1/2/3 |
| II.3 Mục đích đặc biệt | 18 | Sinh nhật KH thân thiết, Lễ Tết |
| II.4 Ngành đặc biệt | 28 | Theo ngành nghề (chỉ áp dụng khi Tag 3) |

---

## I. Phân loại Tag — mọi template phải gắn đúng 1 trong 3 Tag

| ID | Rule |
|---|---|
| TAG-01 | Mọi template bắt buộc gắn đúng 1 trong 3 Tag: Giao dịch / Chăm sóc khách hàng / Hậu mãi. |
| TAG-02 | **Tag 1 – Giao dịch**: nội dung liên quan cụ thể đến từng giao dịch (mã xác thực, biến động số dư, xác nhận đặt hàng/lịch hẹn/đặt chỗ, thông báo trạng thái giao dịch, yêu cầu thanh toán dịch vụ đã dùng...). |
| TAG-03 | **Tag 2 – Chăm sóc khách hàng**: cập nhật hoạt động/chính sách DN (không liên quan upsell/cross-sell/hậu mãi), cập nhật tài khoản, khảo sát/đánh giá dịch vụ, quyền lợi khách hàng thân thiết đang tham gia có thể lệ rõ ràng. |
| TAG-04 | **Tag 3 – Hậu mãi**: quảng bá/upsell/cross-sell sản phẩm-dịch vụ, mời tải/dùng tính năng-kênh-app, mời tái tục/gia hạn dịch vụ, các chương trình hậu mãi khác. |
| TAG-05 | Quy tắc xác định Tag khi mẫu có nhiều mục đích chồng lắp: (a) nếu BẤT KỲ mục đích nào được xác định hoặc *gây hiểu lầm* là Hậu mãi → luôn gắn Tag 3, không ngoại lệ; (b) nếu tổ hợp gồm Giao dịch + Chăm sóc KH (không dính Hậu mãi) → gắn Tag 1; (c) mẫu Xác thực luôn gắn Tag 1; (d) mẫu Đánh giá dịch vụ luôn gắn Tag 2. |
| TAG-06 | ZBS Template Message chỉ gửi được cho user **đã từng phát sinh giao dịch** với DN. Ngoại lệ duy nhất: gửi mã xác thực (OTP) cho khách tạo tài khoản mới. |

---

## II.1 Yêu cầu tổng quan (áp dụng mọi template, không phân biệt Tag)

### Mục đích & đối tượng nhận tin
| ID | Rule |
|---|---|
| GEN-01 | Nội dung phải thể hiện đúng mục đích đã đăng ký khi tạo mẫu. |
| GEN-02 | Mẫu tin phải thể hiện rằng người nhận đã từng có giao dịch với DN (trừ ngoại lệ TAG-06). |

### Ngôn ngữ / Văn phong / Nội dung chung
| ID | Rule |
|---|---|
| GEN-03 | Đúng chính tả. |
| GEN-04 | Không có lỗi đánh máy (typo). |
| GEN-05 | Nội dung tiếng Việt phải có dấu. |
| GEN-06 | Dùng đồng nhất MỘT ngôn ngữ, không pha trộn (VD Anh–Việt). |
| GEN-07 | Nội dung thể hiện rõ mục đích, không gây khó hiểu cho người nhận. |
| GEN-08 | Văn phong phải tương ứng với mục đích/Tag của mẫu tin. |
| GEN-09 | Không chèn link hoặc số điện thoại trong nội dung — phải đặt ở CTA (liên kết GEN-26). |
| GEN-10 | Hạn chế viết tắt. |
| GEN-11 | Không dùng icon, ký tự đặc biệt. |
| GEN-12 | Không dùng từ ngữ/văn phong mang tính (hoặc gây hiểu lầm) mê tín, lừa gạt, thần thánh hóa sản phẩm. VD vi phạm: *tín chủ, thầy pháp, bùa ngãi*. |
| GEN-13 | Văn phong/từ ngữ phải chuyên nghiệp, đồng bộ, cụ thể, diễn giải đúng bối cảnh và mục đích. |

### Logo
| ID | Rule |
|---|---|
| GEN-14 | Logo canh sát lề trái. |
| GEN-15 | Logo có bản phù hợp cho light mode & dark mode, thể hiện rõ ràng. |
| GEN-16 | Màu sắc/hình ảnh logo không lỗi (lem, loang lỗ nền, bẹp...). |
| GEN-17 | Logo tuân chuẩn kích thước quy định. |
| GEN-18 | Logo trùng tên OA/DN phải sở hữu hoặc có quyền sử dụng; nếu không khớp tên OA cần giấy tờ chứng minh (giấy chứng nhận nhãn hiệu / giấy ủy quyền / hợp đồng / giấy chứng nhận tác giả). |
| GEN-19 | Logo không đính kèm SĐT/link, trừ khi đó là thiết kế mặc định công khai trên website chính thức của DN. |
| GEN-20 | Logo darkmode & lightmode phải có tỉ lệ/kích thước tương đương nhau. |

### Tham số
| ID | Rule |
|---|---|
| GEN-21 | Tham số không dấu, không khoảng trắng, không gạch nối. |
| GEN-22 | Mỗi từ trong tham số ngăn cách bằng gạch dưới (`_`). |
| GEN-23 | Tham số bắt đầu bằng `<` kết thúc bằng `>`. |
| GEN-24 | Không dùng "anh/chị" để xưng hô — dùng biến giới tính hoặc từ trung tính (bạn, quý khách, khách hàng, hội viên, nhân viên...). |
| GEN-25 | Mỗi tham số cần có tiền tố giải thích rõ ý nghĩa trong nội dung (VD: "mã hội viên `<member_code>`"). |

### Nút thao tác (CTA)
| ID | Rule |
|---|---|
| GEN-26 | Link & SĐT/hotline **bắt buộc** đặt ở CTA — không nằm trong nội dung mẫu tin. |
| GEN-27 | Không dùng link rút gọn (bitly, onelink...) hoặc link bị cảnh báo truy cập. |
| GEN-28 | Không dùng link không khả dụng (lỗi truy cập / thiếu thông tin công khai rõ ràng). |
| GEN-29 | Không dùng link điều hướng đến nhóm/nhóm chat trên MXH (Zalo, Facebook, Telegram...), Messenger, Zalo cá nhân. |
| GEN-30 | Nếu nội dung nhắc đến app/kênh/phương thức cụ thể → CTA phải dẫn đúng đến app/kênh đó. **Ngoại lệ:** ngành điện/nước với mục đích thông báo hóa đơn — chỉ cần liệt kê kênh thanh toán, không bắt buộc CTA. |
| GEN-31 | Mẫu KHÔNG thuộc Tag Hậu mãi: link CTA không được chứa thông tin ưu đãi không liên quan / gây hiểu lầm là khuyến mãi, quảng cáo, mời mua hàng, mời đăng ký, mời tải app. |
| GEN-32 | Không hỗ trợ CTA dẫn đến kịch bản chatbot. |
| GEN-33 | Link CTA phải liên quan/có mối quan hệ với nội dung mẫu tin. |
| GEN-34 | CTA loại "Đối tác Dịch vụ": brand name domain phải khớp tên OA (đồng nhất ngôn ngữ) hoặc có ủy quyền sử dụng domain từ chủ sở hữu. |

### Thiết lập (áp dụng từ 15/08/2024)
| ID | Rule |
|---|---|
| GEN-35 | Mẫu có mục đích gửi mã giảm giá/voucher (mọi Tag) bắt buộc dùng **Mẫu Voucher**. Ngoại lệ: (a) gửi >1 voucher trong 1 tin; (b) không có mã voucher cụ thể — user đổi/nhận mã tại app/mini-app/kênh (nội dung phải nêu cách dùng + CTA dẫn đến kênh đó). |

### Khác
| ID | Rule |
|---|---|
| GEN-36 | Tuân thủ chính sách chung nền tảng Zalo & chính sách dịch vụ ZBS Template Message. |
| GEN-37 | Nếu nội dung nhắc tên/thương hiệu DN khác → phải cung cấp văn bản hợp tác hoặc thông tin công khai xác thực mối quan hệ hợp tác. |
| GEN-38 | Không hỗ trợ nội dung/mục đích (ở cấp ý định, không chỉ CTA) điều hướng đến nhóm/nhóm chat trên MXH, Messenger, Zalo cá nhân. |
| GEN-39 | Ngành nghề có điều kiện hoặc cần hậu kiểm — Zalo có quyền yêu cầu bổ sung thông tin/văn bản/giấy phép. |
| GEN-40 | STK ngân hàng & chủ tài khoản chuyển khoản phải là thông tin của chính DN sở hữu OA, trừ khi cung cấp giấy tờ chứng minh (chủ DN/người đại diện pháp lý, hoặc văn bản ủy quyền thu hộ). |
| GEN-41 | Nội dung thông báo nội bộ cho nhân viên DN phải có cụm "Thông báo nội bộ". |

---

## II.2 Yêu cầu theo Tag

### Tag 1 — Giao dịch
| ID | Rule |
|---|---|
| T1-01 | Mục đích phải liên quan đến một giao dịch cụ thể. |
| T1-02 | Bắt buộc có tên khách hàng **+** ít nhất 1 tham số định danh/xác định giao dịch (mã đăng ký, mã đơn hàng, mã hóa đơn, mã khách hàng, ngày mua gần nhất...). |
| T1-03 | Nếu bất khả kháng không cung cấp được tên KH → cần tổng cộng **≥ 3 tham số** định danh/giao dịch phù hợp. |
| T1-04 | Số lượng/loại tham số phải đủ để làm rõ đã phát sinh giao dịch, xét theo nội dung + mục đích + mối quan hệ DN-user. |
| T1-05 | *Nhóm Xác thực*: xác thực tài khoản/giao dịch, xác nhận đặt hàng/lịch hẹn/đặt chỗ, xác nhận đăng ký dịch vụ-sự kiện (không mời tham gia), xác nhận đăng ký CTKM thành công (bắt buộc cung cấp cách thức đăng ký ở ghi chú kiểm duyệt), xác nhận giao dịch hoàn thành. |
| T1-06 | *Nhóm Nhắc hẹn*: nhắc hẹn cho sự kiện (tái khám, bảo dưỡng định kỳ, thanh toán, tham gia sự kiện) — **KHÔNG** bao gồm nhắc tái tục dịch vụ (thuộc Tag 3). |
| T1-07 | *Nhóm Thông báo*: trạng thái giao dịch, thời gian thực hiện giao dịch, thanh toán thành công, biến động số dư. |
| T1-08 | *Nhóm Yêu cầu thanh toán/cước/phí*: nếu đến 1 STK ngân hàng cụ thể → bắt buộc dùng Mẫu Yêu Cầu Thanh Toán; nếu trên kênh thanh toán độc lập khác → dùng Mẫu Tùy Chỉnh nhưng CTA bắt buộc dẫn về kênh đó; STK & chủ TK phải đúng DN sở hữu OA (= GEN-40). |

### Tag 2 — Chăm sóc khách hàng
| ID | Rule |
|---|---|
| T2-01 | Cập nhật thông tin DN & chương trình CSKH cho user. |
| T2-02 | Bắt buộc tên KH + ít nhất 1 tham số định danh/giao dịch (cùng logic T1-02/03/04). |
| T2-03 | *Thông báo/Cập nhật*: tích lũy điểm thưởng (KHÔNG giới thiệu sp/dv khác), tình hình học tập, chuyển đổi kênh/website/app (bắt buộc có thông báo chính thức công khai trên kênh DN qua CTA hoặc ghi chú; nếu chuyển sang Zalo OA thì KHÔNG được dẫn CTA mời quan tâm OA — tránh nhầm với Tag 3). |
| T2-04 | *Yêu cầu*: cập nhật hoạt động/chính sách, cập nhật tài khoản/hạng tài khoản, biến động số dư/hạn mức/loyalty, đổi/đóng địa điểm kinh doanh. |
| T2-05 | *Hỗ trợ/Hướng dẫn*: hướng dẫn sử dụng sp/dv đã mua (cần đủ biến xác định đã sở hữu/sử dụng), cảnh báo rủi ro có khả năng xảy ra. |
| T2-06 | *Khảo sát*: cần có phát sinh giao dịch rõ ràng; nếu kèm quà tặng/voucher cho khảo sát → cần đủ thông tin thể lệ + cách nhận (mức KM, điều kiện, HSD). |
| T2-07 | *Quyền lợi KH thân thiết*: sinh nhật (bắt buộc hình ảnh + quà tặng/quyền lợi loyalty công khai — chi tiết ở mục III); quyền lợi tài khoản (có sẵn, công khai tại trang chính sách DN); trả thưởng (nội dung thể hiện rõ đã tham gia chương trình, bắt buộc đính kèm chi tiết ở ghi chú); tặng voucher ngay sau giao dịch (bắt buộc dùng Mẫu Voucher). |

### Tag 3 — Hậu mãi
| ID | Rule |
|---|---|
| T3-01 | Quảng bá/upsell/cross-sell sản phẩm-dịch vụ DN. |
| T3-02 | CTA về hotline DN phải dùng SĐT dạng tổng đài đầu số **1800 hoặc 1900**; nếu dùng số khác → phải cung cấp chứng minh đây là hotline chính thức (công khai trên website/kênh chính thống + đính kèm link ở ghi chú). |
| T3-03 | Không viết tắt. |
| T3-04 | Mục đích Chúc mừng Lễ Tết → bắt buộc áp dụng điều kiện bổ sung (xem mục III.2). |
| T3-05 | Mục đích khuyến mãi/ưu đãi → nội dung phải thể hiện rõ thể lệ, điều kiện áp dụng, hạn sử dụng. |
| T3-06 | Bắt buộc có tên khách hàng; số lượng tham số định danh theo nhu cầu làm rõ nội dung/mục đích/mối quan hệ. |
| T3-07 | *Nhóm mục đích*: giới thiệu sp/dv cũ-mới, thông báo mã giảm giá/CTKM, hậu mãi mùa Lễ Tết, mở mới/mở rộng dv, **quảng bá kênh mới (CHỈ cho phép Zalo OA hoặc Zalo Mini App — không áp dụng cho kênh khác)**, mời tái tục/gia hạn dịch vụ (nếu thanh toán tái tục đến 1 STK cụ thể → bắt buộc Mẫu Yêu Cầu Thanh Toán). |

---

## II.3 Quy định bổ sung — mục đích đặc biệt

### Sinh nhật khách hàng thân thiết
| ID | Rule |
|---|---|
| SP-01a | Bắt buộc sử dụng hình ảnh trong mẫu. |
| SP-01b | Nếu có mã giảm giá/voucher → bắt buộc dùng Mẫu Voucher. |
| SP-01c | Bắt buộc đính kèm thông tin chương trình loyalty/khách hàng thân thiết công khai trên kênh chính thức DN. |
| SP-01d | **KHÔNG hỗ trợ** mục đích sinh nhật nếu không kèm khuyến mãi/voucher/ưu đãi hợp lệ cho khách hàng. |

### Chúc mừng Lễ Tết / Hậu mãi mùa Lễ Tết
| ID | Rule |
|---|---|
| SP-02a | Bắt buộc sử dụng hình ảnh trong template. |
| SP-02b | Bắt buộc kèm thông tin quà tặng/voucher/chương trình hợp lệ trong nội dung. |
| SP-02c | Ngày gửi phải rơi vào 1 trong các khung Lễ Tết quy định (bảng 16 dịp — Tết Dương Lịch 31/12–01/01; Tết Nguyên Đán 23/12–07/01 âm lịch; Valentine 14/02; QT Phụ nữ 08/03; Giỗ Tổ Hùng Vương 29/04; Giải phóng miền Nam 30/04; QT Lao động 01/05; Ngày của mẹ – CN thứ 2 tháng 5; QT Thiếu nhi 01/06; Ngày của cha – CN thứ 3 tháng 6; Quốc khánh 02/09; Trung thu 15/8 âm lịch; Phụ nữ VN 20/10; Halloween 31/10; Nhà giáo VN 20/11; Black Friday – thứ 6 lần 4 tháng 11; Giáng sinh 24–25/12). |
| SP-02d | **KHÔNG hỗ trợ** nếu không kèm khuyến mãi/voucher/ưu đãi hợp lệ. |
| SP-02e | Chương trình KM độc lập trùng ngày lễ tết: không bắt buộc theo SP-02a/b nhưng phải có tên/thể lệ riêng, không gây hiểu lầm là CTKM mùa lễ tết. |

---

## II.4 Yêu cầu theo ngành/nhóm sản phẩm đặc biệt (chỉ áp dụng khi Tag 3, trừ khi ghi rõ khác)

| ID | Ngành | Rule |
|---|---|---|
| IND-a | Mỹ phẩm, thẩm mỹ viện | Bắt buộc công khai thông tin chương trình/sp trên website bán hàng chính thức; nếu liên quan dịch vụ xâm lấn (phẫu thuật, căng chỉ, tiêm filler) → bắt buộc bổ sung giấy phép hành nghề + giấy phép kinh doanh + giấy phép quảng cáo dịch vụ tương ứng. |
| IND-b | Sản phẩm sinh lý / phẫu thuật thẩm mỹ sinh lý | **Không hỗ trợ đăng ký Tag 3**, mọi mục đích. |
| IND-c | Dịch vụ tang lễ | **Không hỗ trợ đăng ký Tag 3**, mọi mục đích. |
| IND-d | Rượu, bia, đồ uống có cồn (< 5.5 độ) | Bắt buộc thêm cụm "Sản phẩm không dành cho người dưới 18 tuổi"; bắt buộc CTA dẫn về trang/website có công nghệ kiểm soát tuổi truy cập (VD popup xác nhận tuổi). |
| IND-e | Rượu, bia, đồ uống có cồn (5.5–15 độ và ≥ 15 độ) | **Không hỗ trợ đăng ký Tag 3**, mọi mục đích. |
| IND-f | Thực phẩm chức năng | Bắt buộc miêu tả rõ chức năng/tác dụng sản phẩm; bắt buộc có cụm "Sản phẩm không phải là thuốc và không có tác dụng thay thế thuốc chữa bệnh"; không chứa cụm gây hiểu lầm là thuốc (nội dung & CTA); bắt buộc công khai chương trình/sp trên website bán hàng chính thức. |
| IND-g | Phong thủy / tử vi | Chỉ cho phép nội dung giới thiệu sản phẩm/dịch vụ KHÔNG mang tính (hoặc gây hiểu lầm) mê tín, lừa gạt, thần thánh hóa. VD vi phạm: *giải hạn, giải vận, giàu phất lên nhờ mua nhẫn/vòng*. |
| IND-h | Thuốc không kê đơn | Bắt buộc đủ giấy tờ: giấy chứng nhận ĐKKD, GXN ngành nghề, chứng nhận đại lý/hợp đồng/hóa đơn mua thuốc, giấy chứng nhận đủ điều kiện kinh doanh dược, GXN thuốc không kê đơn hoặc công văn mô tả thành phần xác định không kê đơn. Thuốc bị khuyến cáo hạn chế/cần giám sát thầy thuốc → **KHÔNG ĐƯỢC** đăng ký Tag 3. |
| IND-i-01 | Cấm tuyệt đối Tag 3 | Sữa thay thế sữa mẹ cho trẻ < 24 tháng / dinh dưỡng bổ sung trẻ < 6 tháng / bình bú-vú ngậm nhân tạo. |
| IND-i-02 | Cấm tuyệt đối Tag 3 | Thuốc trái phép, thuốc theo toa hoặc thuốc kích thích. |
| IND-i-03 | Cấm tuyệt đối Tag 3 | Thuốc kê đơn; thuốc không kê đơn bị khuyến cáo hạn chế/cần giám sát thầy thuốc. |
| IND-i-04 | Cấm tuyệt đối Tag 3 | Sản phẩm/hàng hóa có tính kích dục; đồ chơi tình dục hoặc sản phẩm tập trung khoái cảm tình dục. |
| IND-i-05 | Cấm tuyệt đối Tag 3 | Sản phẩm hoặc dịch vụ người lớn. |
| IND-i-06 | Cấm tuyệt đối Tag 3 | Sản phẩm thuốc lá và liên quan đến thuốc lá. |
| IND-i-07 | Cấm tuyệt đối Tag 3 | Vũ khí, đạn dược, chất gây cháy nổ, sản phẩm kích động bạo lực. |
| IND-i-08 | Cấm tuyệt đối Tag 3 | Dịch vụ/ấn bản phẩm (sách, báo, game, trang TTĐT) không có giấy phép phát hành. |
| IND-i-09 | Cấm tuyệt đối Tag 3 | Thiết bị/phần mềm ngụy trang ghi âm-ghi hình chưa có/bị thu hồi giấy chứng nhận an ninh trật tự. |
| IND-i-10 | Cấm tuyệt đối Tag 3 | Sản phẩm liên quan động/thực vật rừng nguy cấp, quý, hiếm. |
| IND-i-11 | Cấm tuyệt đối Tag 3 | Dịch vụ kinh doanh vàng/ngoại hối chưa được Ngân hàng Nhà nước chấp thuận bằng văn bản. |
| IND-i-12 | Cấm tuyệt đối Tag 3 | Sản phẩm/dịch vụ tài chính gây nhầm lẫn/lừa đảo (quyền chọn nhị phân, ICO, tiền ảo, đấu giá kiểu thầu...). |
| IND-i-13 | Cấm tuyệt đối Tag 3 | Mô hình kinh doanh đa cấp/thu nhập nhanh không mô tả rõ ràng, hứa hẹn thù lao cao bất hợp lý. |
| IND-i-14 | Cấm tuyệt đối Tag 3 | Mô hình đầu tư hợp đồng kỳ nghỉ (timeshare). |
| IND-i-15 | Cấm tuyệt đối Tag 3 | Trò chơi có tính may rủi (cá cược thể thao, bingo, poker, bet). |
| IND-i-16 | Cấm tuyệt đối Tag 3 | Hàng giả, hàng nhái gây nhầm lẫn với nhãn hiệu nổi tiếng. |
| IND-i-17 | Cấm tuyệt đối Tag 3 | Sản phẩm có tính chất mê tín dị đoan. |
| IND-i-18 | Cấm tuyệt đối Tag 3 | Sản phẩm/hàng hóa/dịch vụ khác bị cấm quảng cáo theo quy định pháp luật hiện hành. |
| IND-i-19 | Cấm tuyệt đối Tag 3 | (Catch-all) Sản phẩm khác mà Zalo Business Solutions đánh giá là không an toàn/không phù hợp với người dùng. |

---

## Ghi chú giới hạn dữ liệu đầu vào

Một số rule (GEN-18, GEN-40, IND-a/f/h) yêu cầu đối chiếu với **giấy tờ pháp lý hoặc tên doanh nghiệp đăng ký OA** — thông tin này thường KHÔNG có sẵn trong JSON template (JSON chỉ chứa nội dung hiển thị + cấu hình UI, không chứa hồ sơ pháp lý OA). Đây là giới hạn cần ghi nhận khi sang Bước 2: các rule này chỉ có thể tự động hóa ở mức "phát hiện điều kiện áp dụng → flag cần xác minh thủ công", không thể tự kết luận đúng/sai.

---

## III. Các định dạng dữ liệu khi truyền vào tham số ZBS (Bổ sung mới)

**Phạm vi áp dụng:** Quy định bắt buộc về giới hạn ký tự, kiểu dữ liệu, và cách thức mã hóa khi truyền dữ liệu vào các tham số (parameters) của mẫu tin nhắn ZBS.

### III.1 Tham số trong nội dung chính
| ID | Quy định |
|---|---|
| PRM-01 | **Không cần mã hóa:** Đối với các tham số được truyền vào ở nội dung chính của template, không cần thực hiện mã hóa, doanh nghiệp có thể giữ nguyên dữ liệu gốc khi truyền vào. |
| PRM-02 | Doanh nghiệp phải chọn đúng cài đặt kỹ thuật cho từng tham số và truyền đúng định dạng cũng như giới hạn ký tự vào tham số đó. |

**Bảng chi tiết các tham số phổ biến trong nội dung chính:**

| Tên tham số | Cài đặt kỹ thuật | Giới hạn ký tự | Kiểu dữ liệu | Ví dụ dữ liệu truyền vào |
|---|---|---|---|---|
| `customer_name` | Tên khách hàng (30) | 30 | chuỗi (string) | Nguyễn Văn A |
| `phone_number` | Số điện thoại (15) | 15 | chuỗi (string) | 096983453x |
| `address` | Địa chỉ (80) | 80 | chuỗi (string) | 104 Quang Trung |
| `product_code` | Mã số (30) | 30 | chuỗi (string) | TP-34512 |
| `custom_field` | Nhãn tùy chỉnh (30) | 30 | chuỗi (string) | Mẫu nội dung tùy chỉnh |
| `transaction_status` | Trạng thái giao dịch (30) | 30 | chuỗi (string) | Giao dịch thành công |
| `contact` | Thông tin liên hệ (50) | 50 | chuỗi (string) | 09698453 |
| `personal_title` | Giới tính / Danh xưng (5) | 5 | chuỗi (string) | Chị |
| `product_name` | Tên sản phẩm / Thương hiệu (100) | 100 | chuỗi (string) | Bàn phím Raz |
| `amount_in_standard` | Số lượng / Số tiền (20) | 20 | số (number) | Truyền số nguyên: 1000 (hiển thị 1,000)<br>Truyền số thập phân: 0.3 (hiển thị 0.3) |
| `Time` | Thời gian (20) | 20 | thời gian (datetime) | Các định dạng: `hh:mm:ss dd/mm/yyyy`, `hh:mm dd/mm/yyyy`, `dd/mm/yyyy`, `mm/yyyy` |
| `bank_transfer_note` | Bank Transfer Note (90) | 90 | Bank Transfer Name | Không cho phép các ký tự đặc biệt |

### III.2 Tham số tại nút thao tác (CTA)
| ID | Quy định |
|---|---|
| CTA-01 | **Mã hóa URL:** Doanh nghiệp *nên* mã hóa (encode) các dữ liệu truyền vào tham số ở đường liên kết CTA theo chuẩn UTF-8 (VD: `Nguyễn Văn A` thành `Nguy%E1%BB%85n+V%C4%83n+A`). Điều này giúp Zalo giải mã chính xác mà không gây lỗi. |
| CTA-02 | **Phạm vi mã hóa:** CHỈ mã hóa dữ liệu truyền vào tham số, KHÔNG mã hóa toàn bộ đường liên kết vì sẽ ảnh hưởng đến chất lượng template. |
| CTA-03 | **Tên biến khác biệt:** Các tham số trong URL bắt buộc phải có tên KHÁC với các tham số đã được định nghĩa trong phần nội dung chính hoặc tiêu đề của Template. |
| CTA-04 | **Cấu trúc URL hợp lệ:** Sau khi định dạng, liên kết (URL) truyền vào CTA sẽ có dạng chuẩn: `https://example.com/param?customer_name=<param>URL</param>` |

### III.3 Các định dạng dữ liệu khác (Template tạo qua yêu cầu đặc biệt)
| Kiểu dữ liệu | Đặc điểm cho phép | Ví dụ truyền vào |
|---|---|---|
| **Number** | Cho phép ký tự số. Cho phép số dương, số âm và số thập phân (ngăn cách phần nguyên và thập phân bằng dấu phẩy `,`). | `123`, `100000`, `-100`, `-100,2` |
| **DateTime** | Các định dạng được hỗ trợ. | `hh:mm:ss dd/mm/yyyy`, `hh:mm dd/mm/yyyy`, `dd/mm/yyyy`, `mm/yyyy` |
| **Currency** | Chỉ cho phép các ký tự số 0-9. Chỉ cho phép số dương. | Dữ liệu định dạng tiền tệ hợp lệ |
| **QR code** | Cho phép các ký tự trong bảng mã UTF-8 (Chữ cái, số, ký tự đặc biệt). | Dữ liệu mã QR |
| **Bank Transfer Note**| Không cho phép truyền các ký tự đặc biệt trên bàn phím di động. | Nội dung chuyển khoản hợp lệ |
| **BIN Code** | Mã số theo bảng BIN Code. | Mã ngân hàng hợp lệ |

---

## IV. Thiết lập mục đích gửi và tham số định danh mẫu tin (Bổ sung mới)

**Phạm vi áp dụng:** Quy định về nhóm các tham số định danh (tham số tối thiểu) cần phải có tương ứng với từng loại mục đích gửi để xác thực được tính hợp lệ của giao dịch và thiết lập đúng chuẩn của Zalo.

| Mục đích gửi | Giao diện | Yêu cầu tham số định danh (Bắt buộc) |
|---|---|---|
| **1. Mã xác thực (OTP)** | Sáng (Light) & Tối (Dark) | Mã OTP ngẫu nhiên sinh ra từ hệ thống. |
| **2. Xác nhận cập nhật thông tin giao dịch** (VD: Xác nhận đơn hàng) | Chăm sóc khách hàng (CSKH) | `<ten_khach_hang>`, `<ma_don_hang>`, `<ngay_mua_hang>`, `<gia_tien>`, `<hinh_thuc_thanh_toan>` |
| **3. Thông báo hoàn tất giao dịch và hỗ trợ** (VD: Giao hàng thành công) | Chăm sóc khách hàng (CSKH) | `<ten_khach_hang>`, `<ma_don_hang>`, `<ngay_mua_hang>` |
| **4. Cập nhật thông tin tài khoản** (VD: Thông báo điểm tích lũy) | Chăm sóc khách hàng (CSKH) | `<ten_khach_hang>`, `<ma_khach_hang>`, `<diem>`, `<han_su_dung>` |
| **5. Cập nhật thay đổi về sản phẩm, dịch vụ** (VD: Thông báo bảo trì hệ thống) | Chăm sóc khách hàng (CSKH) | `<ten_khach_hang>`, `<ma_khach_hang>` |
| **6. Thông báo hậu mãi đến khách hàng cũ** (VD: Mời làm khảo sát / Voucher) | Chăm sóc khách hàng (CSKH) | `<ten_khach_hang>`, `<topic_khao_sat>`, `<ma_voucher>` |