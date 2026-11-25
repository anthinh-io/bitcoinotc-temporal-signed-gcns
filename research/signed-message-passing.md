# Truyền tin có dấu (signed message passing) và cách ghép với GCN + GRU + attention

> Ticket nghiên cứu: issue #3. Tài liệu này chỉ ghi lại kết quả tra cứu, không chốt quyết định nào.
> Ngày tra cứu: 2026-11-25.
>
> Quy ước mức độ tin cậy:
> - **[V]** (verified): đã đọc trực tiếp trong paper, trong mã nguồn hoặc trong tài liệu chính thức, có trích dẫn.
> - **[I]** (inferred): suy luận từ nguồn, chưa chạy thử hoặc chưa đọc toàn văn.

## 1. Bảng so sánh các lớp signed

| Phương pháp | Cách gom láng giềng + / − | Có hướng? | Dùng độ lớn rating? | Code |
|---|---|---|---|---|
| **SGCN** (Derr, Ma, Tang, ICDM 2018) [1] | Mỗi node có hai biểu diễn là h^B ("bạn", balanced) và h^U ("thù", unbalanced). **Lớp 1:** h^B = σ(W^B[mean_{N+} h, h_i]) và h^U = σ(W^U[mean_{N−} h, h_i]). **Lớp l>1:** h^B gom h^B của N+ và h^U của N−; h^U gom h^U của N+ và h^B của N−. Cơ chế này mã hoá các quy tắc "bạn của bạn", "thù của thù" và "thù của bạn" theo balance theory. Loss gồm logistic đa lớp (+/−/không cạnh) cộng triplet loss khoảng cách. **[V]** | **Không.** Paper chạy trên "undirected signed networks" (§IV-A). **[V]** | Không, chỉ dùng dấu. **[V]** | PyG `SignedConv` / `SignedGCN` [5]; bản gốc trên PyTorch: benedekrozemberczki/SGCN [13] |
| **SDGNN** (Huang, Shen, Hou, Cheng, AAAI 2021) [2] | Có 4 quan hệ "signed directed relation": out+, in+, out−, in−. Mỗi quan hệ có một aggregator riêng (mean hoặc attention kiểu GAT). Kết quả được nối với z_u rồi đi qua MLP. Loss = L_sign + λ1·L_direction (theo status theory) + λ2·L_triangle (tam giác thoả balance/status). Paper cho thấy khoảng 69% triad trên Bitcoin-OTC thoả cả hai lý thuyết (Table 1). **[V]** | **Có**, đây là điểm khác chính so với SGCN. **[V]** | Không. Bản PyG Signed Directed chỉ lọc theo `s > 0` / `s < 0` [9]. **[V]** | huangjunjie-cs/SiGAT (`sdgnn.py`) [14]; `torch_geometric_signed_directed.nn.signed.SDGNN` [9] |
| **MGS-TGCN** (Li et al., arXiv 2211.13123) [3] | Có hai luồng là Graph-Emb⁺ và Graph-Emb⁻ (Eq. 8–9). **Luồng +:** (S⁺⊙A)·DyEmb⁺_{t−1}·W3 + (S⁻⊙A)·DyEmb⁻_{t−1}·W4. **Luồng −:** (S⁻⊙A)·DyEmb⁻_{t−1}·W3 + (S⁺⊙A)·DyEmb⁺_{t−1}·W4, tức là W3/W4 dùng chung và vai trò đảo nhau. **Motif:** ma trận motif M_t = Σ α_i M_{t,i} gồm 8 loại motif đếm bằng PGD, thay cho A trong một GCN 2 lớp. **Group balance:** C nhóm tiềm ẩn (giống GS-GCN), có ma trận gán xác suất theo từng snapshot. **[V]** | Paper không nói rõ. Ký hiệu A_t ∈ R^{n×n} nhưng không bàn về hướng. **[I]** | Không, chỉ dùng ma trận dấu S. **[V]** | Không tìm thấy code. **[V]** (paper không có link) |
| **wsGAT** (Grassia & Mangioni, 2021) [4] | Mở rộng GAT cho cạnh vừa có dấu vừa có trọng số. Model dự đoán được cả signed weight. **[V, chỉ qua abstract]** Chi tiết công thức chưa đọc. **[I]** | Chưa kiểm tra. | **Có** theo abstract ("weighted and signed"). **[V, abstract]** | Chưa kiểm tra. |
| **MSGNN / `MSConv`** (He et al., LoG 2022) [9] | Dùng magnetic signed Laplacian (phổ, Chebyshev bậc K), có pha q để mã hoá hướng. Dấu nằm trực tiếp trong A, không tách thành hai kênh. **[V, docstring]** | **Có** (qua pha phức). **[V]** | **Có**: `forward(x_real, x_imag, edge_index, edge_weight)`, degree tính trên \|A\| (`absolute_degree=True`). **[V]** | `torch_geometric_signed_directed.nn.general.MSConv` [9] |
| **TrustGuard**, phần spatial (Wang et al., IEEE TDSC) [6] | Mỗi node có hai vai trò: trustee (gom từ in-neighbours) và trustor (gom từ out-neighbours), sau đó gộp bằng một lớp FC. Message là h_v ⊕ ω với ω = W_te·w (embedding của mức trust). Hệ số robust tính bằng cosine similarity, cạnh dưới ngưỡng bị cắt. **[V]** | **Có.** **[V]** | Về hình thức có (w là vector mức trust). Nhưng trên Bitcoin-OTC/Alpha dữ liệu bị nhị phân hoá thành {Trust, Distrust} one-hot, nên độ lớn bị bỏ. **[V]** | github.com/Jieerbobo/TrustGuard (PyG 2.2.0) [15] |
| **SEMBA** (Sharma, Raghavendra, …, Kumar; arXiv 2207.03408) [7] | Mỗi node có hai bộ nhớ, positive memory và negative memory. Khi có cạnh (u,v,+): memory⁺ của u cập nhật từ memory⁺ của v, memory⁻ từ memory⁻. Khi có cạnh (u,v,−): memory⁺ cập nhật từ memory⁻ và ngược lại ("balanced aggregation"). **[V]** | Có ("directed continuous-time dynamic signed network"). **[V]** | Có một task là "signed weight prediction". Việc dùng độ lớn làm input chưa kiểm tra. **[I]** | Không tìm thấy link code trong paper. **[V]** |

