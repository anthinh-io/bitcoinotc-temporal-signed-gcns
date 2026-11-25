# Cách các bài báo chèn cạnh bất thường và gán dấu, điểm cho cạnh chèn

Ticket: #2. Câu hỏi: AddGraph, TADDY (và bài báo TADDY trích dẫn là [9] cho cách chèn), StrGNN, TSAVGA chèn cạnh bất thường như thế nào (loại, tỉ lệ, chèn vào tập nào, chọn cặp nút ra sao)? Có bài nào gán dấu hoặc điểm (rating −10..+10) cho cạnh chèn trên đồ thị có dấu không? Các phê bình đã biết về đánh giá bằng chèn bất thường.

Quy ước ghi nguồn:

- **[V]**: đã kiểm tra trực tiếp trong nguồn (bài báo hoặc mã nguồn chính thức), kèm số trang PDF.
- **[S]**: suy luận của người viết từ nguồn, chưa có câu nào trong nguồn nói thẳng.
- Số trang là số trang của file PDF (trang 1 = trang đầu file), không phải số trang in trong kỷ yếu.

## Tóm tắt

- Bài báo TADDY trích dẫn là [9] chính là **NetWalk** (Yu et al., KDD 2018) [V, TADDY tr. 13, danh mục tài liệu]. AddGraph, StrGNN, TADDY, STCAD đều nói "follow [Yu et al., 2018]" khi chèn vào tập test. Cả họ phương pháp này dùng chung **một** quy trình chèn.
- Quy trình chung: 50% cạnh (hoặc 50% mốc thời gian) đầu làm tập train và coi là sạch. Tập test được chèn 1%, 5%, 10% cạnh giả. Cạnh giả là **cặp nút chưa từng nối** (không có trong toàn bộ đồ thị), chọn ngẫu nhiên. Trong mã của NetWalk và TADDY, cặp nút còn phải thuộc **hai cụm spectral clustering khác nhau**.
- Chỉ có **một loại** cạnh bất thường: cạnh ngẫu nhiên nối hai nút không liên quan (về bản chất là bất thường cấu trúc). Không có bài nào trong nhóm này chèn bất thường thời gian riêng.
- **Không bài nào gán dấu hay điểm cho cạnh chèn.** Mã TADDY bỏ hẳn cột rating của Bitcoin-OTC/Alpha, biến đồ thị thành vô hướng, và gán trọng số 1 cho mọi cạnh, cả thật lẫn giả [V, mã]. Hướng gần nhất là AddGraph: cạnh âm sinh ra lúc *huấn luyện* giữ nguyên trọng số w của cạnh dương gốc [V, AddGraph tr. 4]. Cách này áp dụng cho negative sampling khi train, không áp dụng cho cạnh chèn vào tập test.
- Phê bình mạnh nhất: Latapy & Rajeh (2026) cho thấy đặc trưng đồ thị tầm thường (bậc, trọng số, v.v.) cộng với bộ học cổ điển đã phát hiện cạnh chèn ngẫu nhiên "extremely well". Họ đạt AUC ≈ 0.96–0.97 trên Bitcoin-OTC, cao hơn TADDY [V, tr. 1, 6].

## Bảng theo từng bài báo

