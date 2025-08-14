import sqlite3
class Ogrenci:
    ogrenciList=[]
    def __init__(self,isim,soyisim,no,puan):
        self.isim=isim
        self.soyisim=soyisim
        self.no=no
        self.puan=puan
    @classmethod 
    def ogrenciEkle(cls,isim,soyisim,no,puan):
        yeniOgrenci=Ogrenci(isim,soyisim,no,puan)
        cls.ogrenciList.append(yeniOgrenci)
       
    @classmethod
    def listeyiYazdir(cls):
        for ogrenci in cls.ogrenciList:
            print("ogrenci adi:{},ogrenci soyadi:{},ogrenci no:{},ogrenci puan:{}".format(ogrenci.isim,ogrenci.soyisim,ogrenci.no,ogrenci.puan))
    
class veriTabaniBaglantisi: 
    def __init__(self,veritabanıisim,tabloisim) :
     self.veritabanı=sqlite3.connect(veritabanıisim)
     self.imlec=self.veritabanı.cursor()
     self.tablo=tabloisim

     self.imlec.execute("CREATE TABLE IF NOT EXISTS {}(ogrenciAdi nvarchar(20),ogrenciSoyadi nvarchar(20),no  INTEGER PRIMARY KEY AUTOINCREMENT, puan INTEGER )".format(self.tablo)) 

    def ogrenciGetir(self):
        sorgu="SELECT * FROM {}".format(self.tablo)
        self.imlec.execute(sorgu)
        Ogrenci.ogrenciList.clear()
        for ogrenci in self.imlec:
          Ogrenci.ogrenciEkle(ogrenci[0],ogrenci[1],ogrenci[2],ogrenci[3])

    def ogrenciEkleme(self,isim,soyisim,puan):
        sorgu="INSERT INTO {} (ogrenciAdi,ogrenciSoyadi,puan) VALUES(?,?,?)".format(self.tablo)
        self.imlec.execute(sorgu,(isim,soyisim,puan))
        self.veritabanı.commit()
        self.ogrenciGetir()
        
    def ogrenciSil(self,no) :
        sorgu="DELETE FROM {} WHERE no={}".format(self.tablo,no)
        self.imlec.execute(sorgu)
        self.veritabanı.commit()
        self.ogrenciGetir()



    def notGüncelle(self,no,yeniNot):
        sorgu="UPDATE {} SET puan ={} WHERE no={}".format(self.tablo,yeniNot,no)
        self.imlec.execute(sorgu)
        self.veritabanı.commit()
        self.ogrenciGetir()
                           










veritabani=veriTabaniBaglantisi("ogrenciNotSistemi.db","ogrenci")     
veritabani.ogrenciEkleme("elifnur","atici",80)
veritabani.ogrenciEkleme("ayse","yilmaz",90)
veritabani.ogrenciEkleme("gül","zen",10)
veritabani.ogrenciGetir()
Ogrenci.listeyiYazdir()
veritabani.ogrenciSil(7)
        
Ogrenci.listeyiYazdir()

veritabani.notGüncelle(2,50)
Ogrenci.listeyiYazdir()
