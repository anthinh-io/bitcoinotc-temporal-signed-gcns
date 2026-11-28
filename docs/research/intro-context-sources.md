# Nguồn dẫn chứng cho bối cảnh thực tế và tình hình nghiên cứu trong nước

Ticket: #17 (map #16). Ngày tra cứu: 2026-11-28.

**Phạm vi.** Ghi chú này chỉ thu thập nguồn cho phần mở đầu: "Lý do chọn đề tài và bối cảnh bài toán", "Tình hình nghiên cứu trong nước" và "Ý nghĩa thực tiễn". Các nội dung sau đã có ở ticket đã đóng nên không tra lại, chỉ dẫn chiếu:

- Cách chèn bất thường và phê bình về cạnh chèn ngẫu nhiên: #2.
- Truyền tin có dấu (SGCN, SDGNN, MGS-TGCN) và hỗ trợ của PyG: #3.
- Giao thức đánh giá và các số AUC đã công bố trên Bitcoin-OTC: #4.
- Thống kê bộ dữ liệu SNAP: 35.592 cạnh, 5.881 nút, 10% cạnh âm, không có cặp nào được chấm hai lần: #5.
- Định nghĩa Người dùng gian lận theo REV2 §5.1 và giả định "người sáng lập là node 1": #11.

**Quy ước.**

- Mỗi mục gồm: nhận định, nguồn (tiêu đề, tác giả hoặc tổ chức, năm, URL/DOI, trang hoặc mục), trích dẫn nguyên văn, và ghi chú độ tin cậy.
- Mọi trích dẫn được lấy nguyên văn từ văn bản tải về bằng `curl` hoặc `pdftotext`, trừ khi có ghi khác.
- Trích dẫn tiếng Anh có kèm bản dịch sát nghĩa tiếng Việt.
- Đánh dấu **[suy luận]** cho những chỗ là nhận xét của người tổng hợp, không phải lời của nguồn.

---

## (a) Quy mô và hình thức gian lận trong giao dịch Bitcoin OTC/P2P

**Tóm tắt.**

- Có số liệu chính thức về quy mô gian lận liên quan tiền mã hóa nói chung (FBI IC3, Chainalysis).
- **Không tìm thấy số liệu quy mô gian lận riêng cho thị trường OTC/P2P** từ báo cáo sơ cấp nào.
- Bằng chứng thực nghiệm gần nhất cho P2P là nghiên cứu Paxful/LocalCoinSwap (a4).
- Các nghiên cứu học thuật về lừa đảo Bitcoin giai đoạn 2011–2014 (a5, a6) trùng với khoảng thời gian của bộ dữ liệu Bitcoin-OTC (2010–2016).

### a1. FBI IC3: thiệt hại liên quan tiền mã hóa tại Mỹ năm 2025

- **Nhận định.**
  - Năm 2025, IC3 nhận 181.565 khiếu nại có yếu tố tiền mã hóa, tăng 21% so với 2024.
  - Tổng thiệt hại là 11,366 tỷ USD, tăng 22%, trên tổng số 20,877 tỷ USD thiệt hại do tội phạm Internet.
  - Lừa đảo đầu tư tiền mã hóa là nguồn thiệt hại lớn nhất, với 7,2 tỷ USD.
- **Nguồn.** FBI Internet Crime Complaint Center, *2025 IC3 Annual Report*, 2026. https://www.ic3.gov/AnnualReport/Reports/2025_IC3Report.pdf
  - Tr. 52: "Complaints Involving Cryptocurrency – 2025".
  - Tr. 11 và tr. 53: mục "Cryptocurrency Investment Fraud".
  - Tr. 4 và tr. 6: con số tổng.
  - Tr. 61: định nghĩa descriptor.
- **Trích dẫn.**

  | Nguyên văn | Dịch sát nghĩa |
  |---|---|
  | "Accessibility Description: Describes Cryptocurrency highlights: 181,565 complaints (21% increase from 2024); $11.366 billion in losses (22% increase from 2024), $62,604 average loss, 18,589 complainants lost more than $100,000" (tr. 52) | "181.565 khiếu nại (tăng 21% so với 2024); thiệt hại 11,366 tỷ USD (tăng 22%), thiệt hại trung bình 62.604 USD, 18.589 người khiếu nại mất trên 100.000 USD" |
  | "Cryptocurrency investment fraud was the highest source of financial losses to Americans in 2025 with $7.2 billion reported in losses." (tr. 11) | "Lừa đảo đầu tư tiền mã hóa là nguồn thiệt hại tài chính lớn nhất của người Mỹ năm 2025, với 7,2 tỷ USD được báo cáo." |
  | "These scams are largely perpetrated by organized criminal enterprises based in Southeast Asia using victims of human trafficking as forced labor" (tr. 11) | "Các vụ lừa đảo này phần lớn do các tổ chức tội phạm có trụ sở ở Đông Nam Á thực hiện, dùng nạn nhân buôn người làm lao động cưỡng bức" |
  | "Cryptocurrency: Information reported contains some reference to virtual currency." (tr. 61) | "Tiền mã hóa: thông tin khiếu nại có nhắc tới tiền ảo." |

- **Độ tin cậy.** Đây là nguồn sơ cấp của cơ quan nhà nước.
  - Số liệu chỉ gồm khiếu nại do người dân **tại Mỹ** tự báo cáo, nên không phải ước lượng toàn cầu.
  - Một khiếu nại được tính là "cryptocurrency" khi chỉ cần *nhắc tới* tiền ảo.
  - Mục "Peer-to-Peer Transfer" trong báo cáo (tr. 10, tr. 34) là **phương thức thanh toán** kiểu Zelle, không phải giao dịch tiền mã hóa P2P. Không được gộp hai khái niệm này.

### a2. FBI IC3: mốc so sánh năm 2024

- **Nhận định.** Năm 2024, IC3 ghi nhận 149.686 khiếu nại liên quan tiền mã hóa với 9,3 tỷ USD thiệt hại, tăng 66%.
- **Nguồn.** FBI IC3, *2024 IC3 Annual Report*, 2025, tr. 35. https://www.ic3.gov/AnnualReport/Reports/2024_IC3Report.pdf
- **Trích dẫn.**

  | Nguyên văn | Dịch sát nghĩa |
  |---|---|
  | "Chart outlines cryptocurrency complaints in 2024: 149,686 complaints; $9.3 billion in losses; 66% increase in loss; largest age group to report is 60+." | "Biểu đồ khiếu nại tiền mã hóa năm 2024: 149.686 khiếu nại; 9,3 tỷ USD thiệt hại; thiệt hại tăng 66%; nhóm tuổi báo cáo nhiều nhất là trên 60." |

- **Độ tin cậy.** Như a1.

### a3. Chainalysis 2026 Crypto Crime Report: doanh thu lừa đảo toàn cầu năm 2025

- **Nhận định.** Chainalysis đưa ra ba con số khác nhau, cần trình bày đúng bản chất của từng con số:
  - **Số đo được:** các vụ lừa đảo tiền mã hóa nhận *ít nhất* 14 tỷ USD on-chain trong năm 2025.
  - **Số dự phóng:** con số 2025 *có thể vượt* 17 tỷ USD khi xác định thêm ví.
  - **Số đã hiệu chỉnh:** con số 2024 được nâng từ 9,9 lên 12 tỷ USD.
  - Lừa đảo mạo danh tăng hơn 1.400% so với năm trước.