Ghi chú: MGS-TGCN trích SEMBA dưới tên cũ "Signed Link Representation in Continuous-Time Dynamic Signed Networks" (Raghavendra et al. 2022). Đây là cùng một arXiv ID, 2207.03408. **[V]**

## 2. Các công trình kết hợp signed + temporal

| Công trình | Thành phần thời gian | Mục tiêu / task | Có làm anomaly detection? |
|---|---|---|---|
| **MGS-TGCN** [3] | Discrete-time, 12 snapshot (8/1/3). Không có GRU: "dynamic embedding" là **trung bình trượt có trọng số học được** (Eq. 12–13, cửa sổ = 2). Global embedding cũng là trung bình trượt (Eq. 7). **[V]** | Phân loại cạnh trên Bitcoin-OTC/Alpha, đánh giá bằng F1/Acc/AP/Recall. **[V]** | **Có trên danh nghĩa.** Bản chất là supervised edge-sign classification: dùng G_{1:t} để dự đoán nhãn Y_t; tại snapshot t thì S⁺_t và S⁻_t được đặt toàn số 1 vì chưa biết dấu. **[V]** Có mâu thuẫn nội bộ: kết quả OTC của MGS-TGCN là F1 0.342 / Acc 0.805 ở Table 3 nhưng F1 0.346 / Acc 0.745 ở Table 4. **[V]** |
| **TrustGuard** [6] | Discrete-time, 10 snapshot chia đều theo thời gian. Lớp temporal là attention scaled dot-product có positional embedding học được, multi-head, chạy trên chuỗi embedding của node qua các snapshot. Không thấy cửa sổ trượt: attention phủ mọi snapshot trước thời điểm đích. **[V]** | Dự đoán mức trust u→v tại T+ΔT (single-/multi-timeslot, cả node chưa thấy) trên Bitcoin-OTC/Alpha. **[V]** | **Không phát hiện, chỉ chống chịu.** Mục tiêu là bền vững trước tấn công (cạnh độc hại, on-off attack), không chấm điểm bất thường. **[V]** |
| **SEMBA** [7] | Continuous-time, kiểu TGN: memory module + temporal batching. **[V]** | Dự đoán dấu, sự tồn tại và signed weight của link tương lai trên BTC-OTC, BTC-Alpha, WikiRfA, Epinions. **[V]** | Không. **[V, abstract]** |
| **DGTEN** (Usman & Lee, arXiv 2510.07620) [8] | Positional encoding + attention đa đầu dựa trên KAN + module ODE residual. GNN Gaussian có lượng hoá độ bất định. **[V, abstract]** | Đánh giá trust động trên Bitcoin-OTC/Alpha. **[V]** | Không trực tiếp. Có cơ chế giảm trọng số cạnh khả nghi để chống on-off/sabotage. **[V, abstract]** |
| **Regier et al., arXiv 2605.26290** [10] | SE-SGformer kết hợp LSTM trên quỹ đạo embedding và temporal attention đa đầu. | Dynamic link prediction. | **ĐÃ RÚT BÀI (withdrawn).** Lý do tác giả ghi: "simulation results are incorrect". **Không dùng làm bằng chứng.** **[V]** |
| **TAS-GNN** (arXiv 2603.13290) [11] | Theo abstract thì không có thành phần thời gian. **[I]** | Phát hiện bất thường trong Web-of-Trust (Bitcoin-Alpha): hai kênh trust/distrust riêng, gộp bằng status-aware attention. **[V, abstract]** | **Có**, nhưng là bài toán tĩnh và có vẻ ở mức node. **[I]** |