| Bài báo | Loại bất thường | Tỉ lệ | Chèn vào tập nào | Cách chọn cặp nút | Dấu / điểm của cạnh chèn | Nguồn |
|---|---|---|---|---|---|---|
| **NetWalk** (Yu et al., KDD 2018), tức TADDY [9] | Cạnh giả "not shown in the whole graph" | Tham số `anomaly_percent`. Mã mẫu đặt cứng `anomaly_num = 12` (dòng tính theo tỉ lệ bị comment) | Chỉ tập test (50% cạnh sau theo thứ tự). Train = 50% cạnh đầu | Bốc ngẫu nhiên m cặp (u, v). Giữ cặp có `labels[u] != labels[v]` (spectral clustering trên ma trận kề của *toàn bộ* đồ thị, k = 3). Bỏ self-loop, bỏ trùng, bỏ cặp đã có trong đồ thị. Cạnh giả được đặt vào **vị trí ngẫu nhiên** trong luồng test | Không có. Chỉ là cặp (u, v) | [V, mã] `chengw07/NetWalk` `src/framework/anomaly_generation.py` (commit 2aef4f3). **Chưa đọc được** toàn văn bài KDD (ACM trả 403), nên mô tả trong bài chưa được đối chiếu |
| **AddGraph** (Zheng et al., IJCAI 2019) | Như NetWalk | 1%, 5%, 10% (thí nghiệm không có thời gian). 5% cho thí nghiệm động chính. 10% cho phân tích tham số | Tập test (50% sau). Chèn **trước** khi cắt snapshot (1.000 cạnh/snapshot cho UCI, 6.000 cho Digg) | "follow the approach used in [Yu et al., 2018]" | Cạnh test: không nói gì. Cạnh âm khi *train*: với mỗi cạnh (i, j, w), thay đầu hoặc đuôi theo phân phối Bernoulli, được (i′, j′, w), **giữ nguyên w**. Dùng margin loss. Selective negative sampling bỏ cặp nếu f(i,j,w) > f(i′,j′,w) | [V] tr. 3–4 (negative sampling, Eq. 17). [V] tr. 5 (chèn, tỉ lệ, chia 50/50) |
| **StrGNN** (Cai et al., CIKM 2021; arXiv 2005.07427) | Như NetWalk | 1%, 5%, 10% | Tập test (50% cạnh sau) | "follow the approach used in [Yu et al., 2018]". Cạnh âm khi train là "context-dependent": lấy một cạnh thật {x_a, x_b}, thay x_a bằng nút ngẫu nhiên, giữ lại nếu cặp mới không thuộc đồ thị bình thường | Không có. Bitcoin chỉ được mô tả là "rates another user" | [V] arXiv v2 tr. 4 (negative sampling), tr. 5 (chèn). Không kiểm tra bản CIKM hay mã nguồn |
| **TADDY** (Liu et al., TKDE 2021; arXiv 2106.09876) | Như NetWalk: "randomly link p_A × m_t pairs of disconnected nodes" | p_A ∈ {1%, 5%, 10%} **trên mỗi snapshot** (m_t = số cạnh gốc của snapshot t) | Tập test (50% mốc thời gian sau). "the training data is totally clean" | "follow the approach used in [9]". Mã: spectral clustering k = 42 trên toàn bộ đồ thị, giữ cặp khác cụm, loại cặp đã có trong *toàn bộ* đồ thị. `anomaly_num = floor(p × |test|)` tính **trên cả tập test**, rải vào vị trí ngẫu nhiên rồi mới cắt snapshot cố định (`btc_otc`: 2.000 cạnh), nên tỉ lệ p trên mỗi snapshot chỉ đúng **xấp xỉ** [S]. Cạnh âm khi train: bốc ngẫu nhiên m_t cặp không thuộc tập cạnh train | **Bỏ rating**: `edges[:, 0:2]`. Sắp theo cột thời gian. Đổi (u, v) thành (min, max), tức **vô hướng**. Bỏ trùng. `wei = np.ones_like(row)` cho mọi cạnh | [V] tr. 7 (negative sampling), tr. 8–9 (chèn, 1/5/10%, chia 50/50). [V, mã] `yixinliu233/TADDY_pytorch` `codes/AnomalyGeneration.py`, `0_prepare_data.py` (commit dfe15dd) |
| **TSAVGA** (preprint Research Square rs-7806501) | Train: cạnh giả **giữa các cụm khác nhau**, số lượng theo kích thước cụm (Eq. 8). Test: như TADDY | Train: 1:1 với số cạnh gốc. Test: 1%, 5%, 10% | **Cả hai**: train được tăng cường bằng cạnh giả có nhãn. Test chèn theo [13] TADDY và [14] (Chen et al., ICDE 2024) | Spectral clustering trên độ tương đồng cosine của embedding từ module attention thời gian–cấu trúc. N_ij cạnh giữa cụm i và j tỉ lệ với \|l(i)\|·\|l(j)\|. Nút được chọn ngẫu nhiên, không lặp | Không nói gì về dấu hay điểm | [V] tr. 8–9 (§4.2, Eq. 5–10), tr. 13 (§5.1.3) |
| **STCAD** (arXiv 2505.08330, tham khảo thêm) | Như NetWalk | Train: số cạnh giả bằng số cạnh thật của snapshot. Test: 1%, 5%, 10% | Test chèn theo Yu et al. (2018). Train dùng negative sampling 1:1 | Train: cạnh "never occur from previous snapshots". Test: theo NetWalk | Không có. Bitcoin chỉ được gọi là "rating datasets" | [V] tr. 8, tr. 11 |

