Yığın (stack), bilgisayar belleğinin, her işlev tarafından oluşturulan geçici değişkenleri (`main()` fonksiyon dahil) depolayan özel bir bölgesidir. Yığın, CPU tarafından oldukça yakından yönetilen ve optimize edilen bir `LIFO` (last in, first out) veri yapısıdır. Bir işlev, her yeni bir değişken bildirdiğinde, yığına itilir (pushed). Daha sonra bir fonksiyondan her çıktığında, o fonksiyon tarafından yığına gönderilen tüm değişkenler serbest bırakılır (yani silinirler). Bir yığın değişkeni serbest bırakıldığında, bu bellek bölgesi diğer yığın değişkenleri içiin kullanılabilir hale gelir.

Değişkenleri depolamak için yığını kullanmanın avantajı, belleğin sizin için yönetilmesidir. Belleği elle ayırmanız veya artık ihtiyacınız kalmadığında serbest bırakmanız gerekmez. Artı olarak, `CPU` yığın belleği çok verimli bir şekilde düzenlendğinden, yığın değişkenlerini okumak ve bunlara yazmak çok hızlıdır.

Yığın anlamının anahtarı, bir işlev çıktığında tüm değişkenlerinin yığından fırlatıldığı (ve bu nedenle sosuza kadar kaybolduğu) fikridir. Dolayısıyla, yığın değişkenleri doğası gereği yereldir (`local`). Bu, daha önce gördüğümüz değişken kapsamı (`variable scope`) veya yerel/genel değişkenler olarak bilinen bir kavramla ilgilidir. C programlamlamadki yaygın bir hata, programınızda bu işlevin dışındaki bir yerden (yani bu işlevden çıktıktan sonra), bir işlevin içindeki yığın üzerinde oluşturulan bir değişkene erişmeye çalışmaktır.

Yığının bir başka özelliği de; yığında depolanabilecek değişkenlerin boyutunda bir sınırın (işletim sistemine göre değişebilir) olmasıdır. Bu, öbek üzerinde ayrılan değişkenler için geçerli değildir.

Yığını özetlemek gerekirse;

- Yığın, yerel değişkenleri `pop` ve `push` yaptıkça büyür ve küçülür.

- Yığının boyut sınırları vardır.

- Yığın değişkenleri yalnızca onları oluşturan işlev çalışırken var olur.

- Hafızayı kendi başınıza yönetmenize gerek yoktur, değişkenler otomatik olarak tahsis edilir ve serbest bırakılr.

# Heap

Heap, bilgisayar belleğinin sizin için otomatik yönetilmeyen ve CPU tarafından sıkı bir şekilde yönetilmeyen bölgesidir. Yani daha serbest olan bir bellek bölgesidir ve daha büyüktür. Heap üzerinde bellek ayırmak için, yerleşik C işlevleri olan `malloc()` veya `calloc()` kullanmalısınız. Heap üzerindeki bellek free() olarak ayırdıktan sonra, artık ihtiyacınız kalmadığında bu belleği serbest bırakmak için kullanmaktan sorumlusunuz. Eğer bunu gerçekleştirmezseniz, programınızda bellek sızıntısı olarak bilinen olay gerçekleşecektir. Yani Heap üzerindeki bellek yine de bir kenara bırakalıcaktır ve diğer işlemler tarafından kullanılmayacaktır. Bu yüzden bellek sızıntıları tespit edebilmek için valgrind adında yardımcı bir araçtan yararlanılabilir.

Stack'ın aksine, Heap'in değişken boyutta boyut kısıtlamaları yoktur. Heap belleğinin okunması ve yazılması biraz daha yavaştır. Çünkü öbek üzerindeki belleğe erişmek için işaretçiler (`pointers`) kullanmak gerekir.

Stack'ın aksine, öbek üzerindeki oluşturulan değişkenlere programımızın herhangi bir yerindeki herhangi bir işlev tarafından erişilebilir. Yığın değişkenleri esasen kapsam olarak küreseldir.

#  Stack'ın Artıları - Eksileri

- Hızlı bir erişim mümkündür

- Değişkenleri açıkca ayırma zorunluluğu yoktur

- Yalnıza yerel değişkenler

- İşletim sistemine bağlı olarak stack boyut sınırı vardır

- Değişkenler yeniden boyutlandırılamaz

- Bellek alanı CPU tarafından verimli bir şekilde yönetilir, parçalanmaz.

#  Heap'in Artıları - Eksileri

- Hızlı bir erişim mümkün değildir ve nispeten daha yavaştır

- Bellek alanı verimli bir şekilde yönetilmez. Bellek, blokları parçaladıkça ve daha sonra serbest bıraktıkça zamanla parçalanabilir.

- Değişkenler kullanılarak yeniden boyutlandırılabilir

- Belleği yönetebilmek için değişkenleri ayırma ve yönetmeden siz sorumlusunuzdur.

- Değişkenlere küresel olarak erişilebilir