**Tóm lại [I]:** chưa tìm thấy công trình nào đã peer-review mà ghép **đúng** một lớp signed GNN với GRU và attention trên cửa sổ snapshot (kiểu AddGraph) cho **edge anomaly detection**. Gần nhất là:
- MGS-TGCN: signed + discrete-time + "anomaly", nhưng thành phần thời gian là moving average và bài toán là supervised;
- TrustGuard: signed (có hướng) + temporal attention, nhưng task là dự đoán trust.

Phạm vi tìm kiếm có giới hạn: một vài truy vấn web cộng với related work của MGS-TGCN, chưa đọc hết related-work của TrustGuard.

## 3. PyTorch Geometric hỗ trợ sẵn gì

### 3.1 `torch_geometric.nn.SignedConv` [5]
- Signature là `SignedConv(in_channels, out_channels, first_aggr, bias=True)` và `forward(x, pos_edge_index, neg_edge_index)`. Lớp này cài đặt đúng Eq. 3–6 của SGCN. Output là `cat([out_pos, out_neg])`, nên kích thước bằng 2×out_channels. **[V, source]**
- Mặc định `aggr='mean'`. **[V]**
- **Không nhận edge weight.** Nếu truyền vào một `SparseTensor` thì giá trị bị xoá: `adj_t.set_value(None)` trong `message_and_aggregate`. **[V]**
- **Hướng cạnh:** lớp không đối xứng hoá cạnh. `MessagePassing` có mặc định `flow="source_to_target"` [16], nên với `edge_index` dạng rater→ratee, mỗi node chỉ gom tin từ những người đã **chấm nó**. Muốn có chiều ngược lại thì phải tự thêm cạnh đảo. **[V, mặc định flow; hệ quả là I]**

