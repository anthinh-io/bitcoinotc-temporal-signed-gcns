# Giao thức đánh giá, số đã công bố và code sẵn có trên Bitcoin-OTC

Ticket: #4. Ngày tra cứu: 2026-11-25. Đây chỉ là ghi chú nghiên cứu. Tài liệu không đưa ra quyết định.

**Quy ước nhãn**
- **[V]** (verified): đã đọc trực tiếp trong PDF của paper hoặc trong source code, có trích vị trí.
- **[I]** (inferred): suy luận từ các bằng chứng [V]. Chưa có nguồn nào nói thẳng điều này.

---

## 0. Tóm tắt

1. **Trên Bitcoin-OTC chỉ có một "giao thức chuẩn" thật sự: giao thức TADDY.** Nó kế thừa NetWalk và gồm: tiêm cạnh giả vào nửa sau của dòng cạnh, snapshot có số cạnh cố định, và metric là AUC. TSAVGA dùng lại giao thức này và chép nguyên số baseline của TADDY. STCAD cũng tự nhận theo giao thức này, nhưng code của STCAD làm khác hẳn, nên số của STCAD **không so được** với số của TADDY.
2. **Paper và code của TADDY không khớp nhau** ở ba điểm: kích thước snapshot của OTC, cách dedup, và cách tiêm anomaly. Ngoài ra pipeline TADDY **bỏ hẳn dấu/trọng số rating**, nên mọi AUC "Bitcoin-OTC" đã công bố đều là bài toán trên đồ thị *không dấu* với anomaly *tổng hợp*.
3. **Số baseline có nguồn gốc lẫn lộn.**
   - Hàng NetWalk, StrGNN và các embedding baseline của TADDY trùng khớp với StrGNN arXiv v2.
   - Hàng AddGraph trên Bitcoin không truy được nguồn trước TADDY.
   - Paper AddGraph gốc không có Bitcoin.
4. **EvolveGCN trên BC-OTC là bài toán khác hẳn.** Đó là phân loại dấu cạnh (edge classification) và link prediction, có nhãn thật là dấu rating. Đây **không phải** phát hiện anomaly bằng tiêm cạnh giả.
5. **Code có sẵn**
   - TADDY (MIT) và STCAD (Apache-2.0) đều có code chạy Bitcoin-OTC.
   - AddGraph chỉ có code không chính chủ, và chỉ cho UCI/Digg.
   - Repo StrGNN không kèm bước tiền xử lý Bitcoin.
   - `github.com/IBM/EvolveGCN` hiện trả về **404**, nhưng có fork Apache-2.0.
   - `github.com/iyadnekka/x-addgraph` trả về **404**.

---

## 1. Dữ liệu gốc Bitcoin-OTC (SNAP) — số liệu tự đếm [V]

Nguồn: `https://snap.stanford.edu/data/soc-sign-bitcoinotc.csv.gz`, tải về ngày 2026-11-25. Các số dưới đây đếm bằng `awk`.

| Đại lượng | Giá trị |
|---|---|
| Số dòng (cạnh có hướng, có timestamp) | 35,592 |
| Số cặp có hướng (u→v) khác nhau | 35,592: **không có cạnh có hướng nào lặp lại** |
| Self-loop | 0 |
| Số cặp vô hướng {u,v} khác nhau | **21,492**: "repeated edges" trên OTC thực chất là các rating qua lại A→B và B→A |
| Số node | 5,881 |
| Rating < 0 | 3,563 (≈10.0%) |
| Rating ≤ −5 | 2,662 |

Các paper ghi số cạnh của OTC khác nhau:
- TADDY Table 2 và EvolveGCN Table 1 ghi 35,588.
- STCAD Table 1 và TSAVGA Table 2 ghi 35,592.

Không paper nào ghi 21,492. Đây lại là số cạnh mà code TADDY thực sự dùng (xem §2.1). [I]

---

## 2. Giao thức theo từng paper

### 2.1 Bảng giao thức

