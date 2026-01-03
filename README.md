# Deney Sonu Teslimatı

Sistem Programlama ve Veri Yapıları bakış açısıyla veri tabanlarındaki performansı öne çıkaran hususlar nelerdir?

Aşağıda kutucuk (checkbox) ile gösterilen maddelerden en az birini seçtiğiniz açık kaynak kodlu bir VT kaynak kodları üzerinde göstererek açıklayınız. Açıklama bölümüne kısaca metninizi yazıp, kod üzerinde gösterim videonuzun linkini en altta belirtilen kutucuğa yerleştiriniz.

- [X]  Seçtiğiniz konu/konuları bu şekilde işaretleyiniz. **!**
    
---

# 1. Sistem Perspektifi (Operating System, Disk, Input/Output)

### Disk Erişimi

- [ ]  **Blok bazlı disk erişimi** → block_id + offset
- [ ]  Rastgele erişim

### VT için Page (Sayfa) Anlamı

- [ ]  VT hangisini kullanır? **Satır/ Sayfa** okuması

---

### Buffer Pool

- [X]  Veritabanları, Sık kullanılan sayfaları bellekte (RAM) kopyalar mı (caching) ?

- [X]  LRU / CLOCK gibi algoritmaları
- [X]  Diske yapılan I/O nasıl minimize ederler?

# 2. Veri Yapıları Perspektifi

- [ ]  B+ Tree Veri Yapıları VT' lerde nasıl kullanılır?
- [ ]  VT' lerde hangi veri yapıları hangi amaçlarla kullanılır?
- [ ]  Clustered vs Non-Clustered Index Kavramı
- [ ]  InnoDB satırı diskte nasıl durur?
- [ ]  LSM-tree (LevelDB, RocksDB) farkı
- [ ]  PostgreSQL heap + index ayrımı

DB diske yazarken:

- [ ]  WAL (Write Ahead Log) İlkesi
- [ ]  Log disk (fsync vs write) sistem çağrıları farkı

---

# Özet Tablo

| Kavram      | Bellek          | Disk / DB      |
| ----------- | --------------- | -------------- |
| Adresleme   | Pointer         | Page + Offset  |
| Hız         | O(1)            | Page IO        |
| PK          | Yok             | Index anahtarı |
| Veri yapısı | Array / Pointer | B+Tree         |
| Cache       | CPU cache       | Buffer Pool    |

---

# Video [Linki](https://www.youtube.com/watch?v=Nw1OvCtKPII&t=2635s) 
Ekran kaydı. 2-3 dk. açık kaynak V.T. kodu üzerinde konunun gösterimi. Video kendini tanıtma ile başlamalıdır (Numara, İsim, Soyisim, Teknik İlgi Alanları). 

---

# Açıklama (Ort. 600 kelime)

Buffer Pool, veritabanı sistemlerinde disk I/O maliyetini azaltmak için kullanılan temel bir bellek yönetimi mekanizmasıdır. Disk erişimi sayfa (page) bazlıdır ve rastgele (random) erişimlerde gecikme belirgin biçimde artar. Bu nedenle veritabanı, sık kullanılan sayfaları RAM’de tutarak aynı veriye tekrar erişimde diske gitmek yerine bellekten erişmeyi hedefler. Bu yaklaşım “cache hit” oranını yükselttiği ölçüde sorgu gecikmesini düşürür ve sistemin toplam throughput’unu artırır. Ancak RAM sınırlı olduğu için Buffer Pool dolduğunda, yeni sayfaya yer açmak adına mevcut sayfalardan birinin “victim” olarak seçilip yerinden edilmesi gerekir.

Bu noktada devreye Buffer Replacement algoritmaları girer. Teorik olarak LRU (Least Recently Used) mantığı, en uzun süredir kullanılmayan sayfayı çıkarmayı hedefler; pratikte ise tam LRU’yu eşzamanlı ve düşük maliyetle uygulamak zordur. PostgreSQL gibi sistemler bu nedenle CLOCK-sweep (clock) yaklaşımına benzer stratejiler kullanır. CLOCK yaklaşımında bir “clock hand” (dairesel işaretçi) buffer’lar üzerinde dolaşır ve aday buffer’lar belirli bir “kullanım” göstergesine göre elenir ya da seçilir. PostgreSQL kaynak kodunda bu mantık “clock sweep hand” olarak açıkça modellenmiştir; örneğin freelist.c içinde nextVictimBuffer alanı “bir sonraki değerlendirilecek buffer indeksini” tutarak dairesel taramayı mümkün kılar.Benzer şekilde ilgili iç başlık dosyasında “boş buffer bulmak için clock sweep algoritmasını kullan” ifadesi ve tarama akışı görülebilir.

CLOCK-sweep’in performans avantajı, çok düşük ek yükle “yakın zamanda kullanılan sayfaları” korumaya çalışmasıdır. Hand ilerledikçe, yakın zamanda kullanılan sayfaların kullanım göstergesi (uygulama düzeyindeki erişim/pin davranışıyla ilişkili) victim olma olasılığını düşürür. Böylece çalışma seti bellekte daha uzun süre kalır; diskten tekrar okuma ihtiyacı azalır. Bu tasarım aynı zamanda eşzamanlı (concurrent) ortamlarda yönetilebilir bir kilitleme maliyetiyle uygulanabilir. Sonuç olarak Buffer Pool + CLOCK-sweep, disk I/O’yu minimize ederek sorgu yürütme süresini kısaltır ve sistem kaynaklarını daha verimli kullanır.

Bu çalışmada, CLOCK-sweep yaklaşımı PostgreSQL’in buffer yönetimi kaynak kodu üzerinde incelenmiş; freelist.c içerisindeki clock hand modeli ve buf_internals.h içindeki clock-sweep akışı üzerinden replacement mantığı gösterilmiştir. Ayrıca bufmgr.c dosyası üzerinden buffer manager’ın okuma yolunda cache kullanımını nasıl konumlandırdığı, yani “önce buffer’da ara, yoksa diskten getir” yaklaşımı, I/O minimization perspektifinden özetlenmiştir.
## VT Üzerinde Gösterilen Kaynak Kodları

Açıklama [Linki](https://...) \
Açıklama [Linki](https://...) \
Açıklama [Linki](https://...) \
... \
...