### 3.2 `torch_geometric.nn.models.SignedGCN` [5]
- Gồm `SignedConv` xếp chồng, một bộ phân loại `Linear(2·hidden, 3)` cho ba lớp +/−/không cạnh, và loss = NLL + λ·(triplet⁺ + triplet⁻). **[V]**
- `create_spectral_features` tạo feature bằng TruncatedSVD trên ma trận **đã đối xứng hoá** (thêm cạnh đảo, giá trị ±1). **[V]**
- `test()` tính ROC-AUC trên **nhãn dự đoán cứng** (argmax), không dùng xác suất, nên AUC báo ra thấp hơn ý nghĩa thông thường. **[V, source]**
- Ví dụ chính thức `examples/signed_gcn.py` nạp `BitcoinOTC(edge_window_size=1)`, **gộp tất cả snapshot thành một đồ thị tĩnh** rồi chia ngẫu nhiên 80/20. Ví dụ này không có yếu tố thời gian. **[V]**

### 3.3 `torch_geometric.datasets.BitcoinOTC` [5]
- Lấy từ paper EvolveGCN, tải `soc-sign-bitcoinotc.csv` từ SNAP. **[V]**
- `edge_attr` **giữ nguyên rating −10..+10** (kiểu long), không chỉ giữ dấu. **[V, source]**
- `edge_index` giữ hướng source→target, không thêm cạnh đảo. **[V]**
- Có 138 bước, mỗi bước khoảng 13.8 ngày. Vòng lặp chỉ tăng chỉ số snapshot tối đa 1 lần cho mỗi cạnh, nên nếu dữ liệu có khoảng trống dài thì ranh giới sẽ không đúng 13.8 ngày tuyệt đối; điều này còn phụ thuộc vào thứ tự thời gian trong file. **[V code / I hệ quả]**
- `edge_window_size=10` (mặc định) cho snapshot t chứa mọi cạnh tạo trong 10 bước gần nhất, tức là các snapshot **chồng lấn, tích luỹ**. Muốn có "cạnh mới tại t" kiểu AddGraph thì cần `edge_window_size=1`. **[V]**
- `num_nodes` = 6,005 (max ID + 1 sau khi trừ min), trong khi số node thực là 5,881. **[V]**
- Không có node feature. **[V]**
- Cách chia snapshot khác nhau giữa các nguồn: PyG dùng 138, MGS-TGCN dùng 12, TrustGuard dùng 10. **[V]**

### 3.4 PyTorch Geometric Temporal [12]
- Có các lớp recurrent `GConvGRU`, `GConvLSTM`, `TGCN`, `EvolveGCNH/O`, `A3TGCN` (`attentiontemporalgcn.py`), `DCRNN`, `LRGCN`, … và các lớp attention (`ASTGCN`, `GMAN`, `MTGNN`, …). Có các lớp signal `DynamicGraphTemporalSignal` và `StaticGraphTemporalSignal`. **[V, cây repo]**
- **Không có lớp signed nào và không có dataset Bitcoin.** **[V, liệt kê thư mục]**
- Các lớp này bọc `GCNConv` / `ChebConv` với `edge_weight`. Nếu truyền rating có dấu làm `edge_weight` thì tổng trọng số của một node có thể ≤ 0. Trong `gcn_norm`, `deg.pow_(-0.5)` chỉ che giá trị `inf`, nên **degree âm sẽ cho NaN**. **[V code / I hệ quả]**
- Repo vẫn được cập nhật (push gần nhất 2026-05), khoảng 3k sao. **[V]**

### 3.5 PyTorch Geometric Signed Directed (thư viện mở rộng, LoG 2023) [9]
- Có `SGCN`, `SDGNN`, `SiGAT`, `SNEA`, `SSSNET`, `MSGNN/MSConv`. **[V]**
- Dataset `bitcoin_otc` có 3 cột (src, dst, rating −10..10) và **không có timestamp**, nên chỉ dùng được như đồ thị tĩnh. **[V, đã tải file]**
- `SignedData.separate_positive_negative()` tách thành `edge_weight_p` và `edge_weight_n = −w`, tức là **giữ độ lớn**. **[V]**
- `SDGNN` trong thư viện này là transductive: embedding `x` là một `nn.Parameter` và danh sách kề được dựng cố định lúc khởi tạo. Vì vậy không dùng thẳng cho từng snapshot được. **[V code / I hệ quả]**