| Paper | Snapshot | Train/test split | Dedup cạnh lặp | Anomaly ở test | Train / scoring | Metric |
|---|---|---|---|---|---|---|
| **TADDY** (arXiv 2106.09876v2, TKDE 2021): theo paper | Số cạnh cố định. **1,000 cạnh cho Bitcoin-OTC** và 2,000 cho Bitcoin-Alpha (§5.1.4, tr. 9) | "50% timestamps đầu là train, 50% sau là test" (§5.1.3) | "Repeated edges … are removed" (§5.1.1) | Mỗi test snapshot nối ngẫu nhiên pA·m_t cặp node chưa nối, pA ∈ {1%, 5%, 10%}, train sạch hoàn toàn (§5.1.1, theo NetWalk) | Negative sampling (cặp chưa từng xuất hiện trong train) với BCE, end-to-end | ROC-AUC trung bình trên các test timestamp (Table 3) |
| **TADDY**: theo code `yuetan031/TADDY_pytorch` [V] | `snap_size_dict = {'btc_alpha':1000, 'btc_otc':2000}` (`0_prepare_data.py`). **Ngược với paper.** Số snapshot = `int(len/snap+0.5)` | 50% **cạnh** đầu (sắp theo timestamp), không phải 50% timestamp | Chuẩn hóa về vô hướng (min, max), bỏ self-loop, giữ lần xuất hiện đầu bằng `np.unique`. Chỉ giữ cột 0:2, **bỏ rating**. OTC còn 21,492 cạnh [V, đếm lại] | `anomaly_generation` (`codes/AnomalyGeneration.py`): lấy floor(p·\|test\|) cặp ngẫu nhiên **nằm ở hai cụm spectral khác nhau (k=42)**, không có trong toàn đồ thị, seed=1. Chèn vào vị trí ngẫu nhiên trong *cả dòng test*, rồi mới cắt snapshot. Do đó tỷ lệ theo từng snapshot chỉ xấp xỉ pA | Như paper | `DynADModel.evaluate` in AUC từng snapshot và "TOTAL AUC" gộp. Đánh giá trên test mỗi `print_feq` epoch, không có tập validation [V] |
| **TADDY**: hệ quả trên OTC [I] | Với 21,492 cạnh và snap=2000 (code): khoảng 5 snapshot train và 5–6 snapshot test. Với snap=1000 (paper): khoảng 11 mỗi bên | | | | | |
| **StrGNN** (arXiv 2005.07427v2; CIKM 2021) | Paper v2 không ghi kích thước snapshot. Repo nhận ma trận (T, N, N) do người dùng tự dựng | 50% cạnh đầu là train (§4.3) | Không nói | Tiêm 1/5/10% theo NetWalk (§4.3) | Đồ thị con enclosing h-hop (h=1, w=5), GCN, GRU, BCE | AUC (Table 2) |
| **STCAD** (arXiv 2505.08330v1): theo paper | Số cạnh cố định. **2,000 cho Bitcoin-OTC** và Bitcoin-Alpha (§5.2.2) | "first half of the sequences" là train (§5.2.2) | Không dedup: "edges with the same nodes but different timestamps are regarded as different instances" (Table 1 caption) | Tiêm 1/5/10% "follow Yu et al. (2018)" | Train: cạnh của snapshot cuối là normal, kèm cùng số cạnh chưa từng xuất hiện làm anomaly. Chọn **kết quả tốt nhất trong 300 epoch** | AUC và AP (Table 2) |
| **STCAD**: theo code `changzong/STCAD` [V] | `split_graph_by_size`: 2,000 cạnh mỗi snapshot, dựng bằng `nx.MultiGraph` vô hướng. Snapshot dở dang cuối bị bỏ, và cạnh nằm ở ranh giới mỗi snapshot bị nhảy qua | `snapshots[:4]` làm train, `snapshots[-4:]` làm test (`time_window_len=4`). **Cạnh mục tiêu chỉ lấy từ snapshot cuối** của mỗi cửa sổ (`sampling_strategy`) | Không dedup (MultiGraph) | Anomaly = `negative_node_pair_sampling`: cặp node chưa có cạnh trong *4 snapshot của cửa sổ*, số lượng bằng ratio × số cạnh normal. **Không phải** cách tiêm của NetWalk/TADDY | `main.py` giữ `best_auc` = **AUC lớn nhất trên test** qua các epoch, tức là chọn mô hình trên test | AUC, AP |
| **TSAVGA** (Research Square rs-7806501/v1, preprint 2026) | Số cạnh cố định. **1,000 cho BTC-OTC** và 2,000 cho BTC-Alpha (§5.1.4). Riêng §5.4.4 lại gọi là "số node mỗi snapshot", mâu thuẫn với chỗ trên | 50% cạnh đầu theo thứ tự thời gian (§5.1.3) | Gộp các tương tác lặp của cùng một cặp, timestamp đại diện = trung bình (§4.1) | Test: "same anomaly injection strategy as … [13,14]" (TADDY, FALCON), 1/5/10%. Train: tiêm âm 1:1 giữa các cụm spectral | VGAE + GCN-GRU, BCE | AUC (Table 3) |
| **AddGraph** (IJCAI 2019) | 1,000 cạnh (UCI), 6,000 (Digg) | 50% đầu là train | UCI chỉ còn 13,838 cạnh (Table 2), tức là đã dedup [I] | Tiêm 1/5/10% theo NetWalk | Selective negative sampling và margin loss | AUC. **Không có thí nghiệm Bitcoin** [V] |
| **X-AddGraph** (arXiv 2608.12441v1) | 5,300 cạnh (UCI), "theo đúng giao thức gốc". Trong khi đó paper AddGraph ghi 1,000 | 50% đầu là train, tiêm 5% | — | — | Giải thích post-hoc cho AddGraph | AUC trên UCI (0.8705). **Không có Bitcoin** [V] |

