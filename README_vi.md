# Sức khỏe tài chính và mức độ tương tác kênh của khách hàng – BI Season 10 (ITB Club)

Bài dự thi vòng 1 cuộc thi **Business Intelligence Season 10** do ITB Club tổ chức. Bài toán: ITB, một công ty tài chính tiêu dùng tại Việt Nam, muốn hiểu sức khỏe tài chính, cách chi tiêu và mức độ dùng các kênh giao dịch của khách hàng, tìm ra các phân khúc khách hàng và đề xuất hành động hỗ trợ.

**Kết quả chính:** căng thẳng tài chính hiếm và ngắn, và phần lớn tháng căng thẳng có một giao dịch lớn. Sáu công cụ hỗ trợ không mang tính trừng phạt, bắt đầu từ cảnh báo chi tiêu, có thể đến được 89,4% khách hàng. Điểm sức khỏe tài chính không bao giờ được dùng để từ chối cho vay, giảm hạn mức hay khóa tài khoản.

> Phần giải thích trong notebook viết bằng tiếng Việt. Biểu đồ và bộ slide cuối cùng ([slides/BI10_R01_proposal.pdf](slides/BI10_R01_proposal.pdf), 21 slide) viết bằng tiếng Anh. Bản tiếng Anh của README nằm ở [README.md](README.md).

---

## 1. Câu hỏi kinh doanh

| Task | Câu hỏi |
|---|---|
| 1. Phân tích khám phá | Khách chi tiêu khi nào, ở đâu, vào việc gì? |
| 2. Sức khỏe tài chính | Điểm sức khỏe thấp đi cùng với yếu tố nào, và rơi vào nhóm khách nào? |
| 3. Mức độ tương tác | Khách tương tác nhiều đến đâu, qua những kênh nào? |
| 4. Phân khúc | Có những nhóm khách hàng khác biệt nào? |
| 5. Đề xuất | ITB nên hỗ trợ bằng công cụ gì, cho ai, theo thứ tự nào? |

## 2. Dữ liệu

| Bảng | Số dòng | Mỗi dòng là |
|---|---|---|
| Giao dịch 2025 | 1.852.394 | một giao dịch |
| Khách hàng theo tháng 2025 | 10.992 | một khách trong một tháng |

999 khách ở 34 tỉnh, với 396 tên nghề: 908 khách có đủ 12 tháng, 91 khách chỉ có 1–2 tháng. Mọi số tiền đều là giả lập, nên khách được so sánh bằng tỷ lệ và bách phân vị, không so bằng số tiền. Các phát hiện ở cấp khách hàng dùng 908 khách đủ năm; quy mô nhóm mục tiêu của các đề xuất tính trên toàn bộ 999 khách.

Dữ liệu gốc không lưu trong repo này (xem [data/README.md](data/README.md)).

## 3. Phát hiện chính

**Chi tiêu (Task 1).** Tháng 12 chiếm 15,1% chi tiêu cả năm, trong khi tháng 2 chỉ chiếm 5,3% (908 khách đủ 12 tháng). Đỉnh chi tiêu đến từ số giao dịch tăng (+187,6%), không phải giá trị mỗi giao dịch tăng (−2,1%). Ở các tháng điểm dưới 40, 71,0% chi tiêu là không thiết yếu, so với 39,0% ở các tháng điểm từ 80 trở lên. Xăng dầu và di chuyển có nhiều giao dịch nhất, còn siêu thị và tạp hóa có tổng chi tiêu cao nhất. Khách dưới 25 tuổi giao dịch thường xuyên nhưng có điểm sức khỏe thấp nhất trong các nhóm tuổi năng động; dù vậy tuổi chỉ giải thích 0,25% mức chênh lệch của điểm, nên không đủ mạnh để dùng làm tiêu chí phân khúc.

**Sức khỏe tài chính (Task 2).**
- Bốn tỷ lệ theo tháng (chi tiêu so với thu nhập, mức dùng hạn mức, mức chi thất thường, tỷ trọng chi thiết yếu) giải thích 87,8% sự thay đổi của điểm sức khỏe (R²). Điểm gần như chỉ là bản tóm tắt của bốn tỷ lệ này, nên phần phân tích tập trung vào việc các tỷ lệ tăng khi nào và vì sao.
- Căng thẳng hiếm và ngắn: 0,9% số tháng có điểm dưới 40, và 95,7% trong số đó hồi lại ngay tháng sau.
- 73,4% tháng dưới 40 có một giao dịch từ 20% thu nhập tháng của chính khách đó trở lên, so với 0,9% ở các tháng điểm 60–80. Du lịch chiếm 23,7% chi tiêu ở các tháng này, so với 4,1%. Tháng 12 và tháng 1 chiếm 48,9% số tháng dưới 40.
- Ba lĩnh vực truyền thông – nghệ thuật, khu vực công và giáo dục chiếm 29,6% số khách nhưng chiếm 49,3% số khách từng có tháng dưới 40. Thu nhập của họ ở bách phân vị 44, so với 52 ở các lĩnh vực khác, trong khi mức chi tiêu ngang nhau (50).
- Một quy tắc làm lại được (25% điểm sức khỏe thấp nhất và 25% điểm tương tác cao nhất) tìm ra 64 khách "căng thẳng nhưng tương tác cao". Áp lực của họ đến từ chi nhiều (bách phân vị 79), không phải thu nhập thấp (62).

![Tháng căng thẳng chi vào gì](task2_financial_health/charts/s11d_what_stressed_months_buy.png)