## 4. Hàm ý cho kiến trúc (chỉ liệt kê lựa chọn, chưa chốt)

### 4.1 Hai rủi ro cần quyết định trước
1. **Rò rỉ nhãn (leakage).** GCN của AddGraph dùng A_t của **chính snapshot đang chấm điểm** (Eq. 1–4 [17]). Nếu bản signed truyền tin qua dấu của chính cạnh cần chấm, và "rating âm" lại là proxy của bất thường, thì nhãn bị rò vào input. Các lựa chọn:
   - (a) Làm như MGS-TGCN: tại t đặt S⁺ = S⁻ = 1, tức là truyền tin không dấu ở snapshot hiện tại.
   - (b) Chỉ dùng dấu của G_{≤t−1} cho phần signed, còn A_t thì không dấu.
   - (c) Loại cạnh đang chấm khỏi message passing.
2. **Định nghĩa "bất thường".**
   - (a) Kiểu AddGraph: tiêm cạnh giả và dùng margin loss. Cạnh tiêm vào chưa có rating, nên phải gán dấu hoặc rating cho nó, chẳng hạn ngẫu nhiên hoặc theo phân phối.
   - (b) Kiểu MGS-TGCN: supervised, cạnh âm được coi là bất thường.
   - (c) Kết hợp hai cách.

   Lựa chọn này ảnh hưởng trực tiếp đến việc lớp signed nhìn thấy những gì.

### 4.2 Lựa chọn lớp signed
- **A. `SignedConv` hai kênh (SGCN).** Có sẵn trong PyG. Cách nối với phần thời gian:
  - cho h^B và h^U mỗi kênh một GRU/attention riêng; hoặc
  - nối [h^B‖h^U] trước rồi mới đưa qua GRU + CAB như AddGraph.

  Hạn chế: không có hướng (trừ khi tự thêm cạnh đảo hoặc tách in/out), không có trọng số.
- **B. Bốn quan hệ có hướng (kiểu SDGNN).** Tách láng giềng thành out+, in+, out−, in− với 4 aggregator. Phù hợp với tính có hướng của Bitcoin-OTC (rater→ratee). Phải tự cài cho từng snapshot vì bản thư viện là transductive. Có thể dùng `RGCNConv` hoặc `HeteroConv` của PyG với 4 loại cạnh. **[I]**
- **C. Ma trận dấu kiểu MGS-TGCN (Eq. 8–9).** Có thể cộng thêm motif và group balance, nhưng phải tự cài (không có code) và chi phí cao hơn (cần đếm motif bằng PGD).
- **D. Phổ có dấu và có hướng (`MSConv`).** Dùng thẳng rating làm `edge_weight`, xử lý hướng bằng pha q. Một kênh duy nhất, có trong PyG Signed Directed.

### 4.3 Lựa chọn dùng độ lớn rating
- **i.** Chỉ dùng dấu (SGCN/SDGNN/TrustGuard đều làm vậy trên Bitcoin).
- **ii.** Dùng |w| làm trọng số **bên trong từng kênh dấu**. Cách này cần một weighted mean tự viết, vì `SignedConv` bỏ trọng số.
- **iii.** Đưa rating, có thể chuẩn hoá về [−1,1], làm **edge feature** trong message: kiểu TrustGuard ω = W·w, hoặc `GINEConv`/`TransformerConv` có `edge_dim`. **[I]**
- **iv.** Đưa signed weight vào hệ số attention, kiểu wsGAT.
- **v.** Dùng rating trực tiếp làm `edge_weight` trong `MSConv`.

  Không nên đưa rating có dấu vào `GCNConv` / PyG-Temporal vì degree âm sẽ cho NaN (xem §3.4).