### 2.2 EvolveGCN trên BC-OTC: bài toán khác, KHÔNG phải anomaly injection

Nguồn: arXiv 1902.10191v3 (AAAI 2020) và config trong repo (đọc qua fork, xem §4).

| Mục | Nội dung |
|---|---|
| Nhiệm vụ | (a) **Edge classification**: dự đoán *dấu* rating của cạnh (u,v) tại bước t. (b) **Link prediction**: dự đoán cạnh có xuất hiện ở t+1 không (§4.2). **Không có cạnh tiêm, không có nhãn anomaly.** [V] |
| Nhãn | `bitcoin_dl.py`: rating > 0 cho nhãn dương, rating ≤ 0 cho nhãn âm. Đồ thị được làm vô hướng bằng cách thêm cạnh ngược, rồi **bỏ phiếu đa số** trên mỗi bộ (u, v, step). [V, code] |
| Snapshot | Theo **cửa sổ thời gian**, không theo số cạnh: `aggr_time: 1200000` giây ≈ 13.9 ngày. Comment trong config ghi "three weeks", không khớp với giá trị. [V] |
| Split | Theo thời gian: train 0.7, dev 0.1, test 0.2 tính theo số bước, ra **95 / 14 / 28 bước** (Table 1). [V] |
| Đặc trưng, cửa sổ | One-hot node degree. `num_hist_steps: 10`. [V] |
| Metric | Edge classification: precision, recall, F1 (micro). **Số chỉ có trong biểu đồ cột Fig. 3, không có bảng số.** Ở đây không ước lượng số từ hình. Link prediction trên BC-OTC (Table 2): MAP 0.0026 (EGCN-H) / 0.0028 (EGCN-O), MRR 0.0690 / 0.0968. [V] |

Nhận xét: nhãn "dấu rating" của EvolveGCN có cùng bản chất với nhãn "anomaly thật" của benchmark 2026 (rating ≤ −5, xem §3.3). Cả hai đều *khác* với nhãn tiêm tổng hợp ở §3.1–3.2. [I]

---

## 3. Các số AUC đã công bố trên Bitcoin-OTC

### 3.1 Giao thức TADDY (anomaly tiêm, đồ thị không dấu)

