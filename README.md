# Deney Sonu Teslimatı

Sistem Programlama ve Veri Yapıları bakış açısıyla veri tabanlarındaki performansı öne çıkaran hususlar nelerdir?

Aşağıda kutucuk (checkbox) ile gösterilen maddelerden en az birini seçtiğiniz açık kaynak kodlu bir VT kaynak kodları üzerinde göstererek açıklayınız. Açıklama bölümüne kısaca metninizi yazıp, kod üzerinde gösterim videonuzun linkini en altta belirtilen kutucuğa yerleştiriniz.

- [X]  Seçtiğiniz konu/konuları bu şekilde işaretleyiniz. **!**
    
---

# 1. Sistem Perspektifi (Operating System, Disk, Input/Output)

### Disk Erişimi

- [x]  **Blok bazlı disk erişimi** → block_id + offset
- [ ]  Rastgele erişim

### VT için Page (Sayfa) Anlamı

- [x]  VT hangisini kullanır? **Satır/ Sayfa** okuması

---

### Buffer Pool

- [ ]  Veritabanları, Sık kullanılan sayfaları bellekte (RAM) kopyalar mı (caching) ?

- []  LRU / CLOCK gibi algoritmaları
- [x]  Diske yapılan I/O nasıl minimize ederler?

# 2. Veri Yapıları Perspektifi

- [x]  B+ Tree Veri Yapıları VT' lerde nasıl kullanılır?
- [ ]  VT' lerde hangi veri yapıları hangi amaçlarla kullanılır?
- [ ]  Clustered vs Non-Clustered Index Kavramı
- [ ]  InnoDB satırı diskte nasıl durur?
- [ ]  LSM-tree (LevelDB, RocksDB) farkı
- [x]  PostgreSQL heap + index ayrımı

DB diske yazarken:

- [x]  WAL (Write Ahead Log) İlkesi
- [ ]  Log disk (fsync vs write) sistem çağrıları farkı

---

# Özet Tablo


| Kavram |Sistem Perspektifi | Veritabanı Karşılığı |
|------|-----------------------------------------------|----------------------------------|
| Disk Erişimi | Bellekte pointer ile doğrudan erişim | Blok bazlı erişim (block_id + offset) |
| Buffer Pool | RAM üzerinde sık kullanılan verinin cache’lenmesi | Sayfaların (page) bellekte tutulması |
| Disk I/O Optimizasyonu | Gereksiz sistem çağrılarından kaçınma | Buffer Pool ile disk erişiminin azaltılması |
| B+ Tree | Dengeli ağaç yapısı, logaritmik arama | Index yapısı olarak kullanılır |
| WAL | Önce log’a yazma prensibi | Crash sonrası recovery ve durability sağlar |

---

# Video [Linki](https://www.youtube.com/watch?v=Nw1OvCtKPII&t=2635s) 
Ekran kaydı. 2-3 dk. açık kaynak V.T. kodu üzerinde konunun gösterimi. Video kendini tanıtma ile başlamalıdır (Numara, İsim, Soyisim, Teknik İlgi Alanları). 

---

# Açıklama (Ort. 600 kelime)

Veritabanı yönetim sistemlerinde performans, sistem programlama ve veri yapılarından bakıldığında 
donanım ile yazılım arasındaki etkileşimi görülmektedir. Özellikle disk erişim maliyetleri, bellek kullanımı ve uygun veri
yapılarının seçimi, bir veritabanının ölçeklenebilir ve hızlı çalışmasında önemlidir.

Sistem perspektifinden bakıldığında, disk erişimi veritabanları için en yavaş işlem
adımlarından biridir. İşletim sistemleri diske byte bazlı değil, blok bazlı erişim
yapar. Bu nedenle veritabanı sistemleri verileri diskten okurken block_id ve offset
mantığını kullanır. Rastgele disk erişimleri maliyetli olduğu için, veritabanları
mümkün olduğunca sıralı ve toplu erişimi tercih eder.Bu yüzden page ortaya çıkar.
Veritabanları satır bazlı değil,sayfa bazlı okuma yapar. Bir satıra erişilmek istendiğinde
satırın bulunduğu page tamamıyle belleğe alınır.
PostgreSQL gibi açık kaynak veritabanlarında bu yaklaşım,
disk I/O sayısını azaltarak performans artışı sağlar.