- **Nguồn.** Chainalysis, "2026 Crypto Crime Report: Scams", 13/01/2026. https://www.chainalysis.com/blog/crypto-scams-2026/ (đoạn mở đầu).
- **Trích dẫn.**

  | Nguyên văn | Dịch sát nghĩa |
  |---|---|
  | "In 2025, cryptocurrency scams received at least $14 billion on-chain, a significant increase from the $9.9 billion we first reported in 2024, which reached $12 billion at our recalculation as of this writing" | "Năm 2025, các vụ lừa đảo tiền mã hóa nhận ít nhất 14 tỷ USD on-chain, tăng đáng kể so với 9,9 tỷ USD chúng tôi công bố lần đầu cho 2024, con số này đã lên 12 tỷ USD khi tính lại" |
  | "we project that the 2025 figure could exceed $17 billion as we identify more illicit wallet addresses in the coming months" | "chúng tôi dự phóng con số 2025 có thể vượt 17 tỷ USD khi xác định thêm địa chỉ ví bất hợp pháp trong những tháng tới" |
  | "While high-yield investment programs (HYIP) and pig butchering remain dominant categories by volume, we're seeing increasing convergence across scam types" | "Dù HYIP và lừa đảo 'giết heo' (pig butchering) vẫn chiếm ưu thế về khối lượng, các loại lừa đảo ngày càng hòa trộn vào nhau" |

- **Độ tin cậy.**
  - Chainalysis là công ty phân tích blockchain thương mại và có lợi ích kinh doanh.
  - Phương pháp của họ dựa trên các ví đã được gán nhãn, nên con số là cận dưới và thường được điều chỉnh tăng về sau.
  - Đây là nguồn được báo chí và giới học thuật trích dẫn rộng rãi.
  - Báo cáo cũng nhấn mạnh vai trò của các mạng lưới ở Đông Nam Á (Campuchia, Myanmar), gần với bối cảnh Việt Nam.

### a4. Nghiên cứu thực nghiệm trên chợ P2P tiền mã hóa (Paxful, LocalCoinSwap)

- **Nhận định.** Đây là bằng chứng thực nghiệm gần nhất với "gian lận trong giao dịch P2P". Bài báo cho thấy:
  - Chợ P2P đánh đổi quyền riêng tư lấy rủi ro lừa đảo cao hơn.
  - Hệ thống phản hồi (feedback) bị thao túng bằng thông đồng và tự động hóa.
  - 46% vendor Paxful bị khóa tài khoản.
  - Tài khoản bị khóa tương tác với nhau nhiều hơn mức bình thường. Điều này gợi ý nên dùng tín hiệu từ cấu trúc đồ thị.
- **Nguồn.** T. Tsuchiya, A. Cuevas, N. Christin, "Identifying Risky Vendors in Cryptocurrency P2P Marketplaces", *WWW '24*, 2024. DOI: 10.1145/3589334.3645475. PDF: https://www.andrew.cmu.edu/user/nicolasc/publications/Tsuchiya-WWW24.pdf
  - Abstract và §1.
  - §3.2 "User suspension".
  - §4.2 "User collusion and automation".
- **Trích dẫn.**

  | Nguyên văn | Dịch sát nghĩa |
  |---|---|
  | "Due to disintermediation, these marketplaces trade off increased privacy for higher risk (e.g., scams/fraud)." (Abstract) | "Do không có trung gian, các chợ này đánh đổi quyền riêng tư cao hơn lấy rủi ro cao hơn (ví dụ lừa đảo/gian lận)." |
  | "They may reverse payments, send fake/manipulated gift card receipts, harass users to release payments or block the release of cryptocurrencies." (§1) | "Họ có thể đảo ngược thanh toán, gửi biên nhận thẻ quà tặng giả hoặc bị chỉnh sửa, quấy rối người dùng để giải phóng thanh toán, hoặc chặn việc giải phóng tiền mã hóa." |
  | "these systems are susceptible to various types of attacks and manipulation such as self-promoting, whitewashing, retaliation, and bad-mouthing" (§1) | "các hệ thống này dễ bị tấn công và thao túng như tự đánh bóng, tẩy trắng danh tính, trả đũa và nói xấu (dìm hàng)" |
  | "as many as 46% of all Paxful vendors in our corpus who posted ads are suspended (24 562 users out of 53 224 until March 1, 2023)" (§3.2) | "có tới 46% vendor Paxful từng đăng quảng cáo trong dữ liệu bị khóa (24.562/53.224 người tính đến 01/3/2023)" |
  | "Only 16.26% of the feedback from non-suspended accounts is directed towards suspended accounts, whereas 24.82% of the feedback from suspended accounts goes to other suspended accounts." (§4.2) | "Chỉ 16,26% phản hồi từ tài khoản không bị khóa hướng tới tài khoản bị khóa, trong khi 24,82% phản hồi từ tài khoản bị khóa hướng tới các tài khoản bị khóa khác." |

- **Độ tin cậy.** Bài peer-review tại hội nghị hạng A* (The Web Conference).
  - Nhãn của nghiên cứu là "bị nền tảng khóa tài khoản", không phải "đã xác minh là lừa đảo".
  - Dữ liệu năm 2022–2023, trên các nền tảng P2P có ký quỹ (escrow), khác với Bitcoin-OTC (IRC, không có ký quỹ).
  - **[suy luận]** Các hình thức thao túng nêu trong bài (tự đánh bóng, nói xấu, Sybil) tương ứng với hai kiểu bất thường mà khóa luận quan tâm trên đồ thị đánh giá có dấu.

### a5. Lừa đảo Bitcoin giai đoạn 2011–2014 (trùng thời kỳ với dữ liệu Bitcoin-OTC)

- **Nhận định.**
  - Nghiên cứu thực nghiệm đầu tiên về lừa đảo dựa trên Bitcoin xác định 192 vụ thuộc 4 nhóm: Ponzi, đào coin, ví lừa đảo, sàn lừa đảo.
  - Có ít nhất 11 triệu USD được nạp vào các vụ này, từ 13.000 nạn nhân.
- **Nguồn.** M. Vasek, T. Moore, "There's No Free Lunch, Even Using Bitcoin: Tracking the Popularity and Profits of Virtual Currency Scams", *Financial Cryptography and Data Security (FC 2015)*, LNCS 8975, 2015. DOI: 10.1007/978-3-662-47854-7_4. PDF: https://tylermoore.utulsa.edu/fc15.pdf (Abstract, §1).
- **Trích dẫn.**

  | Nguyên văn | Dịch sát nghĩa |
  |---|---|
  | "we identify 192 scams and categorize them into four groups: Ponzi schemes, mining scams, scam wallets and fraudulent exchanges. [...] We find that at least $11 million has been contributed to the scams from 13 000 distinct victims." | "chúng tôi xác định 192 vụ lừa đảo và chia thành bốn nhóm: mô hình Ponzi, lừa đảo đào coin, ví lừa đảo và sàn giao dịch gian lận. [...] Ít nhất 11 triệu USD đã được nạp vào các vụ lừa đảo từ 13.000 nạn nhân khác nhau." |

- **Độ tin cậy.** Bài peer-review.
  - Con số là cận dưới, vì chỉ 21% số vụ tìm được địa chỉ ví.
  - Bài không riêng về OTC.

### a6. Rủi ro đối tác khi đổi Bitcoin sang tiền pháp định (2010–2013)

- **Nhận định.**
  - Trong 40 sàn Bitcoin lập trong ba năm, 18 sàn đã đóng cửa, thường xóa sạch số dư của khách hàng.
  - Bài ICDM 2016 (b3) trích nguồn này làm căn cứ cho "counterparty risk" (rủi ro đối tác).