Nguồn A: TADDY Table 3 (tr. 10). Nguồn B: StrGNN arXiv v2 Table 2. Nguồn C: TSAVGA Table 3 (tr. 13). Tất cả đã đối chiếu bằng `pdftotext -layout`. [V]

| Method | 1% | 5% | 10% | Nguồn và ghi chú |
|---|---|---|---|---|
| node2vec | 0.6951 | 0.6883 | 0.6745 | A = B |
| Spectral Clustering | 0.7624 | 0.7376 | 0.7047 | A = B |
| DeepWalk | 0.7423 | 0.7356 | 0.7287 | A = B |
| NetWalk | 0.7785 | 0.7694 | 0.7534 | A = B = C |
| AddGraph | 0.8352 | 0.8455 | 0.8592 | A = C. **Không có trong B**: StrGNN v2 không có hàng AddGraph. Chưa tìm được nguồn trước TADDY [I] |
| StrGNN | 0.9012 | 0.8775 | 0.8836 | A = B = C |
| TADDY | 0.9455 | 0.9340 | 0.9425 | A = C |
| RegraphGAN | 0.9195 | 0.9454 | 0.9564 | Chỉ có ở C (thứ cấp) |
| GraphLSTA | 0.9217 | 0.9253 | 0.9227 | Chỉ có ở C (thứ cấp) |
| FALCON | 0.9547 | 0.9639 | 0.9640 | Chỉ có ở C (thứ cấp) |
| **TSAVGA** | **0.9747** | **0.9732** | **0.9737** | C |

Về nguồn gốc số:
- Mọi baseline trong bảng TADDY đều trùng tới 4 chữ số thập phân với StrGNN v2. Riêng AddGraph không có trong StrGNN v2. Như vậy TADDY rất có thể **chép lại** số baseline thay vì chạy lại. [I]
- Bằng chứng tương tự ở UCI: hàng NetWalk và AddGraph trên UCI/Digg của TADDY trùng với AddGraph Table 1. Bảng đó là thiết lập "graphs **without timestamps**". [V]
- TSAVGA chép nguyên bốn hàng NetWalk, AddGraph, StrGNN, TADDY từ TADDY. [V]

### 3.2 Giao thức STCAD (khác giao thức TADDY, xem §2.1)

Nguồn: STCAD Table 2 (tr. 13), cột Bitcoin-OTC. [V]

| Method | AUC 1% | AUC 5% | AUC 10% | AP 1% | AP 5% | AP 10% |
|---|---|---|---|---|---|---|
| STFeature | 0.9588 | 0.9468 | 0.9465 | 0.6551 | 0.7015 | 0.7970 |
| CMFeature | 0.9433 | 0.9309 | 0.9302 | 0.6378 | 0.6749 | 0.7739 |
| AddGraph | 0.9825 | 0.9736 | 0.9629 | 0.8033 | 0.8898 | 0.8917 |
| StrGNN | 0.9809 | 0.9704 | 0.9701 | 0.7868 | 0.8766 | 0.9152 |
| TADDY | 0.9797 | 0.9608 | 0.9628 | 0.7041 | 0.7769 | 0.8595 |
| **STCAD** | **0.9919** | **0.9897** | **0.9861** | **0.8862** | **0.9206** | **0.9501** |

### 3.3 Cùng một method, số khác nhau giữa các paper (Bitcoin-OTC)

| Method | TADDY / TSAVGA (1/5/10%) | STCAD (1/5/10%) | Chênh lệch |
|---|---|---|---|
| AddGraph | 0.8352 / 0.8455 / 0.8592 | 0.9825 / 0.9736 / 0.9629 | +0.10 đến +0.15 |
| StrGNN | 0.9012 / 0.8775 / 0.8836 | 0.9809 / 0.9704 / 0.9701 | khoảng +0.08 |
| TADDY | 0.9455 / 0.9340 / 0.9425 | 0.9797 / 0.9608 / 0.9628 | khoảng +0.02 đến +0.03 |

