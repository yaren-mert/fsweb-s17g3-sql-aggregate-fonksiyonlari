# SQL Sorgu Alıştırmaları

Bu hafta SQL sorguları üzerine çalışıyorsunuz. Bugünkü alıştırmada sizin için hazırladığımız veritabanında aşağıda istediğimiz sonuçları elde etmenize yarayan SQL sorgularını oluşturacaksınız.

## Proje Kurulumu

Projeyi forklayın ve clonlayın. Tamamladığınızda da pushlayın.

### Kütüphane Bilgi Sistemi

Bu veritabanı, bir okulun kütüphanesinden öğrencilerin aldıkları kitapların bilgisini barındırmaktadır.

#### Tablolar

`ogrenci` tablosu öğrencilerin listesini tutar.
`islem` tablosu öğrencilerin kütüphaneden aldıkları kitapların bilgilerini tutar
`kitap` tablosu kütüphanedeki kitapların bilgisini tutar
`yazar` tablosu kitapların yazarları bilgisini tutar
`tur` tablosu kitapların türlerini tutar.

Tablo ilişiklerini görmek için [ktphn.png] dosyasına göz atın.

Yazdığınız sorguları buradan test edebilirsiniz: [https://ergineer.com/assets/materials/fkg36so5-kutuphanebilgisistemi-sql/]

##### Görevler

Aşağıda istenilen sonuçlara ulaşabilmek için gerekli SQL sorgularını yazın.

MIN-MAX, COUNT-AVG-SUM, GROUP BY, JOINS (INNER, OUTER, LEFT, RIGHT
#ilk 3 soruyu join kullanmadan yazın.

    1) Öğrencinin adını, soyadını ve kitap aldığı tarihleri listeleyin.

    select ograd,ogrsoyad,atarih from ogrenci,islem where ogrenci.ogrno=islem.ogrno=islem.ogrno order by ograd

    2) Fıkra ve hikaye türündeki kitapların adını ve türünü listeleyin.
    select kitapadi,turadi from kitap, tur where kitap.turno = tur.turno and turadi in ("Hikaye","Fıkra")

    3) 10B veya 10C sınıfındaki öğrencilerin numarasını, adını, soyadını ve okuduğu kitapları listeleyin.
    select ograd,ogrsoyad,ogrenci.ogrno, kitapadi from ogrenci,islem,kitap where (sinif= "10B" or "10C") and ogrenci.ogrno=islem.ogrno and kitap.kitapno=islem.kitapno
    #join ile yazın
    4) Öğrencinin adını, soyadını ve kitap aldığı tarihleri listeleyin.
    select ograd,ogrsoyad,atarih from ogrenci inner join islem on ogrenci.ogrno=islem.ogrno

    5) Fıkra ve hikaye türündeki kitapların adını ve türünü listeleyin.
    select kitapadi,turadi from kitap inner join tur on kitap.turno=tur.turno where turadi in ("Hikaye", "Fıkra")

    6) 10B veya 10C sınıfındaki öğrencilerin numarasını, adını, soyadını ve okuduğu kitapları, öğrenci adına göre listeleyin.
    select ogrenci.ogrno,ograd,ogrsoyad,sinif,kitapadi from ogrenci inner join islem on ogrenci.ogrno=islem.ogrno inner join kitap on islem.kitapno=kitap.kitapno where sinif in ("10B","10C") order by ogrenci.ograd

    7) Kitap alan öğrencinin adı, soyadı, kitap aldığı tarih listelensin. Kitap almayan öğrencilerinde listede görünsün.
    select ograd,ogrsoyad,atarih from ogrenci left join islem on ogrenci.ogrno=islem.ogrno

    8) Kitap almayan öğrencileri listeleyin.
    select ograd,ogrsoyad,atarih from ogrenci left join islem on ogrenci.ogrno=islem.ogrno where atarih is null

    9) Alınan kitapların kitap numarasını, adını ve kaç defa alındığını kitap numaralarına göre artan sırada listeleyiniz.
    select kitap.kitapno, kitap.kitapadi, count(islem.islemno) as alınankitapsayısı from kitap
    left join islem on islem.kitapno= kitap.kitapno
    group by kitapno

    10) Alınan kitapların kitap numarasını, adını kaç defa alındığını (alınmayan kitapların yanında 0 olsun) listeleyin.
    select kitap.kitapno, kitap.kitapadi, count(islem.islemno) as alınankitapsayısı from kitap
    left join islem on islem.kitapno= kitap.kitapno
    group by kitapno

    11) Öğrencilerin adı soyadı ve aldıkları kitabın adı listelensin.
      select kitap.kitapno, kitap.kitapadi, count(islem.islemno) as alınankitapsayısı from kitap
    left join islem on islem.kitapno= kitap.kitapno
    group by kitapno

    12) Her öğrencinin adı, soyadı, kitabın adı, yazarın adı soyad ve kitabın türünü ve kitabın alındığı tarihi listeleyiniz. Kitap almayan öğrenciler de listede görünsün.
    select ograd,ogrsoyad,kitapadi,yazarad,yazarsoyad,turadi,atarih from ogrenci as o
    left join islem as i on i.ogrno=o.ogrno
    left join kitap as k on k.kitapno=i.kitapno
    left join yazar as y on y.yazarno=k.yazarno
    left join tur as t on t.turno=k.turno

    13) 10A veya 10B sınıfındaki öğrencilerin adı soyadı ve okuduğu kitap sayısını getirin.
    select ogrenci.ograd,ogrenci.ogrsoyad,  count(islem.islemno) as "okunan kitap sayısı"  from ogrenci left join islem on ogrenci.ogrno=islem.ogrno where ogrenci.sinif in ("10A","10B") group by ogrenci.ogrno

    14) Tüm kitapların ortalama sayfa sayısını bulunuz.
    #AVG
    select avg(sayfasayisi) from kitap

    15) Sayfa sayısı ortalama sayfanın üzerindeki kitapları listeleyin.
    select kitapadi,sayfasayisi from kitap
    where sayfasayisi > (select avg(sayfasayisi) from kitap)

    16) Öğrenci tablosundaki öğrenci sayısını gösterin
    select count(ogrno) from ogrenci

    17) Öğrenci tablosundaki toplam öğrenci sayısını toplam sayı takma(alias kullanımı) adı ile listeleyin.
    select count(ogrno) as toplamsayi from ogrenci

    18) Öğrenci tablosunda kaç farklı isimde öğrenci olduğunu listeleyiniz.
     select count(distinct ograd) from ogrenci;

    19) En fazla sayfa sayısı olan kitabın sayfa sayısını listeleyiniz.
    select max(sayfasayisi) from kitap

    20) En fazla sayfası olan kitabın adını ve sayfa sayısını listeleyiniz.
    select kitapadi,sayfasayisi from kitap where sayfasayisi=(select max(sayfasayisi) from kitap)

    21) En az sayfa sayısı olan kitabın sayfa sayısını listeleyiniz.
    select min(sayfasayisi) as enazsayfa from kitap

    22) En az sayfası olan kitabın adını ve sayfa sayısını listeleyiniz.
    select kitapadi,sayfasayisi from kitap
    where sayfasayisi=(select min(sayfasayisi) from kitap)

    23) Dram türündeki en fazla sayfası olan kitabın sayfa sayısını bulunuz.
    select * from kitap as k
    left join tur as t on t.turno=k.turno
    where t.turadi="Dram"
    order by k.sayfasayisi desc
    limit 1

    24) numarası 15 olan öğrencinin okuduğu toplam sayfa sayısını bulunuz.
    select sum(sayfasayisi),o.ogrno from ogrenci as o
    left join islem as i on i.ogrno=o.ogrno
    left join kitap as k on k.kitapno=i.kitapno
    where o.ogrno=15

    25) İsme göre öğrenci sayılarının adedini bulunuz.(Örn: ali 5 tane, ahmet 8 tane )
    select ograd, count(ograd) from ogrenci group by ograd

    26) Her sınıftaki öğrenci sayısını bulunuz.
    select sinif ,count(ogrno) from ogrenci group by sinif;

    27) Her sınıftaki erkek ve kız öğrenci sayısını bulunuz.
    select sinif,cinsiyet,count(cinsiyet) as ogrenci_sayısı from ogrenci
    group by sinif,cinsiyet

    28) Her öğrencinin adını, soyadını ve okuduğu toplam sayfa sayısını büyükten küçüğe doğru listeleyiniz.
    select o.ograd,o.ogrsoyad,sum(sayfasayisi) as toplamsayfa from ogrenci as o
    left join islem as i on i.ogrno=o.ogrno

    29) Her öğrencinin okuduğu kitap sayısını getiriniz.
    select Concat(o.ograd,o.ogrsoyad) as ad_soyad, count(i.ogrno) as okudugukitap from ogrenci as o
    inner join islem as i on i.ogrno=o.ogrno
    group by i.ogrno