### Chi tiết đáng chú ý trong mã (quan sát, chưa kiểm chứng tác động)

- Sau khi đánh lại chỉ số nút về 0-based (`np.unique(..., return_inverse=True)`), mã TADDY vẫn tra nhãn cụm bằng `labels[x[0] - 1]`. Cách tra này lệch một chỉ số, và nút 0 tra vào `labels[-1]`. Ngoài ra `edgeList2Adj` tạo ma trận kích thước `max(id)` × `max(id)` cũng theo giả định 1-based [V, mã]. Hệ quả [S]: điều kiện "khác cụm" có thể chỉ lọc gần đúng, nên cạnh giả của TADDY trên thực tế gần với cặp ngẫu nhiên thuần.
- Mã TADDY có thêm `anomaly_generation2` (không dùng spectral clustering). Comment trong hàm ghi đây là "the actual generation method used for Netwalk (shown in matlab version)". `0_prepare_data.py` gọi bản có clustering (`anomaly_generation`) [V, mã].
- Vì cạnh giả được loại nếu có trong **toàn bộ** đồ thị (cả train lẫn test), mọi cạnh chèn đều là cặp **chưa từng tương tác**. Ngược lại, rất nhiều cạnh thật trong test là cặp đã từng tương tác hoặc nối với nút bậc cao [S]. Đây là nguồn của phê bình ở mục dưới.

## Có bài nào gán dấu hoặc điểm cho cạnh chèn không?

Trong các nguồn đã kiểm tra: **không có**.

- NetWalk, StrGNN, TADDY, STCAD, TSAVGA: cạnh chèn chỉ là cặp nút (u, v). Các bài này không nói gì về trọng số hay dấu [V].
- TADDY chạy Bitcoin-OTC/Alpha như đồ thị **vô hướng, không trọng số**. Rating bị bỏ ngay ở bước tiền xử lý [V, mã]. Nhóm này không dùng thông tin dấu, nên số liệu công bố trên Bitcoin-OTC của TADDY/StrGNN/AddGraph là kết quả trên đồ thị **không dấu** [S].
- AddGraph: cạnh âm khi train mang trọng số w của cạnh dương gốc [V, tr. 4]. Đây là tiền lệ duy nhất tìm được cho việc "cạnh giả kế thừa thuộc tính của cạnh thật", nhưng chỉ áp dụng khi huấn luyện.
- Các bài dùng rating làm **nhãn thật** thay vì chèn:
  - **MGS-TGCN** (Li et al., arXiv 2211.13123): coi bài toán là phân loại dấu cạnh trên đồ thị có dấu theo thời gian. Bài này không chèn cạnh, nhãn là dấu thật. Nút "fraud" chỉ dùng để trực quan hóa (nút có nhiều cạnh âm hơn cạnh dương) [V, tr. 1, 7, 9].
  - **SLADE** (Lee et al., KDD 2024, arXiv 2402.11933): làm ở **mức nút**. Người dùng có tổng điểm nhận được < 0 thì trạng thái tổng là bất thường. Với những người này, thời điểm họ nhận một rating âm được gán nhãn động là bất thường. Rating chỉ dùng để tạo nhãn, không đưa vào đầu vào của mô hình [V, tr. 10–11].
  - **TAS-GNN** (arXiv 2603.13290, 02/2026): GNN có dấu cho Bitcoin-Alpha, nhắc tới "recursive Web-of-Trust labeling". Chưa đọc toàn văn, chưa rõ làm mức nút hay mức cạnh [chưa kiểm tra].

## Phê bình về đánh giá bằng chèn bất thường