### 4.4 Ablation unsigned
- GCN thường trên A nhị phân, hoặc trên |A| làm trọng số, giữ nguyên GRU + CAB (tức AddGraph gốc). Như vậy chỉ thay đúng lớp không gian.

### 4.5 Dữ liệu
- `BitcoinOTC` của PyG đã giữ rating và hướng. Cần chọn `edge_window_size`: 1 cho cạnh mới, >1 cho snapshot tích luỹ. Cũng cần chọn độ hạt thời gian (138 bước của PyG, hay 10–12 bước như TrustGuard/MGS-TGCN).

## 5. Nguồn

1. Derr, Ma, Tang. *Signed Graph Convolutional Network*. ICDM 2018. https://arxiv.org/abs/1808.06354
2. Huang, Shen, Hou, Cheng. *SDGNN: Learning Node Representation for Signed Directed Networks*. AAAI 2021. https://arxiv.org/abs/2101.02390
3. Li et al. *Motif-aware group signed temporal GCN for anomaly detection in bitcoin trust networks*. https://arxiv.org/abs/2211.13123 (đọc bản PDF local v2)
4. Grassia, Mangioni. *wsGAT: Weighted and Signed Graph Attention Networks for Link Prediction*. Complex Networks 2021. https://arxiv.org/abs/2109.11519
5. PyG docs và source:
   - https://pytorch-geometric.readthedocs.io/en/latest/generated/torch_geometric.nn.conv.SignedConv.html
   - https://pytorch-geometric.readthedocs.io/en/latest/generated/torch_geometric.nn.models.SignedGCN.html
   - https://pytorch-geometric.readthedocs.io/en/latest/generated/torch_geometric.datasets.BitcoinOTC.html
   - Source: https://github.com/pyg-team/pytorch_geometric/blob/master/torch_geometric/nn/conv/signed_conv.py , `nn/models/signed_gcn.py`, `datasets/bitcoin_otc.py`, `examples/signed_gcn.py`, `nn/conv/gcn_conv.py`
6. Wang, Yan, Lan, Bertino, Pedrycz. *TrustGuard: GNN-based Robust and Explainable Trust Evaluation with Dynamicity Support*. IEEE TDSC. https://arxiv.org/abs/2306.13339 (đọc bản HTML v4)
7. Sharma, Raghavendra, Lee, Kumar M, Kumar. *Representation Learning in Continuous-Time Dynamic Signed Networks* (SEMBA). https://arxiv.org/abs/2207.03408
8. Usman, Lee. *DGTEN*. https://arxiv.org/abs/2510.07620 (chỉ đọc abstract)
9. PyTorch Geometric Signed Directed: https://github.com/SherylHYX/pytorch_geometric_signed_directed (`nn/signed/SDGNN.py`, `nn/general/MSConv.py`, `data/signed/SignedData.py`, `data/signed/SDGNN_real_data.py`, `datasets/bitcoin_otc.csv`)
10. Regier et al. *Dynamic Link Prediction with Temporally Enhanced Signed GNNs*. **Đã rút bài.** https://arxiv.org/abs/2605.26290
11. Xue et al. *TAS-GNN*. https://arxiv.org/abs/2603.13290 (chỉ đọc abstract)
12. PyTorch Geometric Temporal: https://github.com/benedekrozemberczki/pytorch_geometric_temporal (thư mục `nn/recurrent`, `nn/attention`, `signal`, `dataset`); docs: https://pytorch-geometric-temporal.readthedocs.io
13. https://github.com/benedekrozemberczki/SGCN
14. https://github.com/huangjunjie-cs/SiGAT (`sdgnn.py`)
15. https://github.com/Jieerbobo/TrustGuard
16. https://github.com/pyg-team/pytorch_geometric/blob/master/torch_geometric/nn/conv/message_passing.py (mặc định `flow="source_to_target"`)
17. Zheng et al. *AddGraph: Anomaly Detection in Dynamic Graph Using Attention-based Temporal GCN*. IJCAI 2019 (đọc bản PDF local, Eq. 1–10)