Có thể giải thích chênh lệch bằng các khác biệt sau [I]:
- STCAD dùng snapshot 2,000 cạnh, không dedup.
- STCAD chỉ đánh giá trên snapshot test cuối cùng.
- Anomaly của STCAD là "cặp chưa nối trong cửa sổ 4 snapshot", không lọc theo cụm.
- STCAD chọn epoch tốt nhất ngay trên test.

Thêm vào đó, trong STCAD, TADDY *không* còn đứng đầu nhóm baseline trên OTC. Bản thân paper STCAD cũng ghi nhận điều này (§5.3).

**Nhãn thật (không phải tiêm), thứ cấp, chưa qua bình duyệt.** Nguồn là repo benchmark `Iyad-Assaad-Nekka/Deep-Learning-for-Anomaly-Detection-in-Dynamic-Graphs-A-Verified-Survey-Taxonomy-and-benchmarking`, file `results/comparison.csv`, tạo 2026-08. Giao thức: split 80/20 theo thời gian, nhãn anomaly = rating ≤ −5. Kết quả AUC trên OTC:

| Method | AUC |
|---|---|
| TADDY | 0.5343 |
| SLADE | 0.7572 |
| Deg-Sum (heuristic) | 0.4143 |
| Random | 0.4979 |

Repo không có số cho AddGraph hay StrGNN trên OTC. [V: số trong repo; chất lượng chưa kiểm]

---

## 4. Code có sẵn

Metadata lấy bằng `gh api` ngày 2026-11-25.

| Method | Repo | Chính chủ? | License | Commit cuối | Framework | Hỗ trợ Bitcoin-OTC? | Đánh giá chạy được |
|---|---|---|---|---|---|---|---|
| TADDY | `github.com/yuetan031/TADDY_pytorch`. Bản `yixinliu233/TADDY_pytorch` là fork của repo này | Có (README nói "reference implementation" và dẫn paper) | MIT | 2021-11-17 | Python 3.8, PyTorch 1.7.1, **transformers 3.5.1**, scipy 1.5.2, networkx 2.5, sklearn 0.23.2 (README) | **Có**: `--dataset btc_otc`, kèm sẵn `data/raw/soc-sign-bitcoinotc.csv` | Có, nhưng cần ghim phiên bản. Benchmark 2026 (§3.3) phải ghim `transformers==4.44.2` và vá do API thay đổi (`docs/PATCHES.md`) [V: tài liệu repo đó] |
| STCAD | `github.com/changzong/STCAD` (link trong paper, mục "Availability of data and code") | Có | Apache-2.0 | 2023-07-13 | PyTorch, networkx, sklearn. **Không có file requirements, không ghim phiên bản** | **Có**: `--dataset btc-otc`, có `dataset/raw/btc-otc` | Có thể chạy được [I]. Snapshot size và cửa sổ train/test bị hardcode (§2.1) |
| AddGraph | Không tìm thấy repo của nhóm tác giả. `github.com/Ljiajie/Addgraph`: README ghi "code for reproducing key results" của paper | **Không rõ.** Tên tài khoản không trùng tác giả. Benchmark 2026 xếp vào loại "official repository, patched" | Không có license | 2021-09-29 | PyTorch (import `torch`). Có `.pyc` của CPython 3.6, không có requirements | **Không**: chỉ có UCI và Digg (thư mục `UCI_U_…`, `Digg_D_…`) | Phải tự port sang Bitcoin. Benchmark 2026 ghi các lỗi: lệch window train/test (w=1 và w=5), BPTT không detach, epoch mặc định che plateau |
| AddGraph (X-AddGraph) | `github.com/iyadnekka/x-addgraph` (footnote 3 trong paper) | — | — | — | — | — | **Trả về 404** (cả API lẫn web, 2026-11-25). Wayback không có bản lưu. Tài khoản `Iyad-Assaad-Nekka` có các repo X-TADDY, X-StrGNN, X-SAD, benchmark, nhưng không có x-addgraph |
| StrGNN | `github.com/LeiCaiwsu/StrGNN` (gốc) và `github.com/KnowledgeDiscovery/StrGNN` (fork) | Có | Gốc: không có license. Fork: NOASSERTION (có `LICENSE.txt`) | Gốc: 2022-06-23. Fork: 2024-06-08 | PyTorch. Cần **build thư viện C (`pytorch_DGCNN/lib`, `make`)**. `install.sh` không ghim phiên bản | **Không có sẵn**: chỉ có ví dụ email. Người dùng tự tạo `acc_*.npy` (T, N, N) và các split | Khó chạy. Benchmark 2026 ghi các vấn đề: import hỏng, nhãn bị đảo, rò rỉ target link vào subgraph, AP/F1 tính trên lớp *normal* |
| NetWalk | `github.com/chengw07/NetWalk` | Có | Không có license | 2019-07-10 | TensorFlow 1.x | Không trực tiếp. Script tiêm anomaly mà TADDY tái sử dụng đến từ đây | Cần shim TF1 |
| EvolveGCN | `github.com/IBM/EvolveGCN` (link trong abstract của paper) | Có | Apache-2.0 (theo các fork) | Commit upstream cuối 2021-06-02, đọc từ fork `AleMurru/EvolveGCN` | Python 3.6, PyTorch ≥ 1.0 (README). Có Dockerfile | **Có**: `experiments/parameters_bitcoin_otc_edgecls_egcn_{h,o}.yaml` và `…_linkpred_…` | **Repo gốc hiện trả về 404.** Wayback có bản lưu ngày 2026-06-24. Code còn nguyên trong các fork Apache-2.0, ví dụ `gravlaks/EvolveGCN` (parent = IBM/EvolveGCN) và `AleMurru/EvolveGCN`. Có bản DGL không chính chủ `maqy1995/EvolveGCN-DGL` (không license, 2021-09) |
| TSAVGA | — | — | — | — | — | — | Không có code: "Data will be made available on request" |