- **Latapy & Rajeh, "Trivial Graph Features and Classical Learning are Enough to Detect Random Anomalies"** (arXiv 2603.01841, 03/2026; trang arXiv có liên kết DOI ICDM 2025). Các phương pháp "are mostly evaluated through their ability to detect randomly injected links". Bài dùng đặc trưng tầm thường (bậc, bậc có trọng số, trọng số cạnh, phân bố bậc trong đồ thị lịch sử gần nhất) cùng bộ phân loại scikit-learn. Bài so sánh với NetWalk, AddGraph, StrGNN, TADDY, RustGraph, SLADE trên đúng sáu bộ benchmark này. Trên Bitcoin-OTC, TGF đạt AUC ≈ 0.96–0.97, so với ≈ 0.93–0.95 của TADDY [V, tr. 1, 4, 6]. Chỉ đọc kỹ dòng Bitcoin trong bảng, chưa kiểm từng ô của các bộ dữ liệu khác. Lưu ý [S]: TGF được huấn luyện có giám sát trên nhãn cạnh chèn, nên so sánh không hoàn toàn cùng điều kiện. Dù vậy, kết quả cho thấy cạnh chèn ngẫu nhiên tách được chỉ bằng bậc nút.
- **StrGNN** tự nhận xét về *negative sampling*: mẫu âm từ phân phối "context-independent" (random hoặc injected sampling) "would be very different from the data distribution", nên khi học sẽ kém [V, tr. 4]. Nhận xét này áp dụng cho mẫu âm khi train, nhưng cùng lập luận cũng áp dụng cho cạnh chèn khi test [S].
- **AddGraph** quan sát thấy AUC ở 1% thấp hơn ở 5% và cho rằng nguyên nhân là phân phối điểm bị ảnh hưởng bởi tỉ lệ chèn [V, tr. 5]. Như vậy kết quả phụ thuộc vào tham số chèn.
- **Survey 2406.00134** chỉ nhắc chung rằng dữ liệu tổng hợp có thể tạo ra "unrealistic scenarios with topological and attribute value limitations" [V, tr. 22]. Bài không phân tích riêng cách chèn của NetWalk.
- **TSAVGA** cũng ngầm thừa nhận cạnh ngẫu nhiên thiếu đa dạng. Bài thêm chèn theo cụm để "simulate real-world anomalies" [V, tr. 8–9]. Tuy vậy, tập test của bài vẫn dùng cách chèn ngẫu nhiên cũ.
- Benchmark **BAG** (AAAI 2026) đánh giá 25 mô hình trên 10 tập dữ liệu gồm cả bất thường tổng hợp lẫn tự nhiên. Chỉ đọc được tóm tắt qua kết quả tìm kiếm, trang AAAI trả lỗi 520 [chưa kiểm tra].

## Hàm ý cho đồ thị có dấu

Các lựa chọn còn mở, chưa quyết định và chưa xếp hạng:

1. **Chèn không dấu, đúng như NetWalk/TADDY**: bỏ dấu và điểm của mọi cạnh, chỉ dùng cho biến thể đối chiếu với số đã công bố (glossary: *Edge-count snapshot*). So sánh được trực tiếp với số đã công bố. Mô hình có dấu sẽ không có thông tin dấu trong biến thể này.
2. **Kế thừa dấu hoặc điểm từ một cạnh thật**, theo tiền lệ AddGraph: cạnh giả (u′, v′) lấy rating của một cạnh thật ngẫu nhiên trong cùng snapshot. Phân phối dấu của cạnh giả khớp với dữ liệu (Bitcoin-OTC chủ yếu là rating dương), nên mô hình không thể nhận ra cạnh giả chỉ qua dấu.
3. **Lấy rating ngẫu nhiên từ phân phối biên của snapshot hoặc tập train**: tương tự (2) nhưng tách khỏi cạnh cụ thể nào.
4. **Gán dấu theo ngữ nghĩa bất thường**: ví dụ chèn cạnh âm mạnh (≤ −5) tới người dùng có lịch sử tốt, hoặc cụm cạnh dương dày đặc kiểu "ring" tự nâng uy tín. Cách này gần với gian lận thật hơn, nhưng **chưa có tiền lệ** trong các bài đã đọc. Dấu của cạnh có thể trở thành tín hiệu dễ, giống phê bình của Latapy & Rajeh.
5. **Dấu đồng nhất** (mọi cạnh giả đều +1, hoặc đều −1): đơn giản nhất. Nếu khác với phân phối thật, mô hình có dấu sẽ có lợi thế không công bằng so với bản không dấu dùng làm ablation [S].
6. **Không chèn, dùng nhãn thật từ rating**, như SLADE và MGS-TGCN (glossary: *Fraudulent user*, *Flag week*): tránh hoàn toàn câu hỏi về dấu của cạnh chèn, nhưng đổi bài toán sang mức nút hoặc phân loại dấu. Có thể dùng như phép kiểm tra bổ sung bên cạnh chèn.

