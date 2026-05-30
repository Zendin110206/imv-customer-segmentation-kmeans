# Dokumentasi Kodingan Proyek Machine Learning

## Segmentasi Pelanggan Berdasarkan Pola Belanja Menggunakan K-Means Clustering

### Identitas

| Informasi | Keterangan |
| --- | --- |
| Nama | Muhammad Zaenal Abidin Abdurrahman |
| NIM | 101012300153 |
| Kegiatan | Tugas Individu Machine Learning - Open Recruitment IMV Laboratory 2026 |
| Jenis Machine Learning | Unsupervised Learning |
| Metode | K-Means Clustering |
| Repository | <https://github.com/Zendin110206/imv-customer-segmentation-kmeans> |

## 1. Deskripsi Proyek

Proyek ini bertujuan untuk melakukan segmentasi pelanggan berdasarkan pola belanja pada data e-commerce. Segmentasi dilakukan menggunakan algoritma K-Means Clustering, yaitu metode unsupervised learning yang mengelompokkan data berdasarkan kemiripan karakteristik tanpa memakai label target.

Dataset yang digunakan adalah Olist Brazilian E-Commerce Public Dataset dari Kaggle. Dataset ini berisi data transaksi e-commerce yang sudah dianonimkan, seperti data pelanggan, pesanan, item pesanan, pembayaran, review, dan produk. Karena bentuk datasetnya relational, informasi yang dibutuhkan tidak berada dalam satu file saja. Data perlu dibaca, dicek, digabung, dan diubah menjadi dataset tingkat pelanggan sebelum dapat digunakan untuk clustering.

Pada proyek ini, setiap baris data akhir merepresentasikan satu pelanggan unik berdasarkan `customer_unique_id`. Dari beberapa tabel transaksi, dibuat fitur perilaku pelanggan seperti jumlah pesanan, total pembayaran, penggunaan cicilan, jumlah produk yang dibeli, beban ongkir, skor review, durasi pengiriman, dan keterlambatan pengiriman. Fitur-fitur tersebut kemudian diproses agar siap digunakan oleh K-Means.

Hasil akhir proyek adalah empat segmen pelanggan yang dapat dijelaskan secara bisnis:

1. Low-value one-time customers with high freight burden.
2. Higher-value installment one-time customers.
3. Delayed and dissatisfied customers.
4. Repeat and broader-basket customers.

## 2. Tujuan Proyek

Tujuan utama proyek ini adalah:

1. Memahami struktur dataset e-commerce Olist yang terdiri dari banyak file CSV.
2. Melakukan audit kualitas data, termasuk jumlah baris, kolom, missing value, duplikasi, dan relasi antar tabel.
3. Mengubah data transaksi menjadi customer-level dataset menggunakan `customer_unique_id`.
4. Membuat fitur perilaku pelanggan yang relevan untuk segmentasi.
5. Melakukan preprocessing data agar fitur numerik aman digunakan oleh algoritma berbasis jarak.
6. Mencoba beberapa jumlah cluster menggunakan K-Means.
7. Memilih jumlah cluster akhir berdasarkan metrik clustering, keseimbangan ukuran cluster, dan interpretasi bisnis.
8. Menjelaskan karakteristik setiap cluster dengan bahasa yang mudah dipahami.
9. Memberikan rekomendasi bisnis sederhana berdasarkan hasil segmentasi.

## 3. Dataset

Dataset yang digunakan:

| Informasi | Keterangan |
| --- | --- |
| Nama Dataset | Olist Brazilian E-Commerce Public Dataset |
| Sumber | Kaggle |
| Link Dataset | <https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce> |
| Jenis Data | Data transaksi e-commerce |
| Jenis Task | Customer Segmentation |
| Metode | K-Means Clustering |

Dataset Olist berisi transaksi e-commerce di Brazil yang sudah dianonimkan. Dataset ini cocok untuk customer segmentation karena tidak hanya berisi nilai transaksi, tetapi juga informasi pembayaran, review, pengiriman, produk, dan pelanggan.

File utama yang digunakan dalam proyek:

| File | Fungsi dalam Proyek |
| --- | --- |
| `olist_customers_dataset.csv` | Menyediakan `customer_id`, `customer_unique_id`, kota, state, dan zip code prefix pelanggan. File ini penting karena `customer_id` berada pada level order, sedangkan `customer_unique_id` dipakai untuk menggabungkan riwayat pelanggan yang sama. |
| `olist_orders_dataset.csv` | Menyediakan `order_id`, `customer_id`, status pesanan, tanggal pembelian, tanggal approval, tanggal dikirim ke carrier, tanggal diterima pelanggan, dan estimasi tanggal pengiriman. File ini dipakai untuk menghitung recency, durasi pengiriman, dan keterlambatan. |
| `olist_order_items_dataset.csv` | Menyediakan item dalam pesanan, harga produk, ongkir (`freight_value`), `product_id`, dan `seller_id`. File ini dipakai untuk menghitung jumlah item, jumlah produk, nilai produk, dan beban ongkir. |
| `olist_order_payments_dataset.csv` | Menyediakan metode pembayaran, jumlah cicilan, dan nilai pembayaran. File ini dipakai untuk membaca pola pembayaran dan total nilai transaksi pelanggan. |
| `olist_order_reviews_dataset.csv` | Menyediakan review score dan timestamp review. File ini dipakai sebagai sinyal kepuasan pelanggan. |
| `olist_products_dataset.csv` | Menyediakan kategori produk dan metadata produk. File ini dipakai untuk membantu fitur variasi produk dan konteks produk. |

File yang tidak digunakan langsung pada model pertama:

| File | Alasan |
| --- | --- |
| `olist_geolocation_dataset.csv` | Tidak dipakai pada model pertama karena fokus proyek ini adalah pola belanja pelanggan, bukan analisis geospasial. File ini juga memiliki banyak duplikasi zip code prefix, sehingga perlu strategi agregasi khusus jika nanti digunakan. |
| `olist_sellers_dataset.csv` | Tidak dipakai karena fokusnya seller-side, sedangkan proyek ini menargetkan customer segmentation. |
| `product_category_name_translation.csv` | Tidak dipakai langsung dalam feature matrix karena model saat ini lebih menekankan jumlah/variasi produk, bukan analisis kategori produk berbahasa Inggris. |

Keputusan tidak menggunakan semua file bukan berarti file tersebut tidak berguna. Keputusan ini dibuat agar model pertama tetap fokus, dapat dijelaskan, dan sesuai dengan deadline tugas IMV. File geolocation, sellers, dan category translation dapat menjadi pengembangan lanjutan setelah deliverable utama selesai.

## 4. Exploratory Data Analysis dan Data Quality Audit

Tahap pertama adalah membaca seluruh file mentah dan memahami kualitas datanya. Hal yang dicek:

- jumlah baris dan kolom setiap CSV,
- daftar kolom,
- tipe data awal,
- missing value,
- duplikasi baris,
- key uniqueness,
- hubungan antar tabel,
- status pesanan,
- rentang tanggal transaksi,
- kondisi payment,
- kondisi review,
- harga item dan freight value.

Ringkasan jumlah data mentah:

| Table | Rows | Columns | Duplicate Rows | Main Role |
| --- | ---: | ---: | ---: | --- |
| customers | 99,441 | 5 | 0 | Customer identity and location. |
| orders | 99,441 | 8 | 0 | Order status and timestamps. |
| order_items | 112,650 | 7 | 0 | Item-level product, seller, price, and freight. |
| order_payments | 103,886 | 5 | 0 | Payment method, installment, and payment value. |
| order_reviews | 99,224 | 7 | 0 | Review score and review timestamps. |
| products | 32,951 | 9 | 0 | Product category and attributes. |
| category_translation | 71 | 2 | 0 | Product category translation. |
| geolocation | 1,000,163 | 5 | 261,831 | Optional geographic reference. |
| sellers | 3,095 | 4 | 0 | Optional seller reference. |

Relasi penting yang digunakan:

```text
customers.customer_id -> orders.customer_id
orders.order_id -> order_items.order_id
orders.order_id -> order_payments.order_id
orders.order_id -> order_reviews.order_id
order_items.product_id -> products.product_id
```

Mayoritas pesanan pada dataset berstatus `delivered`. Karena proyek ini membutuhkan fitur pengalaman pengiriman seperti delivery days dan late delivery rate, model menggunakan pesanan yang sudah `delivered`. Pesanan yang belum selesai tidak digunakan pada feature engineering utama karena informasi pengirimannya belum lengkap atau tidak merepresentasikan pengalaman pembelian yang selesai.