- **Nguồn.** T. Moore, N. Christin, "Beware the Middleman: Empirical Analysis of Bitcoin-Exchange Risk", *Financial Cryptography (FC 2013)*, LNCS 7859, tr. 25–33, 2013. PDF: https://www.andrew.cmu.edu/user/nicolasc/publications/MC-FC13.pdf (Abstract).
- **Trích dẫn.**

  | Nguyên văn | Dịch sát nghĩa |
  |---|---|
  | "We examine the track record of 40 Bitcoin exchanges established over the past three years, and find that 18 have since closed, with customer account balances often wiped out. Fraudsters are sometimes to blame, but not always." | "Chúng tôi xem xét lịch sử của 40 sàn Bitcoin lập trong ba năm qua và thấy 18 sàn đã đóng cửa, số dư tài khoản khách hàng thường bị xóa sạch. Đôi khi lỗi do kẻ gian, nhưng không phải luôn luôn." |

- **Độ tin cậy.** Bài peer-review, nói về sàn tập trung chứ không phải OTC.
  - Dùng được để giải thích vì sao người dùng tìm tới giao dịch OTC trực tiếp và vì sao cần hệ thống uy tín.
  - **[suy luận]** Tác giả không nói trực tiếp ý này.

### a7. Không tìm thấy