---

## 5. Hàm ý: cần gì để so với số đã công bố

1. **Muốn so với cột TADDY/TSAVGA (§3.1)** thì phải dùng đúng pipeline `0_prepare_data.py` của TADDY:
   - dedup vô hướng (21,492 cạnh);
   - 50% cạnh đầu làm train;
   - tiêm cạnh giữa các cụm spectral (k=42) với seed=1;
   - AUC trung bình theo snapshot.

   Ngoài ra phải **ghi rõ đã chọn snapshot size nào**: 1,000 theo paper hay 2,000 theo code. Hai lựa chọn này cho số lượng snapshot khác nhau khoảng 2 lần, điều rất quan trọng với GRU và attention. Chỉ cần đổi seed hoặc cách tiêm là đã ra một giao thức khác.
2. **Không so trực tiếp với số STCAD.** Giao thức trong code của STCAD khác (§2.1), và việc chọn mô hình trên test làm số bị đẩy lên. Nếu muốn so thì phải chạy lại STCAD trong giao thức của mình.
3. **Hàng AddGraph trên Bitcoin-OTC (0.8352/0.8455/0.8592) không có code gốc làm chuẩn.** Code AddGraph công khai không hỗ trợ Bitcoin. Muốn có baseline AddGraph thì gần như chắc chắn phải tự port và tự chạy lại, kể cả khi bám theo cấu trúc repo không chính chủ. Khi báo cáo, nên ghi cả số tự chạy lẫn số đã công bố.
4. **Dấu rating bị bỏ trong mọi số anomaly đã công bố.** Một mô hình *signed* (GCN có dấu) chạy trên giao thức TADDY không được dùng thông tin dấu, vì cạnh giả không có dấu. Nếu luận văn muốn khai thác dấu thì cần một trong hai hướng:
   - quy ước dấu cho cạnh giả;
   - dùng regime nhãn thật: rating ≤ −5 (2,662 cạnh) hoặc rating < 0 (3,563 cạnh).

   Hướng thứ hai gần với bài toán edge classification của EvolveGCN hơn, nhưng **gần như chưa có baseline được bình duyệt**. Nguồn duy nhất tìm được là repo benchmark 2026 ở §3.3, chưa qua bình duyệt. Ở đó TADDY chỉ đạt khoảng 0.53 AUC.