Status pesanan utama:

| Status | Count |
| --- | ---: |
| delivered | 96,478 |
| shipped | 1,107 |
| canceled | 625 |
| unavailable | 609 |
| invoiced | 314 |
| processing | 301 |
| created | 5 |
| approved | 2 |

Temuan penting dari audit:

- `customer_unique_id` tidak unik pada tabel customers karena satu pelanggan bisa memiliki lebih dari satu order.
- `order_id` unik pada tabel orders.
- Review text banyak yang kosong, tetapi ini tidak menjadi masalah utama karena proyek memakai `review_score`, bukan analisis teks.
- Beberapa timestamp pengiriman kosong terutama pada order yang tidak delivered.
- Payment dan price memiliki distribusi yang cenderung skewed, sehingga perlu diperhatikan sebelum K-Means.
- Geolocation memiliki banyak duplicate rows, sehingga sengaja tidak dipakai pada model pertama.

## 5. Feature Engineering

Feature engineering adalah tahap mengubah data transaksi menjadi data tingkat pelanggan. Karena tujuan proyek adalah customer segmentation, model tidak boleh bekerja pada level order atau item. Model harus bekerja pada level pelanggan unik.

Alur sederhananya:

```text
Raw relational CSV
-> filter delivered orders
-> join orders dengan customers, payments, reviews, items, products
-> agregasi per customer_unique_id
-> customer-level feature table
```

Contoh pola kode yang digunakan:

```python
delivered_orders = orders.loc[orders["order_status"] == "delivered"].copy()

delivered_orders["order_purchase_timestamp"] = pd.to_datetime(
    delivered_orders["order_purchase_timestamp"],
    errors="coerce"
)

customer_features = (
    order_level_features
    .groupby("customer_unique_id")
    .agg(
        order_count=("order_id", "nunique"),
        total_payment_value=("payment_value", "sum"),
        avg_review_score=("review_score", "mean"),
        avg_delivery_days=("delivery_days", "mean"),
        late_delivery_rate=("is_late_delivery", "mean"),
    )
    .reset_index()
)
```

Kode di atas hanya contoh ringkas dari proses utama. Di notebook, agregasi dibuat lebih lengkap dan dilakukan bertahap agar hasilnya bisa dicek.

Fitur utama yang dibuat:

| Fitur | Makna |
| --- | --- |
| `recency_days` | Jarak hari dari transaksi terakhir pelanggan terhadap tanggal referensi dataset. |
| `order_count` | Jumlah pesanan selesai yang dimiliki pelanggan. |
| `total_payment_value` | Total nilai pembayaran pelanggan dari pesanan yang selesai. |
| `avg_payment_value` | Rata-rata nilai pembayaran per order. |
| `avg_payment_installments` | Rata-rata jumlah cicilan pembayaran. |
| `avg_payment_method_count` | Rata-rata jumlah metode pembayaran dalam order. |
| `total_items` | Total item yang dibeli. |
| `avg_items_per_order` | Rata-rata item per order. |
| `total_product_count` | Total produk yang dibeli pelanggan. |
| `unique_product_count` | Jumlah produk unik yang pernah dibeli pelanggan. |
| `freight_to_payment_ratio` | Perbandingan ongkir terhadap nilai pembayaran. |
| `avg_review_score` | Rata-rata skor review pelanggan. |
| `review_count` | Jumlah review yang tersedia. |
| `avg_delivery_days` | Rata-rata durasi pengiriman. |
| `late_delivery_rate` | Rasio order pelanggan yang terlambat. |

Output feature engineering menghasilkan 93.350 pelanggan unik. Beberapa missing value masih wajar muncul pada tahap ini, misalnya pelanggan yang tidak memiliki review score. Missing value tersebut tidak langsung dihapus karena jumlah review kosong bisa memiliki makna: tidak semua pelanggan memberikan review setelah berbelanja.

## 6. Preprocessing Data

K-Means adalah algoritma berbasis jarak. Artinya, model menghitung kedekatan antar pelanggan berdasarkan nilai fitur. Jika fitur tidak diproses dengan benar, fitur bernilai besar seperti total pembayaran bisa mendominasi fitur lain seperti review score atau late delivery rate.

Fitur yang dipilih untuk model akhir:

| Feature | Behavior Group | Reason |
| --- | --- | --- |
| `recency_days` | Recency | Mengukur seberapa baru pelanggan bertransaksi. |
| `order_count` | Frequency | Mengukur frekuensi pembelian. |
| `total_payment_value` | Monetary | Mengukur total nilai transaksi. |
| `avg_payment_installments` | Payment behavior | Mengukur pola penggunaan cicilan. |
| `total_product_count` | Product behavior | Mengukur total produk yang dibeli. |
| `freight_to_payment_ratio` | Freight behavior | Mengukur beban ongkir relatif terhadap pembayaran. |
| `avg_review_score` | Satisfaction | Mengukur kepuasan pelanggan dari review score. |
| `avg_delivery_days` | Delivery experience | Mengukur durasi pengiriman. |
| `late_delivery_rate` | Delivery experience | Mengukur rasio keterlambatan pengiriman. |
| `has_review_score` | Satisfaction signal | Menandai apakah pelanggan benar-benar memiliki review score sebelum imputasi. |

Tahap preprocessing:

1. Memilih fitur numerik yang relevan untuk customer segmentation.
2. Membuat fitur `has_review_score` sebelum imputasi.
3. Mengisi missing value dengan median untuk fitur yang masih memiliki nilai kosong.
4. Melakukan log transform pada fitur yang sangat skewed.
5. Melakukan standardisasi dengan `StandardScaler`.

Contoh pola kode missing value handling:

```python
model_data["has_review_score"] = model_data["avg_review_score"].notna().astype(int)

for column in columns_to_impute:
    median_value = model_data[column].median()
    model_data[column] = model_data[column].fillna(median_value)
```

Contoh pola kode log transform dan scaling:

```python
for column in skewed_non_negative_features:
    model_data[column] = np.log1p(model_data[column])

scaler = StandardScaler()
scaled_features = scaler.fit_transform(model_data[model_feature_columns])
```

Alasan memakai `np.log1p`:

- aman untuk nilai 0,
- membantu mengurangi efek nilai ekstrem,
- membuat distribusi fitur monetary/freight lebih stabil untuk clustering.

Alasan memakai `StandardScaler`:

- K-Means berbasis jarak,
- semua fitur perlu berada pada skala yang sebanding,
- fitur bernilai besar tidak boleh otomatis menjadi fitur paling dominan hanya karena satuannya lebih besar.

Output preprocessing:

- `data/processed/customer_features_preprocessed.csv`
- `data/processed/customer_features_model_ready.csv`

Kedua file tersebut tidak di-commit karena merupakan processed data yang bisa dibuat ulang dari notebook.

## 7. Metode yang Digunakan

Metode yang digunakan adalah K-Means Clustering.

K-Means bekerja dengan cara:

1. Menentukan jumlah cluster `K`.
2. Membuat centroid awal.
3. Menghitung jarak setiap data ke centroid.
4. Memasukkan setiap data ke cluster dengan centroid terdekat.
5. Menghitung ulang centroid berdasarkan rata-rata anggota cluster.
6. Mengulang proses sampai posisi cluster stabil.

Pada proyek ini, K-Means diuji untuk K=2 sampai K=8.

Parameter utama:

| Parameter | Nilai |
| --- | --- |
| Algorithm | K-Means Clustering |
| K values tested | 2 sampai 8 |
| `random_state` | 42 |
| `n_init` | 20 |
| Silhouette sample size | 10.000 |

`random_state=42` digunakan agar hasil lebih reproducible. `n_init=20` digunakan agar K-Means mencoba beberapa initial centroid dan memilih hasil yang lebih stabil berdasarkan inertia terbaik.

## 8. Evaluasi Model

Karena proyek ini adalah unsupervised learning, tidak ada label benar atau salah untuk setiap pelanggan. Oleh karena itu, metrik seperti accuracy, precision, recall, dan F1-score tidak digunakan. Metrik tersebut cocok untuk supervised learning yang memiliki target label, sedangkan customer segment pada proyek ini ditemukan oleh model.

Evaluasi yang digunakan:

| Metrik | Fungsi |
| --- | --- |
| Inertia | Mengukur seberapa dekat data dengan centroid di dalam cluster. Semakin kecil biasanya semakin compact, tetapi selalu turun saat K bertambah. |
| Elbow Method | Membantu melihat titik ketika penurunan inertia mulai tidak sebesar sebelumnya. |
| Silhouette Score | Mengukur apakah data lebih dekat ke cluster sendiri dibanding cluster lain. Semakin tinggi biasanya semakin baik. |
| Davies-Bouldin Score | Mengukur pemisahan antar cluster. Semakin rendah biasanya semakin baik. |
| Calinski-Harabasz Score | Mengukur rasio pemisahan antar cluster terhadap kepadatan dalam cluster. |
| Cluster Balance | Mengecek apakah cluster terlalu timpang atau masih berguna secara bisnis. |
| Business Interpretability | Mengecek apakah hasil cluster dapat diberi makna yang jelas dan bisa dijelaskan kepada reviewer. |

Ringkasan evaluasi K-Means:

| K | Inertia | Silhouette | Davies-Bouldin | Min Cluster Share | Max Cluster Share |
| --- | ---: | ---: | ---: | ---: | ---: |
| 2 | 797442.7123 | 0.5333 | 0.8562 | 0.0335 | 0.9665 |
| 3 | 673825.7037 | 0.4071 | 1.0524 | 0.0334 | 0.8872 |
| 4 | 573359.7378 | 0.2049 | 1.4242 | 0.0334 | 0.4511 |
| 5 | 482475.1088 | 0.2121 | 1.1868 | 0.0065 | 0.4432 |
| 6 | 433225.7284 | 0.2074 | 1.1743 | 0.0065 | 0.3529 |
| 7 | 394600.7946 | 0.1981 | 1.2388 | 0.0065 | 0.3061 |
| 8 | 360709.6027 | 0.2091 | 1.1999 | 0.0065 | 0.2993 |

## 9. Alasan Pemilihan K=4

Jumlah cluster akhir yang dipilih adalah K=4.

Pemilihan ini tidak hanya berdasarkan satu angka metrik. Jika hanya melihat silhouette score, K=2 terlihat paling tinggi dengan nilai 0.5333. Namun K=2 tidak dipilih karena satu cluster berisi sekitar 96,65% pelanggan. Segmentasi seperti ini terlalu kasar: hampir semua pelanggan masuk satu kelompok besar, sehingga insight bisnis yang dihasilkan kurang actionable.

K=3 juga belum cukup baik karena cluster terbesar masih berisi sekitar 88,72% pelanggan. Dengan distribusi seperti itu, model belum benar-benar memecah mayoritas pelanggan menjadi kelompok yang informatif.

K=5 sebenarnya boleh dipertimbangkan dan bukan pilihan yang salah secara teknis. Dibanding K=4, K=5 memiliki silhouette sedikit lebih tinggi, yaitu 0.2121 dibanding 0.2049, dan Davies-Bouldin lebih rendah, yaitu 1.1868 dibanding 1.4242. Jika hanya melihat dua metrik tersebut, K=5 terlihat menarik.

Namun K=5 mulai menghasilkan cluster yang sangat kecil, yaitu hanya sekitar 0,65% pelanggan atau 603 pelanggan. Cluster kecil tersebut informatif, tetapi membuat segmentasi utama menjadi lebih kompleks untuk tugas ini. K=5 juga cenderung memecah salah satu pola khusus, terutama sinyal review availability atau pelanggan tanpa review, sehingga interpretasinya menjadi lebih teknis dan kurang sederhana untuk dokumentasi awal IMV.

K=4 dipilih karena memberikan kompromi yang lebih baik:

- cluster terbesar turun menjadi sekitar 45,11%, jauh lebih seimbang dibanding K=2 dan K=3,
- cluster terkecil masih 3,34%, bukan 0,65% seperti K=5 sampai K=8,
- hasilnya menghasilkan empat cerita pelanggan yang jelas,
- setiap cluster bisa diberi rekomendasi bisnis yang masuk akal,
- jumlah segmen tidak terlalu banyak untuk dijelaskan dalam dokumentasi, PPT, dan video.

Dengan kata lain, K=5 dapat dianggap valid sebagai alternatif eksplorasi lanjutan, tetapi K=4 lebih cocok sebagai pilihan final untuk tugas ini karena lebih seimbang antara metrik, ukuran cluster, dan kemudahan interpretasi.

## 10. Visualisasi Evaluasi

### Elbow Curve