- Không tìm thấy số liệu chính thức về **số vụ hoặc giá trị lừa đảo riêng trên #bitcoin-otc** hay trên thị trường OTC/P2P nói chung, từ IC3, Chainalysis hay nghiên cứu học thuật.
- Bộ dữ liệu SNAP chỉ cho biết khoảng 10% cạnh là âm (#5). Đây là tỷ lệ đánh giá tiêu cực, **không phải** tỷ lệ lừa đảo.

---

## (b) Cơ chế web-of-trust của #bitcoin-otc và nguồn gốc bộ dữ liệu SNAP

### b1. #bitcoin-otc là chợ OTC trên IRC, không có trung gian; web of trust ra đời để giảm rủi ro đối tác

- **Nguồn.**
  - Trang chủ https://bitcoin-otc.com/ (mục "#bitcoin-otc marketplace", "OTC web of trust", "Disclaimers").
  - Trang https://bitcoin-otc.com/trust.php (mục "What is the OTC web of trust?", "Disclaimers").
  - Truy cập 2026-11-28.
- **Trích dẫn.**

  | Nguyên văn | Dịch sát nghĩa |
  |---|---|
  | "#bitcoin-otc is an over-the-counter marketplace for trading with bitcoin. The marketplace is located in #bitcoin-otc channel on the Libera Chat IRC network." | "#bitcoin-otc là chợ giao dịch phi tập trung (OTC) để mua bán bitcoin, đặt tại kênh #bitcoin-otc trên mạng IRC Libera Chat." |
  | "Since all transactions carry with them counterparty risk (risk of non-payment by one of the parties), it becomes important to keep track of people's reputations and trade histories, so that you can decrease your probability of getting ripped off. And thus, the OTC web of trust is born." (trust.php) | "Vì mọi giao dịch đều có rủi ro đối tác (một bên không thanh toán), cần theo dõi uy tín và lịch sử giao dịch của mọi người để giảm khả năng bị lừa. Từ đó ra đời OTC web of trust." |
  | "All transactions that may occur are conducted directly between counterparties, without any participation or intermediation from #bitcoin-otc. As such, it is each individual's responsibility to conduct due diligence on their counterparties [...] to avoid falling prey to fraudulent users." (trang chủ) | "Mọi giao dịch diễn ra trực tiếp giữa hai bên, #bitcoin-otc không tham gia hay làm trung gian. Vì vậy mỗi người phải tự thẩm định đối tác [...] để không trở thành nạn nhân của người dùng gian lận." |
  | "The OTC web of trust is not foolproof. Do not rely on the ratings blindly - since the cost of entry into the web of trust is only one positive rating, it is not impossible for a scammer to infiltrate the system, and then create a bunch of bogus accounts who all inter-rate each other." (trang chủ, mục Disclaimers) | "Web of trust không hoàn hảo. Đừng tin điểm một cách mù quáng: vì chỉ cần một lượt đánh giá dương để vào hệ thống, kẻ lừa đảo hoàn toàn có thể thâm nhập rồi tạo hàng loạt tài khoản giả chấm điểm qua lại cho nhau." |

- **Độ tin cậy.** Đây là nguồn sơ cấp của chính nền tảng.
  - Trang hiện tại ghi kênh nằm trên "Libera Chat" (thời điểm truy cập 2026); trang không nói kênh ở mạng IRC nào trong giai đoạn 2010–2016. Chi tiết này không ảnh hưởng tới lập luận.
  - Đoạn Disclaimers là mô tả trực tiếp của nền tảng về kiểu tấn công Sybil và thổi phồng điểm. Đây là bằng chứng mạnh cho động cơ phát hiện cạnh bất thường.

### b2. Quy tắc chấm điểm: thang −10..+10, ý nghĩa từng mức, một cặp chỉ có một điểm, điểm tổng dễ bị thao túng

- **Nguồn.**
  - bitcoin-otc wiki, "OTC Rating System", bản lưu Wayback 24/11/2017 (wiki gốc hiện trả lỗi 522): https://web.archive.org/web/20171124163138/https://wiki.bitcoin-otc.com/wiki/OTC_Rating_System . Các mục "What is it?", "How do I use it?", "Rating guidelines", "Relying on the rating", "Notes about gettrust".
  - bitcoin-otc wiki, "Using bitcoin-otc", bản lưu Wayback 24/11/2017: https://web.archive.org/web/20171124132110/http://wiki.bitcoin-otc.com/wiki/Using_bitcoin-otc . Mục 5 "Safety: avoiding fraud", 5.1 "Risk of fraud".
- **Trích dẫn.**

  | Nguyên văn | Dịch sát nghĩa |
  |---|---|
  | "The OTC Rating System is an irc-based web of trust network. Users can give each other ratings between -10 and +10. The system calculates the cumulative trust received by a user from all ratings." (What is it?) | "Hệ thống đánh giá OTC là một mạng web of trust trên IRC. Người dùng chấm nhau từ −10 đến +10. Hệ thống tính tổng độ tin cậy mà một người nhận được từ mọi lượt chấm." |
  | "Note that only one rating can exist between two people. The 'rate' command updates the existing rating if it is used multiple times." (How do I use it?) | "Giữa hai người chỉ tồn tại một điểm. Lệnh 'rate' dùng nhiều lần sẽ cập nhật điểm cũ." |
  | "Only people who already have a trust rating in the system can rate other users." / "You must be registered and authenticated with the bot via the GPG authentication process to enter ratings." | "Chỉ người đã có điểm tin cậy trong hệ thống mới được chấm người khác." / "Phải đăng ký và xác thực với bot qua GPG mới được chấm điểm." |
  | Bảng "Rating guidelines": "10 — You trust this person as you trust yourself." · "5 — You've had a number of good transactions with this person." · "1 — One or two good transactions with this person" · "-1 — Person strikes you as a bit flaky. Unreasonable/unexpected delays in payment, etc." · "-10 — Person failed to hold up his end of the bargain, took payment and ran, fraudster." | 10: tin như tin chính mình · 5: đã có nhiều giao dịch tốt · 1: một hai giao dịch tốt · −1: hơi thiếu tin cậy, chậm thanh toán bất thường · −10: không làm đúng phần mình, nhận tiền rồi bỏ trốn, kẻ lừa đảo. |
  | "Do not rely on total rating blindly, since it can be artificially inflated by the user via the creation of fake identities, or deflated via spiteful spam ratings." (Relying on the rating) | "Đừng tin điểm tổng một cách mù quáng, vì nó có thể bị thổi phồng giả tạo bằng danh tính giả, hoặc bị dìm xuống bằng các lượt chấm ác ý hàng loạt." |
  | "If you don't know anyone, use 'nanotube' and anyone he trusts at 8 or more as a source." (How do I use it?) | "Nếu không quen ai, hãy lấy 'nanotube' và những người anh ta tin ở mức 8 trở lên làm nguồn tin cậy." |
  | "When you trade OTC you engage in a transaction with people you possibly know nothing about. You may send your BTC to the person, and never get anything back." (Using bitcoin-otc, §5.1) | "Khi giao dịch OTC, bạn giao dịch với người có thể bạn chẳng biết gì. Bạn có thể gửi BTC đi và không bao giờ nhận lại gì." |

- **Độ tin cậy.** Đây là tài liệu sơ cấp của nền tảng, xem qua bản lưu Wayback năm 2017, cùng thời kỳ với dữ liệu.
  - Mục "Notes about gettrust" mô tả cách tính "level 2 trust": lấy min của hai chặng, và chặng đầu âm thì cho 0. Đây là một cơ chế lan truyền tin cậy có dấu có từ trước, có thể nhắc khi giới thiệu bài toán.
- **[suy luận] Ba liên hệ với các ticket trước:**
  1. Quy tắc "một cặp chỉ có một điểm, chấm lại thì ghi đè" giải thích phát hiện ở #5 rằng không có cặp nào được chấm hai lần. Hệ quả: timestamp trong SNAP **có thể** là thời điểm cập nhật cuối, không phải lần chấm đầu. Wiki và SNAP đều không xác nhận điều này, nên chỉ ghi như một lưu ý về dữ liệu.
  2. Wiki chỉ định `nanotube` làm gốc tin cậy, với ngưỡng ≥ 8. REV2 (b4) dùng ngưỡng ≥ 0,5 sau khi chuẩn hóa, tức ≥ 5. Điều này ủng hộ về mặt khái niệm cách dựng nhãn bắt đầu từ người sáng lập, nhưng **không xác minh** giả định "người sáng lập = node 1" của #11, vì SNAP đã ẩn danh hóa ID.
  3. Hướng dẫn chấm −10 cho "fraudster" là căn cứ để diễn giải cạnh −10. #5 cho biết −10 chiếm 68% số cạnh âm.

### b3. Kumar et al., ICDM 2016: lý do bộ dữ liệu tồn tại (ẩn danh dẫn tới rủi ro đối tác)

- **Nguồn.** S. Kumar, F. Spezzano, V.S. Subrahmanian, C. Faloutsos, "Edge Weight Prediction in Weighted Signed Networks", *IEEE ICDM 2016*. DOI: 10.1109/ICDM.2016.0033. PDF: https://cs.stanford.edu/~srijan/pubs/wsn-icdm16.pdf
  - §V "Real-world data: weighted signed networks", đoạn "Bitcoin networks", và Table III.
  - §IV-A, đoạn định nghĩa fairness.
- **Trích dẫn.**

  | Nguyên văn | Dịch sát nghĩa |
  |---|---|
  | "Bitcoin is a cryptocurrency that is used to trade anonymously over the web. Due to anonymity, there is counterparty risk [32], which has lead to the emergence of several exchanges where Bitcoin users rate the level of trust they have in other users. We created two datasets from two such exchanges - Bitcoin-OTC (OTC for short) and Bitcoin-Alpha" (§V) | "Bitcoin là tiền mã hóa dùng để giao dịch ẩn danh trên web. Do ẩn danh nên có rủi ro đối tác [32], dẫn tới sự ra đời của một số sàn nơi người dùng Bitcoin chấm mức độ tin cậy của họ với người khác. Chúng tôi tạo hai bộ dữ liệu từ hai sàn như vậy: Bitcoin-OTC và Bitcoin-Alpha" |
  | "According to OTC's guidelines, a rating of -10 should be given to fraudsters while at the other end of the spectrum, +10 means "you trust the person as you trust yourself"." (§V) | "Theo hướng dẫn của OTC, điểm −10 dành cho kẻ lừa đảo, còn +10 nghĩa là 'bạn tin người này như tin chính mình'." |
  | "As an example, in real world networks such as the Bitcoin networks, scammers create multiple accounts to increase their own ratings and to reduce the ratings of benign users." (§IV-A) | "Ví dụ, trong các mạng thực như mạng Bitcoin, kẻ lừa đảo tạo nhiều tài khoản để nâng điểm của chính mình và hạ điểm của người dùng lành tính." |
  | Table III: "Bitcoin-OTC 5,881 35,592 89%" | Bitcoin-OTC: 5.881 nút, 35.592 cạnh, 89% cạnh dương |

- **Độ tin cậy.** Bài peer-review và là trích dẫn gốc bắt buộc khi dùng bộ dữ liệu.
  - [32] là Moore & Christin 2013 (a6).
  - Bài cũng nói dự đoán trọng số cạnh hữu ích cho "anomaly detection [2], [3]" (§I). Đây là chỗ nối trực tiếp với bài toán của khóa luận.

### b4. Kumar et al., WSDM 2018 (REV2): người dùng gian lận cho điểm giả vì lợi ích tiền bạc; cách dựng nhãn trên OTC; đặc trưng hành vi

- **Nguồn.** S. Kumar, B. Hooi, D. Makhija, M. Kumar, C. Faloutsos, V.S. Subrahmanian, "REV2: Fraudulent User Prediction in Rating Platforms", *WSDM 2018*. DOI: 10.1145/3159652.3159729. PDF: https://cs.stanford.edu/~srijan/pubs/rev2-wsdm18.pdf
  - Abstract, §1, §5.1 "Datasets: Rating Networks", §5.8 "Discoveries", Fig. 6(d–e).
  - Định nghĩa nhãn ở §5.1 đã phân tích ở #11. Ở đây chỉ lấy phần động cơ.
- **Trích dẫn.**

  | Nguyên văn | Dịch sát nghĩa |
  |---|---|
  | "Rating platforms enable large-scale collection of user opinion about items (e.g., products or other users). However, fraudulent users give fake ratings for excessive monetary gains." (Abstract) | "Nền tảng đánh giá cho phép thu thập ý kiến người dùng về đối tượng (sản phẩm hoặc người dùng khác) ở quy mô lớn. Tuy nhiên, người dùng gian lận cho điểm giả để thu lợi tiền bạc quá mức." |
  | "Users trading anonymous cryptocurrency, such as Bitcoin, rate others on their trustworthiness. Because users frequently look at these ratings and reviews before buying a product online or choosing whom to trade Bitcoins with, there is a huge monetary incentive for fraudulent users to give fake ratings" (§1) | "Người dùng giao dịch tiền mã hóa ẩn danh như Bitcoin chấm điểm độ tin cậy của nhau. Vì người dùng thường xem các điểm và đánh giá này trước khi mua hàng hay chọn người để giao dịch Bitcoin, kẻ gian có động cơ tiền bạc rất lớn để cho điểm giả" |
  | "This task is challenging due to the lack of training labels, disbalance in the percentage of fraudulent and non-fraudulent users, and camouflage by fraudulent users" (§1) | "Bài toán khó vì thiếu nhãn huấn luyện, mất cân bằng tỷ lệ gian lận và không gian lận, và kẻ gian ngụy trang" |
  | "fraudulent users in OTC network: (d) give ratings in quick succession of less than a few minutes, and (e) exhibit bimodal rating pattern: they either give all -1.0 ratings (possibly, bad-mouthing a competitor) or all +1.0 ratings (possibly, over-selling their products/friends)." (§5.8) | "người dùng gian lận trên mạng OTC: (d) chấm liên tiếp cách nhau dưới vài phút, và (e) có kiểu chấm hai cực: hoặc toàn −1,0 (có thể là dìm đối thủ), hoặc toàn +1,0 (có thể là thổi phồng sản phẩm/bạn bè)." |

- **Độ tin cậy.** Bài peer-review.
  - Câu ở §1 là lập luận động cơ, không phải số liệu đo được.
  - Kết luận ở §5.8 dựa trên người dùng do REV2 *gắn cờ*, không phải nhãn đã xác minh.
  - **[suy luận]** Hai kiểu "dìm hàng" và "thổi phồng" ở §5.8 khớp trực tiếp với định nghĩa bất thường cạnh trong khóa luận, và là căn cứ để giải thích vì sao dấu và trọng số cạnh mang thông tin.

### b5. Trang SNAP: mô tả gốc của bộ dữ liệu

- **Nguồn.** Stanford SNAP, "Bitcoin OTC trust weighted signed network": https://snap.stanford.edu/data/soc-sign-bitcoin-otc.html (mục "Dataset information").
- **Trích dẫn.**

  | Nguyên văn | Dịch sát nghĩa |
  |---|---|
  | "This is who-trusts-whom network of people who trade using Bitcoin on a platform called Bitcoin OTC. Since Bitcoin users are anonymous, there is a need to maintain a record of users' reputation to prevent transactions with fraudulent and risky users. Members of Bitcoin OTC rate other members in a scale of -10 (total distrust) to +10 (total trust) in steps of 1. This is the first explicit weighted signed directed network available for research." | "Đây là mạng 'ai tin ai' giữa những người giao dịch Bitcoin trên nền tảng Bitcoin OTC. Vì người dùng Bitcoin ẩn danh, cần lưu lại uy tín của người dùng để tránh giao dịch với người gian lận hoặc rủi ro. Thành viên chấm nhau theo thang −10 (hoàn toàn không tin) tới +10 (hoàn toàn tin), bước 1. Đây là mạng có hướng, có dấu, có trọng số tường minh đầu tiên dành cho nghiên cứu." |

- **Độ tin cậy.** Đây là trang chính thức của kho dữ liệu. Trang yêu cầu trích dẫn cả ICDM 2016 (b3) và WSDM 2018 (b4).

---

## (c) Nghiên cứu trong nước và quy mô gian lận tiền mã hóa tại Việt Nam

### c1. Nghiên cứu của tác giả hoặc đơn vị Việt Nam: đã tìm thấy

**c1.1. Khảo sát GNN cho phát hiện bất thường trong FinTech (ĐH Mở TP.HCM)**

- **Nguồn.** Vinh Truong Hoang, Nghia Dinh, Viet-Tuan Le, Kiet Tran-Trung, Bay Nguyen Van (AI Lab, Khoa CNTT, Trường ĐH Mở TP.HCM), Kittikhun Meethongjan (Suan Sunandha Rajabhat Univ., Thái Lan), "Detecting Anomalies in FinTech: A Graph Neural Network and Feature Selection Perspective", *Computers, Materials & Continua* 86(1), 2026, tr. 1–40. DOI: 10.32604/cmc.2025.068733. https://www.techscience.com/cmc/v86n1/64431/html (Abstract).
- **Trích dẫn.**

  | Nguyên văn | Dịch sát nghĩa |
  |---|---|
  | "Traditional anomaly detection techniques often fail to capture the relational and dynamic characteristics of financial data. Graph Neural Networks (GNNs), capable of modeling intricate interdependencies among entities, have emerged as a powerful framework for detecting subtle and sophisticated anomalies." | "Các kỹ thuật phát hiện bất thường truyền thống thường không nắm được tính quan hệ và tính động của dữ liệu tài chính. GNN, với khả năng mô hình hóa các phụ thuộc phức tạp giữa các thực thể, đã trở thành khung mạnh để phát hiện các bất thường tinh vi." |

- **Độ tin cậy.**
  - Đây là bài khảo sát, không phải đề xuất mô hình.
  - Bài đăng tạp chí có thẩm định (chưa kiểm tra chỉ mục và chất lượng thẩm định của tạp chí). Nên trích như một tổng quan, không dùng làm căn cứ kỹ thuật.
  - Bài có bàn về đồ thị động. Khi dò từ khóa trên bản HTML, **không thấy** nhắc tới đồ thị có dấu hay Bitcoin-OTC.

**c1.2. GNN trên dữ liệu Elliptic (Bitcoin) cho chống rửa tiền (RMIT Hà Nội, ĐHQGHN)**

- **Nguồn.** Dang Sy Duy (RMIT University, Hanoi), Nguyen Duy Chien (Vietnam National University, Hanoi), Kapil Dev, Jeff Nijsse (RMIT University, Hanoi), "Normalisation and Initialisation Strategies for Graph Neural Networks in Blockchain Anomaly Detection", arXiv:2602.23599, 27/02/2026. https://arxiv.org/abs/2602.23599 (Abstract, trang 1).
- **Trích dẫn.**

  | Nguyên văn | Dịch sát nghĩa |
  |---|---|
  | "We present a systematic ablation of initialisation and normalisation strategies across three GNN architectures (GCN, GAT, and GraphSAGE) on the Elliptic Bitcoin dataset." | "Chúng tôi thực hiện ablation có hệ thống các chiến lược khởi tạo và chuẩn hóa trên ba kiến trúc GNN (GCN, GAT, GraphSAGE) với bộ dữ liệu Elliptic Bitcoin." |

- **Độ tin cậy.** Đây là preprint chưa qua peer-review.
  - Bài toán là phân loại nút có giám sát (giao dịch hợp pháp hay bất hợp pháp), khác với phát hiện cạnh bất thường không giám sát trên mạng tin cậy có dấu.
  - Dùng được để nói rằng trong nước đã bắt đầu có nhóm làm GNN cho dữ liệu blockchain.

### c2. Nghiên cứu trong nước: không tìm thấy

- **Không tìm thấy** công trình nào của tác giả hoặc đơn vị Việt Nam về:
  - phát hiện bất thường (cạnh hoặc nút) trên **đồ thị có dấu** hoặc **đồ thị động**;
  - bộ dữ liệu **Bitcoin-OTC/Bitcoin-Alpha**;
  - phát hiện người chấm điểm gian lận trên mạng đánh giá tin cậy.
- **Không tìm thấy** bài trên tạp chí khoa học trong nước về phát hiện gian lận giao dịch tiền mã hóa bằng học máy. Các kết quả tìm được trên vjol.info.vn chỉ gồm:
  - bài pháp lý hoặc chính sách về tiền ảo;
  - bài phát hiện gian lận thẻ tín dụng (TNU Journal of Science and Technology), không phải dữ liệu blockchain.
- **Đã tìm.** WebSearch (tiếng Việt và tiếng Anh), với các từ khóa:
  - "phát hiện bất thường đồ thị", "mạng nơ-ron đồ thị", "phát hiện gian lận giao dịch tiền mã hóa";
  - "Vietnam National University / Hanoi / Ho Chi Minh City" kết hợp "graph neural network", "Elliptic", "Ethereum phishing/Ponzi";
  - "SoICT/RIVF/KSE graph anomaly detection";
  - "luận văn thạc sĩ phát hiện bất thường đồ thị GNN".
  - Có giới hạn domain vjol.info.vn, vnu.edu.vn, jst.iuh.edu.vn, vjs.ac.vn. Riêng bộ lọc domain vjs.ac.vn không có tác dụng, vì kết quả trả về toàn arXiv.
- **Chưa bao phủ.**
  - Google Scholar, Scopus, IEEE Xplore và ACM DL lọc theo affiliation.
  - Kho luận văn số của ĐHBK Hà Nội, ĐHCN–ĐHQGHN, ĐHKHTN và ĐH CNTT–ĐHQG TP.HCM.
  - Kỷ yếu FAIR, SoICT, KSE, RIVF tra trực tiếp.
  - Nên tra thủ công các nguồn này trước khi viết câu "trong nước chưa có nghiên cứu…". Hiện chỉ đủ căn cứ để viết "chưa tìm thấy".
- **Chưa mở được.** Bài pháp lý "Identifying fraudulent behavior and some proposals for protecting cryptocurrency investors" trên vjol.info.vn (chưa xác định tên tạp chí): https://vjol.info.vn/phaply/en/article/view/162583 . Trang bị lặp chuyển hướng nên chưa xác minh nội dung. **Không dùng** cho đến khi đọc được.

### c3. Quy mô và hình thức gian lận tiền mã hóa tại Việt Nam

**Lưu ý chung.** Các con số dưới đây đo **những đại lượng khác nhau**, không được gộp:

| Mục | Đại lượng | Con số | Nguồn |
|---|---|---|---|
| c3.1 | Mức độ chấp nhận | Hạng 4 (chỉ số tổng hợp từ hoạt động on-chain) | Chainalysis |
| c3.2 | Số người sở hữu | 17 triệu, hạng 7 | Chủ tịch VBA |
| c3.3 | Số người sở hữu | hơn 20 triệu, hạng 5 | "theo thống kê", không rõ nguồn |
| c3.2 | Thiệt hại lừa đảo qua mạng nói chung | 1.660 tỷ đồng | A05 |

**c3.1. Việt Nam thuộc nhóm chấp nhận tiền mã hóa cao nhất thế giới (hạng 4 năm 2025)**

- **Nguồn.** Chainalysis, "2025 Global Crypto Adoption Index", 02/09/2025. https://www.chainalysis.com/blog/2025-global-crypto-adoption-index/
- **Trích dẫn.**

  | Nguyên văn | Dịch sát nghĩa |
  |---|---|
  | "India ranks first in the 2025 Chainalysis Global Crypto Adoption Index, followed closely by the United States, Pakistan, Vietnam, and Brazil." | "Ấn Độ đứng đầu Chỉ số chấp nhận tiền mã hóa toàn cầu 2025 của Chainalysis, theo sát là Mỹ, Pakistan, Việt Nam và Brazil." |
  | "In 2025, APAC furthered its status as the global hub of grassroots crypto activity, led by India, Pakistan, and Vietnam" | "Năm 2025, châu Á – Thái Bình Dương củng cố vị thế trung tâm toàn cầu của hoạt động tiền mã hóa từ cơ sở, dẫn đầu là Ấn Độ, Pakistan và Việt Nam" |

- **Độ tin cậy.** Nguồn thương mại.
  - Đây là chỉ số tổng hợp về mức độ sử dụng, **không phải** số người sở hữu hay số vụ lừa đảo.

**c3.2. Bộ Công an (A05): gần 1.500 vụ lừa đảo qua mạng, thiệt hại hơn 1.660 tỷ đồng từ đầu năm 2025; hơn 1.500 kênh/hội nhóm liên quan tiền ảo**

- **Nguồn.** VnEconomy, "17 triệu người Việt sở hữu tài sản tiền mã hóa", 09/08/2025. https://vneconomy.vn/17-trieu-nguoi-viet-so-huu-tai-san-tien-ma-hoa.htm
  - Bài thuật lại số liệu của Cục A05 và phát biểu của Chủ tịch Hiệp hội Blockchain và Tài sản số Việt Nam (VBA) tại buổi tập huấn ngày 8/8/2025.
- **Trích dẫn (nguyên văn tiếng Việt).**
  - "Tính đến năm 2024, số người Việt sở hữu tài sản tiền mã hóa là 17 triệu người, đứng thứ 7 trên toàn cầu…"
  - "Số liệu của Cục An ninh mạng và phòng, chống tội phạm sử dụng công nghệ cao (A05, Bộ Công an), cho biết chỉ tính riêng từ đầu năm 2025 đến nay, đã phát hiện gần 1.500 vụ việc lừa đảo qua mạng, gây thiệt hại hơn 1.660 tỉ đồng. Trong đó, riêng các hoạt động liên quan giao dịch tiền ảo, tiền mã hóa lên tới hơn 1.500 kênh, hội nhóm."
- **Độ tin cậy.** Đây là nguồn thứ cấp (báo chí) thuật lại số liệu của cơ quan công an.
  - Con số 1.660 tỷ đồng là cho **mọi loại lừa đảo qua mạng**. Chỉ con số "hơn 1.500 kênh, hội nhóm" là riêng cho tiền mã hóa.
  - Con số 17 triệu là phát biểu của VBA, không nêu phương pháp.
  - Không tìm thấy báo cáo gốc của Bộ Công an tách riêng thiệt hại do lừa đảo tiền mã hóa.

**c3.3. Giao dịch P2P "chợ đen" tại Việt Nam: uy tín chỉ dựa vào đánh giá của thành viên kỳ cựu**

Đây là tương đồng trực tiếp với Bitcoin-OTC.

- **Nguồn.** Thượng tá Nguyễn Thành Chung (Cục A05, Bộ Công an), "Phòng, chống tội phạm liên quan tài sản mã hóa", *Báo Nhân Dân điện tử*, mục Góc nhìn, 03/10/2025. https://nhandan.vn/phong-chong-toi-pham-lien-quan-tai-san-ma-hoa-post912624.html
- **Trích dẫn (nguyên văn tiếng Việt).**
  - "Bên cạnh giao dịch trên sàn quốc tế, hoạt động trao đổi, mua bán tài sản mã hóa giữa cá nhân với cá nhân trên không gian mạng còn được tổ chức theo dạng "chợ đen". Trên các nền tảng mạng xã hội hiện nay tràn lan các hội nhóm mua bán, trao đổi tài sản mã hóa [...] Thành viên của các nhóm này đăng thông tin về nhu cầu mua, bán, hoặc trao đổi tài sản mã hóa. Nếu "khớp lệnh", các thành viên sẽ trực tiếp giao dịch với nhau. Uy tín của người mua, người bán tài sản mã hóa trên mạng chỉ dựa vào đánh giá của các thành viên kỳ cựu trong nhóm."
  - "Các giao dịch mua bán tài sản mã hóa giữa cá nhân với cá nhân trên thị trường "chợ đen" có giá trị từ vài nghìn cho đến vài trăm nghìn USDT, cá biệt có những trường hợp giá trị giao dịch mua bán tài sản mã hóa lên tới hàng triệu USDT/ngày."
  - "nếu cá nhân sử dụng tiền pháp định để mua tài sản mã hóa theo hình thức P2P qua các sàn giao dịch quốc tế thì rất khó để xác minh và kiểm soát."
  - "theo thống kê, Việt Nam hiện có hơn 20 triệu người đang sở hữu tài sản mã hóa, đứng thứ 5 toàn cầu" (bài không nêu nguồn thống kê).
- **Độ tin cậy.**
  - Tác giả là cán bộ cơ quan chuyên trách, bài đăng trên báo Đảng. Đây là quan điểm nghiệp vụ, không phải báo cáo thống kê.
  - Các con số giá trị giao dịch là ước lượng định tính, không kèm phương pháp.
  - **[suy luận]** Cơ chế "khớp lệnh trong nhóm, giao dịch trực tiếp, uy tín dựa trên đánh giá của thành viên kỳ cựu" về bản chất giống #bitcoin-otc (b1, b2): chợ trên kênh chat, không trung gian, uy tín do cộng đồng chấm. Đây là cầu nối mạnh nhất giữa bộ dữ liệu và bối cảnh Việt Nam.

**c3.4. Ví dụ vụ việc: sàn giả "Toptrade1" (Hưng Yên, 2025), 100 triệu USDT giao dịch**

- **Nguồn.** VnEconomy (bản tiếng Anh), "Massive $99 mln crypto fraud ring busted", 03/08/2025. https://en.vneconomy.vn/massive-99-mln-crypto-fraud-ring-busted.htm . Bài dẫn cổng thông tin Công an tỉnh Hưng Yên.
- **Trích dẫn.**

  | Nguyên văn | Dịch sát nghĩa |
  |---|---|
  | "At the time of the arrests, the total amount of virtual currency transacted on the platform had reached 100 million USDT, equivalent to over VND2.6 trillion (over $99.25 million)." | "Tại thời điểm bắt giữ, tổng lượng tiền ảo giao dịch trên nền tảng đã đạt 100 triệu USDT, tương đương hơn 2.600 tỷ đồng (hơn 99,25 triệu USD)." |
  | "these were entirely virtual notifications, and account holders could not withdraw any actual cash." | "đó hoàn toàn là thông báo ảo, chủ tài khoản không rút được đồng tiền thật nào." |

- **Độ tin cậy.** Nguồn báo chí thuật lại công an tỉnh.
  - 100 triệu USDT là **khối lượng giao dịch** trên sàn, không phải số tiền bị chiếm đoạt.
  - Đây là lừa đảo sàn giả, không phải OTC. Chỉ dùng làm ví dụ minh họa.

**c3.5. Khung pháp lý: thí điểm thị trường tài sản mã hóa (Nghị quyết 05/2025/NQ-CP)**

- **Nguồn.** VnEconomy, "Chính thức thí điểm thị trường tài sản mã hóa tại Việt Nam từ 9/9/2025", 09/09/2025. https://vneconomy.vn/chinh-thuc-thi-diem-thi-truong-tai-san-ma-hoa-tai-viet-nam-tu-992025.htm
- **Nội dung đã kiểm qua WebFetch.**
  - Nghị quyết 05/2025/NQ-CP ban hành ngày 9/9/2025.
  - Thời gian thí điểm là 5 năm.
  - Nguyên tắc có nêu "bảo vệ quyền và lợi ích hợp pháp của các tổ chức, cá nhân tham gia thị trường tài sản mã hóa".
- **Độ tin cậy.**
  - Đây là nguồn **thứ cấp**. Trích dẫn trên là kết quả tóm lược của WebFetch, chưa đối chiếu với văn bản gốc.
  - Thư Viện Pháp Luật trả 403. Nếu đưa vào khóa luận, cần trích từ Công báo hoặc vanban.chinhphu.vn.

### c4. Vụ việc đã loại

- Vụ "Mr Pips" (2024–2025) là lừa đảo **forex**, không phải tiền mã hóa, nên không dùng.
- Các bài báo nêu con số "12.000 tỷ đồng (2024)" và "trên 6.000 tỷ đồng (2025)" thiệt hại do lừa đảo trực tuyến chỉ thấy trong kết quả tìm kiếm, **chưa mở**, nên không dùng.

---

## (d) Ứng dụng thực tế của phát hiện bất thường trên mạng đánh giá và mạng tin cậy

### d1. REV2 được triển khai tại Flipkart: 127/150 người bị gắn cờ được xác nhận gian lận

- **Nguồn.** Kumar et al., WSDM 2018 (b4): Abstract, §1, §5.3 và Fig. 1(b).
- **Trích dẫn.**

  | Nguyên văn | Dịch sát nghĩa |
  |---|---|
  | "We reported the 150 most unfair users in the Flipkart network to their review fraud investigators, and 127 users were identified as being fraudulent (84.6% accuracy). The Rev2 algorithm is being deployed at Flipkart." | "Chúng tôi báo 150 người dùng kém công bằng nhất trên Flipkart cho đội điều tra gian lận đánh giá, và 127 người được xác định là gian lận (độ chính xác 84,6%). Thuật toán Rev2 đang được triển khai tại Flipkart." |

- **Độ tin cậy.** Bài peer-review, có đồng tác giả từ Flipkart.
  - "Đang được triển khai" là tuyên bố của tác giả năm 2018. Chưa có nguồn độc lập xác nhận vận hành lâu dài.
  - Đây là ứng dụng thực tế gần nhất với khóa luận: cùng nhóm tác giả, cùng bộ dữ liệu, cùng ý tưởng fairness/goodness.

### d2. GEM: GNN dị thể phát hiện tài khoản độc hại tại Alipay

- **Nguồn.** Z. Liu, C. Chen, X. Yang, J. Zhou, X. Li, L. Song, "Heterogeneous Graph Neural Networks for Malicious Account Detection", *CIKM 2018*, tr. 2077–2085. DOI: 10.1145/3269206.3272010. arXiv: https://arxiv.org/abs/2002.12307 (§1 Contributions, §4.3.5 "Online Results").
- **Trích dẫn.**

  | Nguyên văn | Dịch sát nghĩa |
  |---|---|
  | "Our approach is deployed at Alipay, one of the largest third-party mobile and online cashless payment platform serving more than 4 hundreds of million users. The approach can detect tens of thousands malicious accounts daily." | "Phương pháp được triển khai tại Alipay, một trong những nền tảng thanh toán không tiền mặt bên thứ ba lớn nhất, phục vụ hơn 400 triệu người dùng. Phương pháp phát hiện hàng chục nghìn tài khoản độc hại mỗi ngày." |
  | "everyday we treat top ten thousand scored newly registered accounts identified by our approach as accounts at risk. Under this strategy, the precision evaluated by the security department from Alipay is over 98% after a long time observation." (§4.3.5) | "mỗi ngày chúng tôi coi 10.000 tài khoản mới đăng ký có điểm cao nhất là tài khoản rủi ro. Với chiến lược này, độ chính xác do bộ phận an ninh Alipay đánh giá là trên 98% sau thời gian dài quan sát." |

- **Độ tin cậy.** Bài peer-review của tác giả trong công ty, số liệu vận hành do công ty tự báo cáo.
  - Đồ thị trong bài là tài khoản–thiết bị, không phải mạng đánh giá. Bài minh họa việc GNN trên đồ thị đã được dùng trong thực tế để chống gian lận.

### d3. Amazon: chặn hơn 200 triệu đánh giá giả năm 2022, có dùng GNN để tìm nhóm tác nhân xấu

- **Nguồn.** Amazon, "How Amazon is using AI to ensure authentic customer reviews", aboutamazon.com, đăng khoảng 17/11/2023 theo metadata `datePublished`. https://www.aboutamazon.com/news/policy-news-views/how-ai-spots-fake-reviews-amazon
- **Trích dẫn.**

  | Nguyên văn | Dịch sát nghĩa |
  |---|---|
  | "In fact, in 2022, Amazon observed and proactively blocked more than 200 million suspected fake reviews in its stores worldwide." | "Năm 2022, Amazon đã phát hiện và chủ động chặn hơn 200 triệu đánh giá nghi giả trên các cửa hàng toàn cầu." |
  | "Amazon also uses deep graph neural networks to analyze and understand complex relationships and behavior patterns to help detect and remove groups of bad actors or point towards suspicious activity for investigation." | "Amazon cũng dùng mạng nơ-ron đồ thị sâu để phân tích quan hệ và mẫu hành vi phức tạp, giúp phát hiện và loại bỏ các nhóm tác nhân xấu, hoặc chỉ ra hoạt động đáng ngờ để điều tra." |

- **Độ tin cậy.** Đây là thông cáo của doanh nghiệp, không kiểm chứng độc lập và không công bố phương pháp chi tiết.
  - Nguồn này dùng được để nói rằng GNN đã được ứng dụng công nghiệp cho bài toán đánh giá giả.

### d4. Yelp: phần mềm tự động "không đề xuất" 18% đánh giá; đóng hơn 1.300 tài khoản thuộc vòng trao đổi đánh giá (2024)

- **Nguồn.** Yelp, "Yelp Releases 2024 Trust & Safety Report", thông cáo báo chí ngày 05/02/2025. https://www.yelp-press.com/press-releases/press-release-details/2025/Yelp-Releases-2024-Trust--Safety-Report/default.aspx
- **Trích dẫn.**

  | Nguyên văn | Dịch sát nghĩa |
  |---|---|
  | "about 78% were recommended by Yelp's automated recommendation software [...] Additionally, 18% were not recommended, 3% were removed by Yelp's User Operations team" | "khoảng 78% được phần mềm đề xuất tự động của Yelp đề xuất [...] 18% không được đề xuất, 3% bị đội User Operations gỡ bỏ" |
  | "Over 1,300 user accounts were closed for being associated with review exchange rings or other suspicious review activity." | "Hơn 1.300 tài khoản bị đóng vì liên quan tới vòng trao đổi đánh giá hoặc hoạt động đánh giá đáng ngờ khác." |

- **Độ tin cậy.** Đây là báo cáo tự công bố của doanh nghiệp, số liệu tại Mỹ.
  - "Không đề xuất" không đồng nghĩa với "giả mạo".

### d5. FRAUDAR: phát hiện nhóm mua follower trên đồ thị Twitter 1,47 tỷ cạnh

- **Nguồn.** B. Hooi, H.A. Song, A. Beutel, N. Shah, K. Shin, C. Faloutsos, "FRAUDAR: Bounding Graph Fraud in the Face of Camouflage", *KDD 2016*. DOI: 10.1145/2939672.2939747. PDF: https://www.cs.cmu.edu/~christos/PUBLICATIONS/kdd16-fraudar.pdf (Abstract, §1).
- **Trích dẫn.**

  | Nguyên văn | Dịch sát nghĩa |
  |---|---|
  | "in real-world experiments with a Twitter follower-followee graph of 1.47 billion edges, FRAUDAR successfully detected a subgraph of more than 4000 detected accounts, of which a majority had tweets showing that they used follower-buying services." | "trong thí nghiệm thực tế trên đồ thị follower–followee của Twitter với 1,47 tỷ cạnh, FRAUDAR phát hiện một đồ thị con gồm hơn 4.000 tài khoản, phần lớn có tweet cho thấy họ dùng dịch vụ mua follower." |
  | "Fraudsters can evade these methods using camouflage, by adding reviews or follows with honest targets so that they look "normal"." | "Kẻ gian né các phương pháp này bằng ngụy trang, tức thêm đánh giá hoặc lượt theo dõi tới các đối tượng trung thực để trông 'bình thường'." |

- **Độ tin cậy.** Bài peer-review.
  - Đây là ứng dụng trên dữ liệu thật, không phải triển khai sản phẩm.
  - Khái niệm "camouflage" (ngụy trang) cũng được REV2 nhắc tới, và là lý do bất thường khó phát hiện chỉ bằng điểm tổng.

### d6. Mô hình học máy trên chợ P2P tiền mã hóa dự báo tài khoản rủi ro tốt hơn điểm uy tín

- **Nguồn.** Tsuchiya et al., WWW '24 (a4): Abstract, §7.
- **Trích dẫn.**

  | Nguyên văn | Dịch sát nghĩa |
  |---|---|
  | "we perform a monthlong online evaluation to show that our models are significantly more successful than mere feedback-based reputation schemes at predicting which users will be suspended in the future." | "chúng tôi đánh giá trực tuyến trong một tháng và cho thấy mô hình thành công hơn đáng kể so với cơ chế uy tín dựa trên phản hồi thuần túy trong việc dự đoán người dùng nào sẽ bị khóa." |

- **Độ tin cậy.** Như a4.
  - Đây là bằng chứng rằng nếu chỉ nhìn điểm uy tín thì không đủ để phát hiện tài khoản rủi ro trên chợ P2P tiền mã hóa.

---

## Gợi ý dùng cho phần mở đầu

### "Lý do chọn đề tài và bối cảnh bài toán"

1. **Mở bằng quy mô.**
   - IC3 2025 (a1): 11,366 tỷ USD thiệt hại liên quan tiền mã hóa tại Mỹ, tr. 52.
   - Chainalysis (a3): ít nhất 14 tỷ USD on-chain toàn cầu năm 2025, dự phóng trên 17 tỷ USD.
   - Phải nói rõ hai giới hạn: IC3 chỉ tính tại Mỹ, còn 17 tỷ USD là con số dự phóng.
2. **Từ ẩn danh tới rủi ro đối tác tới hệ thống uy tín.**
   - Dùng câu ở §V của ICDM 2016 (b3) và câu mô tả của SNAP (b5): đây là lý do bộ dữ liệu tồn tại.
   - Kèm câu "the OTC web of trust is born" (b1) để cho thấy chính nền tảng nhận diện rủi ro này.
3. **Hệ thống uy tín dễ bị thao túng.** Đây là lý do cần phát hiện bất thường thay vì chỉ đọc điểm.
   - Disclaimer "cost of entry [...] only one positive rating [...] bogus accounts who all inter-rate each other" (b1).
   - Wiki "artificially inflated [...] or deflated via spiteful spam ratings" (b2).
   - REV2 §5.8 "bad-mouthing / over-selling" (b4).
   - Tsuchiya §1 và §4.2 (a4).
4. **Bối cảnh Việt Nam.**
   - Việt Nam đứng thứ 4 về mức độ chấp nhận tiền mã hóa (c3.1).
   - Bài của Cục A05 trên Nhân Dân (c3.3) mô tả chợ đen P2P với uy tín "chỉ dựa vào đánh giá của các thành viên kỳ cựu". Đây là chỗ nối trực tiếp: bài toán trên Bitcoin-OTC là mô hình thu nhỏ của hiện tượng đang diễn ra trong nước.
5. **Khoảng trống trong nước.** Chỉ nên viết "chưa tìm thấy công bố trong nước về phát hiện bất thường trên đồ thị có dấu và động" (c2), và nêu c1.1, c1.2 là các hướng gần nhất. Cần tra thủ công thêm Google Scholar và kho luận văn trước khi khẳng định mạnh hơn.

### "Ý nghĩa thực tiễn"

1. **Ứng dụng trực tiếp lên cùng loại dữ liệu.** REV2 ở Flipkart (d1): 127/150, độ chính xác 84,6%, cùng tác giả với bộ dữ liệu.
2. **GNN trên đồ thị đã được dùng trong công nghiệp chống gian lận.**
   - GEM ở Alipay (d2): độ chính xác trên 98% với 10.000 tài khoản mỗi ngày.
   - Amazon dùng GNN tìm nhóm tác nhân xấu và chặn hơn 200 triệu đánh giá giả năm 2022 (d3).
   - Yelp đóng hơn 1.300 tài khoản thuộc vòng trao đổi đánh giá (d4).
3. **Chỉ dùng điểm uy tín thì không đủ.** Tsuchiya (a4, d6): 46% vendor Paxful bị khóa, và mô hình tốt hơn điểm uy tín khi đánh giá trực tuyến. Từ đó nêu được giá trị của việc chấm điểm bất thường cho từng lượt đánh giá (cạnh) để hỗ trợ kiểm duyệt.
4. **Gắn với Việt Nam.** Kết quả có thể áp dụng cho các cộng đồng mua bán P2P dựa trên đánh giá lẫn nhau (c3.3) và cho các sàn trong khuôn khổ thí điểm theo NQ 05/2025 (c3.5).
   - **[suy luận]** Đây là định hướng ứng dụng, không có nguồn nào khẳng định.
