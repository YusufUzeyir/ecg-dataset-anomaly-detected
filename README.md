# Kurulum
Kafka'yı sayfasından [indirin](https://kafka.apache.org/downloads). İndirirken Source download kısmından değil de Binary downloads kısmından indirin.

[Buradan](https://kafka.apache.org/documentation/#zkversion) kafka sürümünüzle uyumlu çalışan zookeper sürümüne bakıp indirebilirsiniz.
Gerekli configration ayalarını yaptıktan sonra çalıştırma kısmına geçelim.

Spark'ı sayfasından [indirin](https://www.apache.org/dyn/closer.lua/spark/spark-3.5.4/spark-3.5.4-bin-hadoop3.tgz).


## Gerekli Kütüphaneler
```
pip install pandas numpy matplotlib seaborn scikit-learn torch kafka-python pyspark
```

## Çalıştırma
Cmd'yi açın ve zookeeper'ı çalıştırın.

```bash
zkserver
```
Zookeeper çalıştıktan sonra yeni bir cmd ekranında Kafka'nın kurulu olduğu dizine gidin.
```bash
cd C:\kafka_2.12-3.8.1\bin\windows
```
Kafkay'ı çalıştırın.
```bash
kafka-server-start.bat ..\..\config\server.properties
```

Yeni bir cmd ekranı açın ve Spark'ı çalıştırın.
```bash
pyspark
```
<hr>

# Proje Hakkında
Veri Setinin ilk 5 satırı
| record  | type |0_pre-RR	|0_post-RR|0_pPeak|0_tPeak|0_rPeak|
| ------------- | ------------- |------|-|-|-|-|
|I01|N|163|165|0.069610222|-0.083280601|0.614133158|
|I01|N|165|166|-0.097030038|0.597254429|-0.078703617|
|I01|N|166|102|0.109399148|0.68052794|-0.010648647|
|I01|VEB|102|231|0.176376471|0.256431069|-0.101097641|
|I01|N|231|165|0.585576857|0.607461311|-0.083498942|

Temizlenmiş veri setinin ilk 5 satırı
| record | type | 0_pre-RR          | 0_post-RR         | 0_pPeak          | 0_tPeak          | 0_rPeak          |
|--------|------|-------------------|-------------------|------------------|------------------|------------------|
| 0      | 1    | 0.30914826498422715 | 0.29999999999999993 | 0.5204273586199608 | 0.43704830497773817 | 0.4267099531052496 |
| 0      | 1    | 0.31545741324921134 | 0.3032258064516129  | 0.3739916644212248 | 0.6184598813426074  | 0.27066302487813837 |
| 0      | 1    | 0.31861198738170343 | 0.0967741935483871  | 0.5553920124211019 | 0.6406582667805286  | 0.2859909771495636  |
| 0      | 4    | 0.1167192429022082  | 0.5129032258064515  | 0.6142485623873464 | 0.527605920453083   | 0.265619241433121   |
| 0      | 1    | 0.31545741324921134 | 0.2935483870967741  | 0.47651322364936743 | 0.4294802377150699  | 0.42653890745210354 |




### Temizleme İşlemleri
- Kategorik verileri sayısal forma dönüştürme  
- Aykırı değerlerin z-skoru ile tespiti ve çıkarımı  
- Eksik değerleri ortalama ile doldurma  
- Verileri 0-1 aralığında normalize etme (MinMaxScaler)


### MinMaxScaler normalizasyonunu:  
$z = \frac{x - \min(x)}{\max(x) - \min(x)}$

### İstatistiksel Grafikeler:
Normalize edilmeiş veri setinin Box Plot grafiği <br>
<img src="[https://github.com/user-attachments/assets/92c30c72-02fd-4d64-99e4-776081d354a6](https://github.com/user-attachments/assets/2a06f0d9-89fb-4154-a58d-ca6df8f48a9c)" alt="image" width="600"/><br><br>
Korelasyon Matrisi<br>
<img src="https://github.com/user-attachments/assets/dfc49e23-afba-4dd6-a0ce-7f6cba36a30b" alt="image" width="600"/><br><br>
0_pre-RR Sütunu için normal-anormal veri dağılımı<br>
<img src="https://github.com/user-attachments/assets/24519b47-dac9-48ef-b847-38073f384c4f" alt="image" width="600"/><br><br>
Veri setindeki dengeli dağılım<br>
<img src="https://github.com/user-attachments/assets/8b199edf-fe96-486f-848b-ab6b8a460546" alt="image" width="400"/><br><br>

### Model Eğitimi
20 Epoch'daki değerler:
| Epoch | Training Loss | Validation Loss |
|-------|---------------|-----------------|
| 1     | 0.0509        | 0.0283          |
| 2     | 0.0296        | 0.0238          |
| 3     | 0.0262        | 0.0238          |
| 4     | 0.0249        | 0.0217          |
| 5     | 0.0242        | 0.0221          |
| 6     | 0.0223        | 0.0204          |
| 7     | 0.0218        | 0.0200          |
| 8     | 0.0214        | 0.0197          |
| 9     | 0.0210        | 0.0189          |
| 10    | 0.0204        | 0.0189          |
| 11    | 0.0206        | 0.0188          |
| 12    | 0.0197        | 0.0202          |
| 13    | 0.0193        | 0.0195          |
| 14    | 0.0196        | 0.0178          |
| 15    | 0.0190        | 0.0186          |
| 16    | 0.0192        | 0.0183          |
| 17    | 0.0191        | 0.0175          |
| 18    | 0.0182        | 0.0177          |
| 19    | 0.0184        | 0.0189          |
| 20    | 0.0180        | 0.0198          |

Eğitim ve Doğrulama kaybının grafiği<br>
<img src="https://github.com/user-attachments/assets/4f181356-707f-41ee-a88f-f580a23f59ab" alt="image" width="400"/><br><br>

Modelin metrik değerleri:
|                | precision | recall | f1-score |
|----------------|-----------|--------|----------|
| 0              | 0.97      | 0.86   | 0.91     |
| 1              | 0.99      | 1.00   | 1.00     |
| **accuracy**   |           |        | 0.99     |
| **macro avg**  | 0.98      | 0.93   | 0.95     |
| **weighted avg** | 0.99    | 0.99   | 0.99     |