Veritabanı sistemlerinde disk erişimini azaltmak için Buffer Pool yapısı kullanılır. 
Buffer Pool, disk üzerindeki page’lerin
RAM üzerinde tutulduğu bellek alanıdır. Sık kullanılan page’ler bellekte
önbelleğe alınarak aynı veriye tekrar erişildiğinde disk yerine RAM kullanılır.
Buffer Pool dolduğunda, hangi page’in bellekten çıkarılacağına karar vermek için
sayfa değiştirme algoritmaları kullanılır.
Bu algoritmalar,belli süredir kullanılmayan page’leri bellekten çıkararak
aktif kullanılan verilerin RAM’de kalmasını sağlar.
Bu yaklaşım sayesinde diske yapılan I/O işlemleri minimum seviyeye indirilir.
Özellikle sorguların çoğunun bellek üzerinden karşılanması, veritabanı
performansını önemli ölçüde artırır. PostgreSQL,
işletim sisteminin cache mekanizmalarıyla birlikte çalışarak disk erişim maliyetini
daha da düşürmeyi hedefler.

Veri yapıları perspektifinden bakıldığında, veritabanı performansını belirleyen
yapılardan biri B+ Tree veri yapısıdır. B+ Tree, disk tabanlı sistemler
için dengeli bir ağaç yapısıdır ve veritabanı indekslerinin
oluşturulmasında yaygın olarak kullanılır. Bu yapı sayesinde arama, ekleme ve
silme işlemleri az sayıda page erişimi ile gerçekleştirilebilir.
B+ Tree’nin iç düğümleri yönlendirme bilgilerini içerirken, yaprak düğümleri
asıl veriye veya verinin disk üzerindeki adresine işaret eder. Her bir düğüm,
genellikle disk üzerindeki bir page’e karşılık gelir. Bu durum, disk erişim
sayısını sınırlayarak performansın artmasını sağlar.
PostgreSQL veritabanında tablo verileri heap yapısında saklanır ve indeksler
ayrı veri yapıları olarak tutulur. İndeksler, ilgili satırın bulunduğu page ve
offset bilgisini içerir. Bu heap + index ayrımı, veri ekleme ve güncelleme
işlemlerini daha esnek hale getirirken, indekslerin bağımsız olarak yönetilmesine
olanak tanır.

Veritabanlarında diske yazma işlemleri sırasında hem performans hem de veri
tutarlılığını sağlamak için WAL kullanılır.Veritabanında yapılacak tüm değişiklikler 
önce log dosyasına yazılır, ardından asıl veri dosyalarına uygulanır. 
Bu yaklaşım, sistem çökmesi durumunda verilerin log üzerinden geri yüklenebilmesini sağlar.
WAL write gibi sistem çağrılarını kontrollü şekilde kullanarak disk yazma maliyetini azaltmayı hedefler. 
Birden fazla değişikliğin aynı anda halde loglanması, rastgele disk yazımlarının önüne geçerek performans
artışı sağlar. PostgreSQL gibi açık kaynak veritabanlarında wal_writer ve
checkpointer süreçleri bu işlemleri yönetir.

Sonuç olarak, veritabanı performansı disk erişimlerinin minimize edilmesi,
buffer pool kullanımı, B+ Tree gibi uygun veri yapılarının tercih edilmesi ve
WAL gibi sistem seviyesinde güvenli yazma teknikleri ile doğrudan ilişkilidir.
Sistem programlama ve veri yapıları bilgisi, modern veritabanı sistemlerinin
tasarımını ve çalışma prensiplerini anlamada önemlidir.





## VT Üzerinde Gösterilen Kaynak Kodları


 [PostgreSQL Buffer Pool Kaynak Kodları](https://github.com/postgres/postgres/tree/master/src/backend/storage/buffer)
 [PostgreSQL B+ Tree Kaynak Kodları](https://github.com/postgres/postgres/tree/master/src/backend/access/nbtree)
 [PostgreSQL WAL (Transaction Log) Kodları](https://github.com/postgres/postgres/tree/master/src/backend/access/transam)
 [PostgreSQL Disk Page Kodları](https://github.com/postgres/postgres/tree/master/src/backend/storage/page)

  

