# SQL Sorgu Alıştırmaları

Bu hafta SQL sorguları üzerine çalışıyorsunuz. Bugünkü alıştırmada sizin için hazırladığımız veritabanında aşağıda istediğimiz sonuçları elde etmenize yarayan SQL sorgularını oluşturacaksınız.

# Proje Kurulumu
Projeyi forklayın ve clonlayın. Tamamladığınızda da pushlayın.

## Kütüphane Bilgi Sistemi

Bu veritabanı, bir okulun kütüphanesinden öğrencilerin aldıkları kitapların bilgisini barındırmaktadır.

#Tablolar 
`ogrenci` tablosu öğrencilerin listesini tutar.
`islem` tablosu öğrencilerin kütüphaneden aldıkları kitapların bilgilerini tutar
`kitap` tablosu kütüphanedeki kitapların bilgisini tutar
`yazar` tablosu kitapların yazarları bilgisini tutar
`tur` tablosu kitapların türlerini tutar.

Tablo ilişiklerini görmek için [ktphn.png] dosyasına göz atın.

Yazdığınız sorguları buradan test edebilirsiniz: [https://ergineer.com/assets/materials/fkg36so5-kutuphanebilgisistemi-sql/]



# Görevler
Aşağıda istenilen sonuçlara ulaşabilmek için gerekli SQL sorgularını yazın. 



	1) ÖRNEK SORU: Yazar tablosunu KEMAL UYUMAZ isimli yazarı ekleyin.
	INSERT INTO yazar (yazarad,yazarsoyad) VALUES ('KEMAL','UYUMAZ');

	
	2) Biyografi türünü tür tablosuna ekleyiniz.
	INSERT INTO tur (turadi) VALUES ('Biyografi');
	
	3) 10A sınıfı olan ÇAĞLAR ÜZÜMCÜ isimli erkek, 
    sınıfı 9B olan LEYLA ALAGÖZ isimli kız ve sınıfı 11C olan Ayşe Bektaş isimli kız öğrencileri 
    tek sorguda ekleyin. 
	INSERT INTO ogrenci (ograd,ogrsoyad,sinif,cinsiyet) VALUES ('ÇAĞLAR','ÜZÜMCÜ','10A','E')
    ('LEYLA','ALAGÖZ','9B','K')
    ('Ayşe','Bektaş','11C','K');
	
	4) Öğrenci tablosundaki rastgele bir öğrenciyi yazarlar tablosuna yazar olarak ekleyiniz.
	INSERT INTO yazar (yazarad,yazarsoyad) (SELECT ograd,ogrsoyad FROM ogrenci ORDER BY RAND()
    LIMIT 1);
	
	5) Öğrenci numarası 10 ile 30 arasındaki öğrencileri yazar olarak ekleyiniz.
	INSERT INTO yazar (yazarad,yazarsoyad) (SELECT ograd,ogrsoyad WHERE ogrno <30 AND ogrno > 10
    FROM ogrenci ORDER BY RAND());
	
	6) Nurettin Belek isimli yazarı ekleyip yazar numarasını yazdırınız.
	(Not: Otomatik arttırmada son arttırılan değer @@IDENTITY değişkeni içinde tutulur.)
	INSERT INTO yazar (yazarad,yazarsoyad) VALUES ('Nurettin','Belek') 
    SELECT @IDENTITY as yazarno;
	
	7) 10B sınıfındaki öğrenci numarası 3 olan öğrenciyi 10C sınıfına geçirin.
	UPDATE ogrenci SET sinif = '10C' WHERE ogrno = 3;
	
	8) 9A sınıfındaki tüm öğrencileri 10A sınıfına aktarın
	UPDATE ogrenci SET sinif = '10A' WHERE sinif = '9A';
	
	9) Tüm öğrencilerin puanını 5 puan arttırın.
	UPDATE ogrenci SET puan = puan + 5;
	
	10) 25 numaralı yazarı silin.
    DELETE FROM yazar WHERE yazarno = 25

	11) Doğum tarihi null olan öğrencileri listeleyin. (insert sorgusu ile girilen 3 öğrenci listelenecektir)
	SELECT * FROM ogrenci WHERE dtarih is null
	
	12) Doğum tarihi null olan öğrencileri silin. 
	DELETE FROM ogrenci WHERE dtarih is null
	
	13) Kitap tablosunda adı a ile başlayan kitapların puanlarını 2 artırın.
	UPDATE kitap SET puan = puan + 2 WHERE kitapadi ILIKE 'a%'
	
	14) Kişisel Gelişim isimli bir tür oluşturun.
	INSERT INTO tur (turadi) VALUES ('Kişisel Gelişim')
	
	15) Kitap tablosundaki Başarı Rehberi kitabının türünü bu tür ile değiştirin.
	UPDATE kitap SET turno = (SELECT turno FROM tur WHERE turadi = 'Kişisel Gelişim')
    WHERE kitapadi = 'Başarı Rehberi'
	
	16) Öğrenci tablosunu kontrol etmek amaçlı tüm öğrencileri görüntüleyen "ogrencilistesi" adında bir 
    prosedür oluşturun.
	CREATE PROCEDURE ogrencilistesi 
    LANGUAGE 'SQL' 
    AS $BODY$
        SELECT * FROM ogrenci
    $BODY$;
    CALL ogrencilistesi;
	
	17) Öğrenci tablosuna yeni öğrenci eklemek için "ekle" adında bir prosedür oluşturun.
	CREATE PROCEDURE ekle (IN ograd character varying, IN ogrsoyad character varying, 
    IN cinsiyet character varying,IN dtarih date, IN puan integer)
    LANGUAGE 'SQL' as ogr
    AS $BODY$
        INSERT INTO ogrenci VALUES(ogr.ograd,ogr.ogrsoyad,ogr.dtarih,ogr.cinsiyet,ogr.puan)
    $BODY$;
    CALL ekle('hazal';'taştan';'K';'1997-07-11';10);
	
	18) Öğrenci noya göre öğrenci silebilmeyi sağlayan "sil" adında bir prosedür oluşturun.
	CREATE PROCEDURE sil (IN ogrno integer)
    LANGUAGE 'SQL' as ogr
    AS $BODY$
        DELETE FROM ogrenci WHERE ogrno = ogr.ogrno
    $BODY$
    CALL sil(1)
	
	19) Öğrenci numarasını kullanarak kolay bir biçimde öğrencinin sınıfını değiştirebileceğimiz bir 
    prosedür oluşturun.
	CREATE PROCEDURE sinif.guncelle (IN ogrno integer,IN sinif character varying)
    LANGUAGE 'SQL' as ogr
    AS $BODY$
        UPDATE ogrenci SET sinif = ogr.sinif FROM ogrenci WHERE ogrno = ogr.ogrno
    $BODY$
    CALL sinif.guncelle(2,'10A')
	
	20) Öğrenci adı ve soyadını "Ad Soyad" olarak birleştirip, ad soyada göre kolayca arama yapmayı 
    sağlayan bir prosedür yazın.
	CREATE PROCEDURE ad.soyad(IN fullname character varying)
    LANGUAGE 'SQL' as ogr
    AS $BODY$
    SELECT * FROM ogrenci WHERE CONCAT (ograd,ogrsoyad) LIKE CONCAT ('%',ogr.fullname,'%')
    $BODY$
    CALL ad.soyad('Hazal Taştan')
	
	21) Daha önceden oluşturduğunu tüm prosedürleri silin.
	DROP PROCEDURE IF EXIST 'ekle'
    DROP PROCEDURE IF EXIST 'sil'
    DROP PROCEDURE IF EXIST 'ad.soyad'
    DROP PROCEDURE IF EXIST 'sinif.guncelle'
    DROP PROCEDURE IF EXIST 'ogrencilistele'
	
	#Esnek görevler (Esnek görevlerin hepsini Select in Select ile gerçekleştirmeniz beklenmektedir.)
	22) Select in select yöntemiyle dram türündeki kitapları listeleyiniz.
	SELECT * FROM kitap WHERE turno = (SELECT turno FROM tur WHERE turadi = 'Dram')
	
	23) Adı e harfi ile başlayan yazarların kitaplarını listeleyin.
	SELECT * FROM kitap WHERE yazarno IN (SELECT yazarno FROM yazar WHERE yazaradi ILIKE 'e%')
	
	24) Kitap okumayan öğrencileri listeleyiniz.
	SELECT * FROM ogrenci WHERE ogrno NOT IN (SELECT ogrno FROM islem)
	
	25) Okunmayan kitapları listeleyiniz
    SELECT * FROM kitap WHERE kitapno NOT IN (SELECT kitapno FROM islem)
	
	26) Mayıs ayında okunmayan kitapları listeleyiniz.
    SELECT * FROM kitap WHERE kitapno IN (SELECT kitapno FROM islem 
    WHERE EXTRACT (MONTH FROM atarih::timestamp)!= 5 AND EXTRACT (MONTH FROM vtarih::timestamp)!=5);