5. **Nguồn số đáng tin nhất** là các số do chính tác giả chạy:
   - TADDY: hàng TADDY;
   - StrGNN v2: hàng StrGNN và các embedding baseline;
   - TSAVGA: hàng TSAVGA.

   Các hàng baseline trong TADDY và TSAVGA là số chép lại. RegraphGAN, GraphLSTA, FALCON chỉ thấy qua TSAVGA, một preprint chưa có code. Ticket này chưa kiểm số của ba method đó trong paper gốc.
6. **Việc còn mở**
   - Kiểm bản StrGNN CIKM'21 xem có hàng AddGraph trên Bitcoin không.
   - Đọc giá trị F1 trong Fig. 3 của EvolveGCN bằng mắt từ PDF.
   - Chạy thử TADDY với cả snapshot 1,000 và 2,000 để xem có tái lập được 0.9455/0.9340/0.9425 không.

---

## 6. Nguồn

**Paper (nguồn sơ cấp)**
- TADDY: Y. Liu et al., "Anomaly Detection in Dynamic Graphs via Transformer", IEEE TKDE 2021, doi:10.1109/TKDE.2021.3124061; arXiv:2106.09876v2. Dùng §5.1 (tr. 8–9), Table 2, Table 3 (tr. 10).
- StrGNN: L. Cai et al., "Structural Temporal Graph Neural Networks for Anomaly Detection in Dynamic Graphs", arXiv:2005.07427v2 (CIKM 2021). Dùng §4.3 và Table 2. https://arxiv.org/abs/2005.07427
- STCAD: C. Zong et al., "Structural-Temporal Coupling Anomaly Detection with Dynamic Graph Transformer", arXiv:2505.08330v1. Dùng §5.1–5.2, Table 1, Table 2 (tr. 13), mục Availability.
- TSAVGA: "Dynamic Graph Anomaly Detection via Temporal-Structural Attention and Variational Graph Autoencoder", Research Square, doi:10.21203/rs.3.rs-7806501/v1. Dùng §4.1, §5.1, Table 3 (tr. 13).
- AddGraph: L. Zheng et al., IJCAI 2019, tr. 4419–4425. Dùng §4.1, Table 1, Table 2.
- X-AddGraph: arXiv:2608.12441v1. Dùng §4 (thiết lập trên UCI) và footnote 3.
- EvolveGCN: A. Pareja et al., AAAI 2020, arXiv:1902.10191v3. Dùng §4.1–4.6, Table 1, Table 2, Fig. 3.
- Dữ liệu: https://snap.stanford.edu/data/soc-sign-bitcoin-otc.html

**Code**
- https://github.com/yuetan031/TADDY_pytorch: các file `0_prepare_data.py`, `codes/AnomalyGeneration.py`, `codes/DynADModel.py`, `README.md`
- https://github.com/changzong/STCAD: các file `main.py`, `sampling.py`
- https://github.com/Ljiajie/Addgraph
- https://github.com/LeiCaiwsu/StrGNN và https://github.com/KnowledgeDiscovery/StrGNN
- https://github.com/chengw07/NetWalk
- https://github.com/IBM/EvolveGCN (404 ngày 2026-11-25). Bản lưu: http://web.archive.org/web/20260624105312/https://github.com/IBM/EvolveGCN. Fork: https://github.com/gravlaks/EvolveGCN và https://github.com/AleMurru/EvolveGCN (các file `bitcoin_dl.py`, `experiments/parameters_bitcoin_otc_edgecls_egcn_o.yaml`)
- https://github.com/iyadnekka/x-addgraph (404)
- Benchmark thứ cấp: https://github.com/Iyad-Assaad-Nekka/Deep-Learning-for-Anomaly-Detection-in-Dynamic-Graphs-A-Verified-Survey-Taxonomy-and-benchmarking (các file `README.md`, `docs/PROTOCOL.md`, `docs/PATCHES.md`, `results/comparison.csv`)