Các điểm cần lưu ý khi chọn [S]:

- Ablation có dấu và không dấu chỉ công bằng nếu dấu của cạnh giả không tự nó lộ ra là giả. Phương án (2) và (3) giữ được điều kiện này, phương án (4) và (5) thì không.
- Hướng của cạnh: NetWalk/TADDY làm vô hướng. Bitcoin-OTC có hướng (u chấm v), nên cần quyết định cạnh giả có hướng hay không, và nút nào là nút nguồn (liên quan đến định nghĩa *Anomalous node* trong glossary).
- Tập loại trừ cặp nút: loại cặp có trong *toàn bộ* đồ thị như TADDY làm sẽ rò thông tin từ tương lai vào tập test. Loại cặp chỉ có trong quá khứ (như STCAD làm khi train) thì không rò.
- Với snapshot theo tuần thay cho snapshot theo số cạnh, "p_A × m_t trên mỗi snapshot" (đúng như bài TADDY mô tả) khác với "p × |test| rải ngẫu nhiên" (như mã TADDY làm).

## Nguồn

Bản PDF lưu trong repo (`.local/Graph Neural Networks (GNNs)/Papers/`, không commit):

- Zheng, Li, Li, Li, Gao. *AddGraph: Anomaly Detection in Dynamic Graph Using Attention-based Temporal GCN*. IJCAI 2019. File `0614.pdf`. https://www.ijcai.org/proceedings/2019/614
- Liu, Pan, Wang, Xiong, Wang, Chen, Lee. *Anomaly Detection in Dynamic Graphs via Transformer* (TADDY). TKDE. File `2106.09876v2.pdf`. https://arxiv.org/abs/2106.09876
- *TSAVGA* (Temporal-Structural Attention + GRU VGAE), preprint Research Square. File `rs.3.rs-7806501v1.pdf`.
- *STCAD*. arXiv 2505.08330. File `2505.08330v1.pdf`. https://arxiv.org/abs/2505.08330
- Survey dynamic graph anomaly detection. arXiv 2406.00134. File `2406.00134v1.pdf`. https://arxiv.org/abs/2406.00134
- Li et al. *Motif-aware group signed temporal GCN for anomaly detection in bitcoin trust networks* (MGS-TGCN). arXiv 2211.13123. https://arxiv.org/abs/2211.13123

Nguồn trên web:

- Yu, Cheng, Aggarwal, Zhang, Chen, Wang. *NetWalk: A Flexible Deep Embedding Approach for Anomaly Detection in Dynamic Networks*. KDD 2018. https://dl.acm.org/doi/10.1145/3219819.3220024 (chỉ đọc được trang tóm tắt)
- Mã NetWalk chính thức: https://github.com/chengw07/NetWalk/blob/2aef4f3712fabc36e5d57fdcc0ce57ed31797719/src/framework/anomaly_generation.py
- Mã TADDY chính thức: https://github.com/yixinliu233/TADDY_pytorch/blob/dfe15ddbbc179ed0ca9a96482fe1696dfbcb5bcd/codes/AnomalyGeneration.py và https://github.com/yixinliu233/TADDY_pytorch/blob/dfe15ddbbc179ed0ca9a96482fe1696dfbcb5bcd/0_prepare_data.py
- Cai, Chen, Luo, Gui, Ni, Li, Chen. *Structural Temporal Graph Neural Networks for Anomaly Detection in Dynamic Graphs* (StrGNN). CIKM 2021. arXiv: https://arxiv.org/abs/2005.07427
- Latapy, Rajeh. *Trivial Graph Features and Classical Learning are Enough to Detect Random Anomalies*. arXiv 2603.01841. https://arxiv.org/abs/2603.01841
- Lee, Kim, Shin. *SLADE: Detecting Dynamic Anomalies in Edge Streams without Labels via Self-Supervised Learning*. KDD 2024. https://arxiv.org/abs/2402.11933
- *TAS-GNN: A Status-Aware Signed Graph Neural Network for Anomaly Detection in Bitcoin Trust Systems*. arXiv 2603.13290. https://arxiv.org/abs/2603.13290 (chỉ đọc tóm tắt)
- *BAG: Benchmarking Anomaly Detection on Dynamic Graphs*. AAAI 2026. https://ojs.aaai.org/index.php/AAAI/article/view/38510 (chưa đọc được)