**Mức độ tương tác (Task 3).** 99,0% số tháng đã ở mức tương tác "cao" hoặc "rất cao", nên nhãn này gần như không phân biệt được khách. Kênh nào cũng có ít nhất 94,3% khách dùng trong năm, nhưng POS vẫn chiếm 58,82% số giao dịch. 59 khách đủ năm (6,5%) thuộc nhóm khỏe nhất nhưng ít tương tác nhất.

**Phân khúc (Task 4).** K-Means (K = 4) trên 908 khách đủ năm, cộng một nhóm theo quy tắc cho 91 khách ít dữ liệu, cho ra năm phân khúc: Stable & Engaged 32,6%, Stretched & Engaged 20,8%, Lower Engagement & Pressured 19,8%, Emerging Digital 17,6%, Limited History 9,1% (silhouette 0,19). Hai phân khúc chịu áp lực chứa 65 trong 70 khách từng có tháng dưới 40.

## 4. Đề xuất (Task 5)

| Công cụ | Phát hiện làm căn cứ | Nhóm mục tiêu (trên 999) | Mức độ mạnh |
|---|---|---|---|
| Cảnh báo chi tiêu | 73,4% so với 0,9% số tháng có giao dịch ≥ 20% thu nhập | 389 (38,9%) | 79× |
| Nhắc lập kế hoạch | Tháng 12 và tháng 1 chiếm 48,9% số tháng dưới 40 | 687 (68,8%) | 2,9× |
| Công cụ lập ngân sách | Nhóm chi sát thu nhập vượt thu nhập 2,59 tháng mỗi năm so với 1,29 | 229 (22,9%) | 2,0× |
| Nội dung giáo dục tài chính | 12,6% so với 5,5% từng dưới 40 ở ba lĩnh vực nghề | 302 (30,2%) | 2,3× |
| Khuyến khích dùng kênh số | 61,6 so với 182,7 giao dịch mỗi tháng | 150 (15,0%) | 3,0× |
| Gợi ý sản phẩm | Du lịch chiếm 23,7% so với 4,1% chi tiêu ở tháng căng thẳng | 216 (21,6%) | 5,8× |

Mức độ mạnh = vấn đề xuất hiện ở nhóm mục tiêu nhiều gấp bao nhiêu lần so với các khách khác. Sáu công cụ cộng lại đến được 893 khách (89,4%). Thứ tự triển khai đề xuất bắt đầu từ cảnh báo chi tiêu (mạnh nhất, độ phủ rộng thứ hai), sau đó là nhắc lập kế hoạch trước tháng 12.

![Ma trận ưu tiên](task5_recommendations/charts/s22_priority_matrix.png)

## 5. Thành viên và phần đóng góp

| Thành viên | Vai trò |
|---|---|
| Nguyễn Hoàng Anh Duy | Trưởng nhóm. Chốt định nghĩa chung và ghép bài cuối cùng; Task 2 (sức khỏe tài chính) và Task 5 (đề xuất); tóm tắt và phần giới thiệu bài toán |
| Nhi | Task 1 – phân tích khám phá |
| Giang | Task 1 – phân tích khám phá |
| Nguyên | Task 3 – mức độ tương tác |
| Dương | Task 4 – phân khúc |

## 6. Cấu trúc repo

```
├── slides/BI10_R01_proposal.pdf        bộ slide 21 trang
├── data/                               đặt dữ liệu gốc vào đây (không đưa lên repo)
├── task1_eda/                          3 notebook:
│     part1  Q1, Q2, Q5 trên 908 khách đủ 12 tháng (slide 1, 5, 8)
│     part2  kiểm tra dữ liệu và trả lời Q1–Q5 trên cả 999 khách
│     part3  mùa vụ, danh mục, tỉnh và kênh digital trên cả 999 khách (slide 4, 6, 7)
├── task2_financial_health/             notebook, charts/, outputs/
├── task3_engagement/                   notebook
├── task4_segmentation/                 notebook, outputs/ (bảng khách đã xử lý)
└── task5_recommendations/              notebook, inputs/ (phân khúc Task 4), charts/, outputs/
```

## 7. Cách chạy lại

1. Đặt bốn file liệt kê trong [data/README.md](data/README.md) vào thư mục `data/`.
2. Cài Python 3.10 trở lên cùng `pandas`, `numpy`, `scipy`, `scikit-learn`, `matplotlib` và `jupyter`.
3. Chạy từng notebook từ chính thư mục của nó. Task 5 đọc kết quả của Task 2 và Task 4, nên chạy hai task đó trước.

## 8. Giới hạn

- Dữ liệu giả lập, chỉ một năm (2025); các phát hiện theo mùa cần thêm một năm dữ liệu để kiểm chứng.
- Các phát hiện cho thấy những yếu tố đi cùng nhau, chưa phải nhân quả. Mỗi đề xuất nên chạy thử có nhóm đối chứng trước.
- Mốc giao dịch lớn 20% thu nhập chưa được thử với các mốc khác.
- Không có dữ liệu về việc khách đồng ý nhận thông báo, nên quy mô nhóm mục tiêu là số khách đủ điều kiện, không phải số khách chắc chắn nhận được.
- Các notebook Task 1 dùng hai nhóm khách: part1 dùng 908 khách đủ 12 tháng, part3 dùng cả 999 khách. Vì vậy tháng 12 là 15,1% ở slide 1 và 15,04% ở slide 4.
- Trong Task 1 part2, Q3 chỉ tính QR và thương mại điện tử là digital. Part3 và Task 3 tính mọi kênh trừ POS, theo đúng data dictionary.