![K-Means Elbow Curve](https://raw.githubusercontent.com/Zendin110206/imv-customer-segmentation-kmeans/main/reports/figures/kmeans_elbow_curve.png)

Elbow curve menunjukkan bahwa inertia terus turun ketika K bertambah. Hal ini normal karena semakin banyak cluster, jarak data ke centroid biasanya semakin kecil. Karena inertia selalu turun, keputusan K tidak boleh hanya berdasarkan nilai inertia terkecil.

### Metric Comparison

![K-Means Metric Comparison](https://raw.githubusercontent.com/Zendin110206/imv-customer-segmentation-kmeans/main/reports/figures/kmeans_metric_comparison.png)

Grafik ini membandingkan silhouette score dan Davies-Bouldin score. Grafik membantu melihat bahwa K=2 kuat dari sisi silhouette, tetapi belum tentu paling berguna karena cluster balance-nya terlalu timpang.

### Cluster Balance

![K-Means Cluster Balance](https://raw.githubusercontent.com/Zendin110206/imv-customer-segmentation-kmeans/main/reports/figures/kmeans_cluster_balance.png)

Cluster balance penting karena segmentasi pelanggan harus bisa dipakai. Jika satu cluster terlalu dominan, hasilnya kurang membantu untuk membedakan strategi bisnis antar kelompok pelanggan.

## 11. Hasil Segmentasi Pelanggan

Model akhir menggunakan K=4 dan menghasilkan empat segmen pelanggan.

| Cluster | Segment Name | Customer Count | Customer Share |
| --- | --- | ---: | ---: |
| 0 | Low-value one-time customers with high freight burden | 42,112 | 45.11% |
| 1 | Higher-value installment one-time customers | 40,718 | 43.62% |
| 2 | Delayed and dissatisfied customers | 7,401 | 7.93% |
| 3 | Repeat and broader-basket customers | 3,119 | 3.34% |

Visualisasi ukuran segmen:

![Customer Segment Size Distribution](https://raw.githubusercontent.com/Zendin110206/imv-customer-segmentation-kmeans/main/reports/figures/customer_segment_size_distribution.png)

Visualisasi KPI bisnis:

![Business KPI Differences](https://raw.githubusercontent.com/Zendin110206/imv-customer-segmentation-kmeans/main/reports/figures/customer_segment_business_kpi_summary.png)

## 12. Interpretasi Setiap Cluster

### Cluster 0: Low-value one-time customers with high freight burden

Cluster ini berisi 42.112 pelanggan atau 45,11% dari dataset customer-level. Karakteristik utamanya adalah mayoritas pelanggan hanya membeli satu kali, memiliki median payment value lebih rendah, dan freight-to-payment ratio lebih tinggi.

Interpretasi bisnis:

Pelanggan pada cluster ini kemungkinan cukup sensitif terhadap ongkir. Jika nilai produk relatif rendah tetapi ongkir terasa besar, pelanggan mungkin tidak terdorong untuk membeli ulang.

Rekomendasi:

- memberikan promo ongkir dengan minimum pembelian,
- menawarkan bundling produk agar ongkir terasa lebih efisien,
- membuat kampanye reaktivasi sederhana untuk mendorong pembelian kedua.

### Cluster 1: Higher-value installment one-time customers

Cluster ini berisi 40.718 pelanggan atau 43,62% dari dataset customer-level. Pelanggan pada cluster ini juga mayoritas one-time buyer, tetapi memiliki nilai pembayaran lebih tinggi dan penggunaan cicilan lebih besar.

Interpretasi bisnis:

Pelanggan ini menunjukkan potensi nilai transaksi yang lebih tinggi. Mereka belum tentu loyal, tetapi mereka bersedia melakukan transaksi bernilai lebih besar, terutama jika metode pembayaran fleksibel.

Rekomendasi:

- menonjolkan promo cicilan,
- memberi rekomendasi produk bernilai lebih tinggi,
- menawarkan premium bundle,
- mengirim follow-up recommendation setelah pembelian pertama.

### Cluster 2: Delayed and dissatisfied customers

Cluster ini berisi 7.401 pelanggan atau 7,93% dari dataset customer-level. Karakteristik utamanya adalah review score lebih rendah, delivery days lebih panjang, dan late delivery rate tinggi.

Interpretasi bisnis:

Masalah utama cluster ini adalah pengalaman pengiriman. Pelanggan yang menerima pesanan terlambat cenderung lebih tidak puas dan memberi review lebih rendah. Jika pengalaman ini tidak diperbaiki, pelanggan berisiko tidak kembali.

Rekomendasi:

- memperbaiki monitoring keterlambatan pengiriman,
- memberi notifikasi proaktif ketika terjadi delay,
- memberikan service recovery seperti voucher atau kompensasi ringan,
- mengevaluasi seller atau jalur logistik yang sering terlambat.

### Cluster 3: Repeat and broader-basket customers

Cluster ini berisi 3.119 pelanggan atau 3,34% dari dataset customer-level. Karakteristiknya adalah order count lebih tinggi, total payment value lebih besar, dan variasi produk lebih luas.

Interpretasi bisnis:

Cluster ini adalah kelompok yang paling mendekati loyal customer pada data saat ini. Jumlahnya kecil, tetapi penting karena menunjukkan perilaku pembelian ulang dan potensi customer lifetime value yang lebih baik.

Rekomendasi:

- membuat loyalty program,
- memberi rekomendasi cross-selling,
- memberi early access pada promo tertentu,
- menjaga kualitas pengiriman agar pelanggan tetap kembali.

## 13. Visualisasi PCA

PCA digunakan untuk membantu memvisualisasikan hasil clustering dari fitur berdimensi banyak ke bentuk 2D dan 3D. PCA bukan model clustering utama. PCA hanya alat bantu visualisasi.

### PCA 2D

![Customer Segments PCA 2D](https://raw.githubusercontent.com/Zendin110206/imv-customer-segmentation-kmeans/main/reports/figures/customer_segments_pca.png)

Visualisasi PCA 2D membantu melihat gambaran pemisahan cluster pada dua komponen utama. Karena data asli memiliki lebih banyak dimensi, visualisasi 2D tidak selalu menunjukkan semua perbedaan cluster secara sempurna.

### PCA 3D

![Customer Segments PCA 3D](https://raw.githubusercontent.com/Zendin110206/imv-customer-segmentation-kmeans/main/reports/figures/customer_segments_pca_3d.png)

Visualisasi PCA 3D memberikan sudut pandang tambahan karena memakai tiga komponen utama. Visual ini membantu melihat struktur cluster dengan lebih luas dibanding PCA 2D.

## 14. Penjelasan Output Kodingan

Output utama dari notebook adalah:

| Output | Fungsi |
| --- | --- |
| `reports/tables/kmeans_evaluation_metrics.md` | Tabel evaluasi K=2 sampai K=8. |
| `reports/tables/customer_segment_profiles.md` | Profil median setiap cluster dan ukuran segmen. |
| `reports/tables/segment_interpretation.md` | Ringkasan interpretasi dan rekomendasi setiap segmen. |
| `reports/figures/kmeans_elbow_curve.png` | Visual elbow method. |
| `reports/figures/kmeans_metric_comparison.png` | Visual perbandingan metrik clustering. |
| `reports/figures/kmeans_cluster_balance.png` | Visual distribusi ukuran cluster untuk setiap K. |
| `reports/figures/customer_segment_size_distribution.png` | Visual ukuran final empat segmen. |
| `reports/figures/customer_segment_business_kpi_summary.png` | Visual perbedaan KPI bisnis antar segmen. |
| `reports/figures/customer_segments_pca.png` | Visual PCA 2D. |
| `reports/figures/customer_segments_pca_3d.png` | Visual PCA 3D. |

File processed CSV tidak dimasukkan ke GitHub karena dapat dibuat ulang dari notebook dan termasuk data hasil proses. Repository tetap menyimpan notebook, dokumentasi, tabel ringkasan, dan gambar hasil analisis agar reviewer bisa memahami proses dan hasilnya.

## 15. Kendala dan Solusi

| Kendala | Solusi |
| --- | --- |
| Dataset terdiri dari banyak tabel relational. | Tabel dibaca satu per satu, relasi dicek, lalu join dilakukan bertahap berdasarkan key yang jelas. |
| Timestamp dari CSV awalnya terbaca sebagai string. | Kolom tanggal dikonversi eksplisit menggunakan `pd.to_datetime(..., errors="coerce")`. |
| Pesanan non-delivered memiliki informasi pengiriman yang belum lengkap. | Feature engineering utama memakai order berstatus `delivered`. |
| Beberapa pelanggan tidak memiliki review score. | Dibuat fitur `has_review_score`, lalu missing review score diimputasi dengan median. |
| Fitur monetary dan freight skewed. | Digunakan `np.log1p` untuk mengurangi pengaruh nilai ekstrem. |
| Fitur memiliki skala berbeda. | Digunakan `StandardScaler` sebelum K-Means. |
| K=2 memiliki silhouette tinggi tetapi cluster terlalu timpang. | Pemilihan K mempertimbangkan cluster balance dan interpretasi bisnis, bukan satu metrik saja. |
| K=5 memiliki beberapa metrik yang menarik tetapi membuat cluster sangat kecil. | K=4 dipilih karena lebih mudah dijelaskan dan lebih seimbang untuk segmentasi utama. |
| Geolocation berpotensi menambah kompleksitas join. | Geolocation tidak dipakai pada model pertama dan disimpan sebagai ide pengembangan lanjutan. |

## 16. Kesimpulan

Proyek ini berhasil melakukan segmentasi pelanggan e-commerce menggunakan K-Means Clustering pada dataset Olist. Data mentah yang terdiri dari banyak CSV berhasil diubah menjadi customer-level dataset dengan 93.350 pelanggan unik. Setelah preprocessing, fitur perilaku pelanggan digunakan untuk mengevaluasi beberapa jumlah cluster dari K=2 sampai K=8.

K=4 dipilih sebagai hasil akhir karena memberikan keseimbangan terbaik antara metrik clustering, distribusi ukuran cluster, dan interpretasi bisnis. Walaupun K=5 memiliki beberapa metrik yang terlihat lebih baik, K=4 lebih sesuai untuk segmentasi utama karena tidak menghasilkan cluster yang terlalu kecil dan lebih mudah dijelaskan.

Empat segmen pelanggan yang ditemukan adalah:

1. Low-value one-time customers with high freight burden.
2. Higher-value installment one-time customers.
3. Delayed and dissatisfied customers.
4. Repeat and broader-basket customers.

Insight utama dari proyek ini adalah mayoritas pelanggan pada dataset Olist merupakan one-time buyers, tetapi mereka tidak memiliki karakteristik yang sama. Ada pelanggan yang lebih sensitif terhadap ongkir, ada pelanggan dengan nilai transaksi lebih tinggi dan penggunaan cicilan, ada pelanggan yang terdampak keterlambatan pengiriman, dan ada kelompok kecil yang menunjukkan perilaku repeat purchase.

Segmentasi ini dapat membantu bisnis menyusun strategi yang lebih terarah, seperti promo ongkir, bundling, rekomendasi produk, service recovery, dan loyalty program.

## 17. Link Kodingan dan Deliverables

Repository GitHub:

<https://github.com/Zendin110206/imv-customer-segmentation-kmeans>

Notebook utama:

- <https://github.com/Zendin110206/imv-customer-segmentation-kmeans/blob/main/notebooks/01_raw_data_inspection.ipynb>
- <https://github.com/Zendin110206/imv-customer-segmentation-kmeans/blob/main/notebooks/02_customer_feature_engineering.ipynb>
- <https://github.com/Zendin110206/imv-customer-segmentation-kmeans/blob/main/notebooks/03_feature_validation_preprocessing.ipynb>
- <https://github.com/Zendin110206/imv-customer-segmentation-kmeans/blob/main/notebooks/04_kmeans_modeling_and_cluster_interpretation.ipynb>

Dokumentasi pendukung di repository:

- <https://github.com/Zendin110206/imv-customer-segmentation-kmeans/blob/main/docs/raw_data_audit.md>
- <https://github.com/Zendin110206/imv-customer-segmentation-kmeans/blob/main/docs/feature_engineering_summary.md>
- <https://github.com/Zendin110206/imv-customer-segmentation-kmeans/blob/main/docs/preprocessing_summary.md>
- <https://github.com/Zendin110206/imv-customer-segmentation-kmeans/blob/main/docs/kmeans_evaluation_summary.md>

Link final untuk pengumpulan:

```text
Link HackMD public: gunakan link halaman HackMD ini setelah permission diset public-readable.
Link PPT: akan ditambahkan setelah file PPT final tersedia.
Link Video Presentasi: akan ditambahkan setelah video final tersedia.
Link Google Drive folder/file pendukung: akan ditambahkan jika final deliverables diunggah ke Google Drive.
```
