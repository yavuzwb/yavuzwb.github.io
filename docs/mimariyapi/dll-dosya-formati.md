DLL, Microsoft işletim sisteminde birden fazla kullanılan programların verilerini içeren bir yazılım kütüphanesidir. Bir yürütülebilir yani exe dosyası çalıştırıldığında buraya aktarılan bir kütüphanedir de diyebiliriz.

**DLL’e Genel bir bakış**

Dynamic link yazılım veya uygulama çalıştırıldığı zaman, bunları kütüphaneye (library) bağlayan bir mekanizmadır. Bu mekanizma gerçekleştirildiği zamanda kütüphaneler hiçbir şekilde uygulama içerisinde kopyalanmaz ve aynı şekilde kalır. DLL bağlantısı oluşturulduğunda değil, uygulama çalıştırıldığında bağlantı verir. Bunun haricinde bir DLL dosyası başka bir DLL bağlantısı da içerebilir.  
  
**DLL’nin Avantajları**
  
1-) Daha az kaynak kullanır - Bir dll dosyası ana uygulama çalıştırıldığı zaman RAM’a yüklenmez ve gerekmediği sürece yer işgal etmezler. Sadece gerektiğinde yüklenir ve çalıştırılır. Örnek vermek gerekirse MS Word dosyasını düzenlediğimiz zaman, DLL dosyası gerekli değildir. Sadece yazıcıya göndermeye karar verdiğimizde DLL, dosyanın yüklenmesine ve çalıştırılmasında rol oynar.  
  
2-) Modüler mimarinin geliştirilmesinde yardımcı olur - Bir DLL, modüler veya birden çok sürüm gerektiren programların geliştirilmesinde yardımcı olur.  
  
3-) Kolay kurulum ve kullanım - DLL içerisinde yer alan bir işlev güncelleştirme veya düzeltmeye ihtiyaç duyduğun zaman, DLL’nin dağıtım ve yüklenmesi sırasında DLL, herhangi bir programa ihtiyaç duymaz. Ayrıca aynı DLL dosyasını pek çok uygulama kullanıyorsa eğer, kullanan uygulamaların tamamı bu güncelleştirme veya düzeltmeden yararlanabilir.

**Önemli DLL dosyaları**

- Kernel32.dll - User32.dll - Comdlg32.dll - Gdi32.dll

