**Bellek ve Adresleme Modları (Memory and Addressing Modes)**

# 1-) Statik Veri Bölgelerini (static data regions) Bildirmesi

Bu amaç için özel assembler direktiflerini kullanarak x86 derlemesinde statik veri bölgelerini bildirebilirsiniz. Veri bildirimlerinden önce .data yönergesi gelmelidir. Bu yönergeyi takiben `.byte`, `.short` ve `.long` yönergeleri sırasıyla bir, iki ve dört byte’lık veri konumlarını bildirmek için kullanılabilir. Oluşturulan verilerin adresine atıfta bulunmak için onları etiketleyebilirsiniz. Bu etiketler montaj da çok yararlı ve çok yönlüdür, daha sonra ise birleştirici veya bağlayıcı tarafından anlaşılacak olan bellek konumlarına (`memory locations`) adlar verirler. Bu, değişkenleri isme göre bildirmeye benzer ancak bazı alt düzey kurallara uyar. Örnek vermek gerekirse sırayla bildirilen konumlar bellek içerisinde birbirinin yanında yer alacaktır.

Örnek olarak;

```
.data

var:
      .byte 64 /*Konum değişkeni olarak adlandırılan ve 64 bir değerini içerren byte bildirin*/
 
      .byte 10 /* 10 değerini içeren etiketsiz bir byte bildirin, Konumu  "var + 1"dir .*/
x:
      .short 42 /* 42 olarak başlatılan, konumu x olarak adlandırılan 2 byte’lık bir değer bildirin. */
y:
      .long 30000 
```
Dizilerin birçok boyuta sahip olabileceği ve indekslerle erişilebildiği high-level dillerin aksine x86 assembly programlama dilinde diziler, bellekte bitişik olarak bulunan birkaç hücredir. Aşağıdaki ilk örnekte olduğu gibi, bir dizi yalnızca değerleri listeleyerek bildirilebilir. Bir byte dizisinin özel durumu için dize değişmezleri kullanılabilir. Geniş bir bellek alanının sıfırlarla doldurulması durumunda .zero yönergesi kullanılabilir.

Örnek olarak;

```
S:
   .long 1, 2, 3  /* 1,2 ve 3 olarak başlatılan üç tane 4 byte’lık değer bildirin.  s + 8 konumundaki değer 3 olacaktır */

Barr:
    .zero 10  /* 0 olarak başlatılan barr konumundan başlayarak 10 byte bildirin  */
Str:
     .string "Merhaba"  /* Merhaba için ASCII karakter değerlerine başlatılan adres Str'nin başlatıldığı adresten başlayarak ardından nul (0) byte ile başlayan 6 byte bildirin */
```
# 2-) Adresleme Belleği (Addressing Memory) 

Modern x86-uyumlu işlemciler `232 byte`’a kadar bellek adresleme kapasitesine sahiptir; bellek adresleri `32 byte` genişliğindedir. Bellek bölgelerine atıfta bulunmak için etiketleri kullandığımız yukarıdaki örneklerde görüldüğü üzere bu etiketler aslında derleyici (assembler) tarafından bellekteki adresleri belirtilen 32 byte’lık miktarlarla değiştirilebilir. Bellek bölgelerine etiketlerle (yani sabit değerler) atıfta bulunmayı desteklemeye ek olarak x86, bellek adreslerini hesaplamak ve bunlara başvurmak için esnek bir şema sağlar. Bu esneklik 32 byte’lık yazmaçlardan ikisine kadar ve 32 byte’lık işaretli sabit birlikle eklenebilir bir hafıza adresini hesaplamak içindir. Kayıtlardan biri isteğe bağlı olarak `2`, `4` veya `8` ile önceden çarpılabilir.

Adresleme modları birçok x86 talimatıyla birlikte kullanılabilir. Burada, verileri yazmaçlar ve bellek arasında hareket ettiren mov komutunu kullanan bazı örnekler gösteriyoruz. Bu komutun iki işleneni vardır; birincisi `kaynak`, ikincisi ise `hedefi` belirtmektir.

Adres hesaplamalarını kullanan bazı mov komut örnekleri aşağıda yer almaktadır;

```
mov (%ebx), %eax
mov %ebx, var(,1)
mov -4(%esi), %eax
mov %cl, (%esi,%eax,1)
mov (%esi,%ebx,4), %edx 
```
Geçersiz adres hesaplamalarına ait bazı örneklerde şunları içeriyor;

```
mov (%ebx,%ecx,-1), %eax
mov %ebx, (%eax,%esi,%edi,1)
```
# 3-) İşlem Son Ekleri (Operation Suffies)

Genel olarak belirli bir bellek adresindeki veri öğesinin amaçlanan boyutu, referans verildiği assembly kodu talimatından çıkarılabilir. Örnek vermek gerekirse; yukarıdaki tüm talimatlarda, bellek bölgelerinin boyutu, yazmaç işleneninin boyutundan çıkarılabilir. `32 byte`’lık bir yazmaç yüklerken derleyici, bahsettiğimiz bellek bölgesinin `4 byte` genişliğinde olduğu sonucuna varabilirdi. 1 byte’lık bir kaydın değerini belleğe kaydederken derleyici, adresin bellekteki `tek bir byte`’a başvurmasını istediğimizi çıkarabilirdi.

Bununla birlikte, bazı durumlarda başvurulan bellek bölgesinin boyutu belirsizdir. Mesela `mov $2, (%ebx)` talimatını düşünelim. Bu komut 2 değerini `EBX` adresindeki tek byte’a taşımalı mı? Belki de 2’nin 32 byte’lık tamsayı gösterimini EBX adresinden başlayarak 4 byte’a taşımalıdır. Her ikisi de geçerli bir yorum olduğundan derleyici hangisinin doğru olduğuna açıkca yönlendirilmelidir. Boyut örnekleri `b`, `w` ve `I` bu amaca hizmet eder ve sırasıyla `1`, `2` ve `4` byte’lık boyutları belirler.

Örnek olarak;

```
movb $2, (%ebx)
movw $2, (%ebx)
movl $2, (%ebx)
```
# 4-) Talimatlar (Instructions)

Makina talimatları genellikle üç kategoriye ayrılır; 

- Data Movement (`veri hareketi`)
- Arithmetic/logic (`aritmatik/mantık`)
- Control-flow (`kontrol akışı`)

Bu bölümde her kategoriden önemli x86 talimatlarına bakacağız. Burada x86 talimatlarının kapsamlı bir liste olarak sunulmasından ziyade yararlı bir alt küme olarak düşünülmelidir. Daha öncede belirttiğim gibi tam kapsamlı liste için Intel’in resmi web sitesinde sunduğu dökümantasyonlara bakılması gerekmektedir.

# - Data Movement (veri hareketi)

**mov (Move)** — `mov` komutu birinci işleneni (kayıt içerikleri, bellek içerikleri veya sabit bir değer) tarafından belirtilen veri öğesini, ikinci işlenen tarafından belirtilen konuma (bir kayıt veya bellek) kopyalar. Register-to-register hareketleri mümkün olsa da, bellekten belleğe doğrudan hareketler mümkün değildir. Hafiza transferlerinin istendiği durumlarda, kaynak hafıza içerikleri önce bir kayıt listesine yüklenmelidir ve ardından hedef hafıza adresine depolanabilir.

Örnek olarak;

```
mov %ebx, %eax — EBX’teki değeri EAX’a kopyalar
movb $5, var(,1) — 5 değerini byte var konumunda depolar
```
**push (Push on stack)** — `push` talimatı, işlenenini bellekteki donanım destekli yığının üstüne yerleştirir. Spesifik olarak, push ilk olarak ESP’yi 4 azaltır ardından işlenenini adresteki 32 byte’lık konumun içeriğine yerleştirir (`%esp`). ESP, x86 yığını büyüdüğünden yani yığın yüksek adreslerden daha düşük adreslere doğru büyüdüğünden, itme ile azaltılır.

Syntax;

```
push <reg32>
push <mem>
push <con32>
```

Örnek vermek gerekirse;

```
push %eax — eax’ı yığına it
push var(,1) — var adresindeki 4 byte’ı yığına it
```
**pop (Pop from stack)** — `pop` talimatı, 4 byte’lık veri elemanını donanım destekli yığının üstünden belirtilen işlenene (kayıt veya bellek konumu) kaldırır. Önce bellek konumunda (%esp) bulunan 4 byte’ı belirtilen kayıt veya bellek konumuna taşır ve ardından `ESP`’yi 4 artırır.

Syntax;

```
pop <reg32>
pop <mem>
```

Örnek olarak;

```
pop %edi — yığının en üstteki öğesini EDI’ye aç
pop (%ebx) — EBX konumundan başlayarak yığının en üst elemanını 4 byte’ta belleğe açar
```

**lea (Load effective address)** — `lea` talimatı, ilk işlenen tarafından belirtilen adresi ikinci işlenen tarafından belirtilen kayda yerleştirir. Hafıza konumunun içeriği yüklenmez, sadece etkin ve adres ve hesaplanır ve kayıt listesine yerleştirilir. Bu, bir bellek bölgesine bir işaretçi elde etmek veya basit aritmetik işlemler gerçekleştirmek için kullanışlıdır.

Syntax;

```
lea <mem>, <reg32>
```

Örnek vermek gerekirse;

```
lea (%ebx,%esi,8), %edi — "EBX + 8" * ËSI" miktarını EDI’ye yerleştirir 
lea val(,1), %eax —  val değeri EAX’a yerleştirilir
```
# - Arithmetic/logic (aritmatik/mantık)

**add (Integer addition)** — `add` talimatı, iki işlenenini toplayarak sonucu ikinci işleneninde saklar. Her iki işlenen de yazmaç olabilirken, en fazla bir işlenen bir bellek konumu olabilir.

Syntax;

```
add <reg>, <reg>
add <mem>, <reg>
add <reg>, <mem>
add <con>, <reg>
add <con>, <mem>
```

Örnek vermek gerekirse;

```
add $10, %eax — EAX, EAX + 10 olarak ayarlandı.
addb $10, (%eax) — EAX’ta depolanan bellek adresinde depolanan tek byte’a 10 ekleyin
```

**sub (Integer subtraction)** — `sub` talimatı, birinci işlenenin değerini ikinci işlenenin değerinden çıkarmanın sonucunu, ikinci işlenenin değerinde depolar. Add ile olduğu gibi her iki işlenende yazmaç olabiirken, en fazla bir işlenen bir bellek konumu olabilir.

Syntax;

```
sub <reg>, <reg>
sub <mem>, <reg>
sub <reg>, <mem>
sub <con>, <reg>
sub <con>, <mem>
```

Örnek vermek gerekirse;

```
sub %ah, %al — AL, AL - AH olarak ayarlanır
sub $216, %eax — EAX’ta depolanan değerden 216 çıkar
```
**inc, dec (Increment, Decrement)** — `inc` talimatı, işlenenin içeriğini bir arttırır. `Dec` talimatı ise işlenen içeriğini birer birer azaltır.

Syntax;

```
inc <reg>
inc <mem>
dec <reg>
dec <mem>
```

Örnek vermek gerekirse;

```
dec %eax — EAX içeriğinden bir tane çıkarın
incl var(,1) — var konumunda depolanan 32 byte’lık tam sayıya bir ekleyin
```

**imul (Integer multiplication)** — `imul` talimatının iki temel biçimi vardır: iki işlenen (yukarıdaki syntax listesindeki ilk iki) ve üç işlenen yukarıdaki syntax listesindeki son iki) dir. İki işlenen formu, iki işleneni birlikte çarpar ve sonucu ikinci işlenende depolar. Sonuç olarak işlenen bir kayıt olmalıdır.

Üç işlenen formu, ikinci ve üçüncü işlenenlerini birlikte çarpar ve sonucu son işleneninde saklar. Yine, sonuç işlenen bir kayıt olmalıdır. Ayrıca ilk işlenen sabit bir değerle sınırlıdır.

Syntax;

```
imul <reg32>, <reg32>
imul <mem>, <reg32>
imul <con>, <reg32>, <reg32>
imul <con>, <mem>, <reg32>
```

Örnek vermek gerekirse;

```
imul (%ebx), %eax — EAX içeriğini, EBX konumundaki 32 byte’lık bellek içeriğiyle çarpın. Sonucu EAX’ta saklayın.
imul $25, %edi, %esi — ESI, EDI*25 olarak ayarlandı
```

**idiv (Integer division)** — `idiv` talimatı, 64 bitlik tamsayı `EDX:EAX`’in içeriğini belirtilen işlene değerine böler. Bölümün bölüm sonucunu `EAX`’ta saklanırken, geri kalanı `EDX`’e yerleştirir.

Syntax;

```
idiv <reg32>
idiv <mem>
```

Örnek vermek gerekirse;

```
idiv %ebx — EDX:EAX’in içeriğini EBX içeriğine bölün. Bölümü EAX’e ve kalanı EDX’e yerleştirin
idivw (%ebx) — EDX:EAX’in içeriğini EBX’teki bellek konumunda depolanan 32 bitlik değere bölün. Bölümü EAX’e ve kalanı EDX’e yerleştirin.
```
**and, or, xor (Bitwise logical and, or, and exclusive or)** — Bu talimatlar, ilk işlenen konumuna yerleştirerek, işlenenleri üzerinde belirtilen mantıksal işlemi gerçekleştirir.

Syntax;

```
and <reg>, <reg>
and <mem>, <reg>
and <reg>, <mem>
and <con>, <reg>
and <con>, <mem>
or <reg>, <reg>
or <mem>, <reg>
or <reg>, <mem>
or <con>, <reg>
or <con>, <mem>
xor <reg>, <reg>
xor <mem>, <reg>
xor <reg>, <mem>
xor <con>, <reg>
xor <con>, <mem>
```

Örnek vermek gerekirse;

```
and $0x0f, %eax — EAX’in son 4 biti dışında tümünü sil.
xor %edx, %edx — EDX’in içeriğini sıfıra ayarlayın
```

**not (Bitwise logical not)** — İşlenen içerikleri mantıksal olarak olumsuzdurlar (işlenendeki tüm bit değerlerini çevirir). 

Syntax;

```
not <reg>
not <mem>
```

Örnek vermek gerekirse;

```
not %eax — EAX’ın tüm parçalarını çevir.
```

**neg (Negate)** — işlenen içeriklerden ikisinin tümleyici olumsuzlanmasını gerçekleştirir.

Syntax;

```
neg <reg>
neg <mem>
```

Örnek vermek gerekirse;

```
neg %eax — EAX, (- EAX) olarak ayarlandı
```

**shl, shr (Shift left and right)** — Bu talimatlar, ilk işlenen içeriğindeki bitleri sola ve sağa dayandırarak sonuçta ortaya çıkaran boş bit konumlarını sıfırlarla doldurur. Kaydırılan işlenen 31 yere kadar kaydırılabilir. Kaydırılacak bit sayısı, `8 bitlik` bir sabit veya `CL` olabilen ikinci işlenen tarafından belirtilir. Her iki durumda da 31’den daha büyük kaydırma sayımları 32 modulo gerçekleştirilir.

Syntax;

```
shl <con8>, <reg>
shl <con8>, <mem>
shl %cl, <reg>
shl %cl, <mem>

shr <con8>, <reg>
shr <con8>, <mem>
shr %cl, <reg>
shr %cl, <mem>
```

Örnek vermek gerekirse;

```
shl $1, eax — EAX değerini 2 ile çarpın (en önemli bit 0 ise)
shr %cl, %ebx — EBX değerinin 2n’ye bölünmesinin sonucunun tabanını EBX’te saklayın, burada n CL’deki değerdir. Dikkat: negatif tam sayılar için, bölmenin C anabiliminden farklıdır.
```

# - Control-flow (kontrol akışı)

x86 işlemci, geçerli talimatın başladığı bellekteki konumu gösteren 32 bitlik bir değer olan bir yönerge işaretçisi (`EIP`) kaydı tutar. Normalde, bellekteki bir sonraki talimata işaret etmek için artıp, bir talimatın yürütülmesinden sonra başlar. EIP kaydı doğrudan değiştirilemiyor ancak sağlanan kontrol akışı talimatlarıyla dolaylı olarak güncellenebiliyor.

Program metninde etkili konumlara atıfta bulunmak için `<label>`gösterimi kullanılıyor. Etiketler, bir etiket adı ve ardından iki nokta üst üste girilerek x86 assembly kodu metninin herhangi bir yerine eklenebilir. Örnek olarak;

```
      mov 8(%ebp), %esi
begin:
       xor %ecx, %ecx
       mov (%esi), %eax
```       

Yukarıdaki kod parçacığında ikinci talimat begin olarak etiketlenmiştir. Kod parçacığının başka bir yerinde, daha uygun bir sembolik ad kullanarak bu komutun bellekte bulunduğu konumuna başvurabiliriz. Bu etiket, 32 bitin değeri yerine konumu ifade etmenin uygun bir yoludur.

**jmp (Jump)** — Program kontrol akışını işlenen tarafından belirtilen hafıza konumundaki talimata aktarır.

Syntax;

```
jmp <label>
```

Örnek vermek gerekirse;

```
jmp begin — begin etiketli talimata atlayın
```

**jcondition (Conditional jump)** — Bu talimatlar, makine durum (`machine status`) kelimesi adı verilen ve özel bir kayıtta saklanan bir dizi koşul kodunun durumuna dayanan koşullu atlamalardır. Makine durum kelimesinin içeriği, gerçekleştirilen son aritmetik işlemle ilgili bilgileri içerir. Örnek olarak; bu kelimenin bir biti, son sonucun sıfır olup-olmadığını gösterir. Bir diğeride son sonucun negatif olup-olmadığını gösterir. Bu koşul kodlarına bağlı olarak, bir dizi koşullu atlama gerçekleştirilebilir. 

Örnek olarak; son aritmetik işlemin sonucu sıfırsa, jz komutu belirtilen işlenen etikete bir sıçrama gerçekleştirir. Aksi takdirde, kontrol sırayla sondaki talimata ilerler. Koşullu dalların bir kısmına, sezgisel olarak gerçekleştirilen son işlemi temel alan adlar verilmiştir. (cmp gibi) 

Örneğin, `jle` ve `jne`gibi koşullu dallar, önce istenen işlenenler üzerinde bir `cmp` işlemi gerçekleştirmeye dayanır.

Syntax;

```
je <label> (eşit olduğunda atlal)
jne <label> (eşit olmadığında atlal)
jz <label> (son sonuç sıfır olduğunda atla)
jg <label> (büyük olduğunda atla)
jge <label> (büyük veya eşit olduğunda atla)
jl <label> (küçük olduğunda atla)
jle <label> (küçük veya eşit olduğunda atla)
```

Örnek vermek gerekirse;

```
cmp %ebx, %eax
jle done
```

EAX içeriği EBX’in içeriğinden daha az veya ona eşitse, yapılan etikete atlayın. Aksi takdirde, bir sonraki talimata devam edin.

**cmp (Compare)** — Makine durum (`machine status`) sözcüğündeki koşul kodlarını uygun şekilde ayarlayarak, belirtilen iki işlenenin değerini karşılaştırın. Bu talimat, ilk işleneni değiştirmek yerine çıkarma sonucunun atılması dışında alt talimata eşdeğerdir.

Syntax;

Syntax

```
cmp <reg>, <reg>
cmp <mem>, <reg>
cmp <reg>, <mem>
cmp <con>, <reg>
```

Örnek vermek gerekirse;

```
cmpb $10, (%ebx)
jeq loop
```

EBX’teki bellek konumunda depolanan byte tam sayı sabiti 10’a eşitse eğer, döngü etiketli konuma atlayın.

**call, ret (Subroutine call and return)** — Bu talimatlar, bir alt rutin (subroutine) çağrısı (`call`)  ve dönüşü (`ret`) uygular. Çağrı talimatı ilk olarak mevcut kod konumunu bellekteki donanım destekli yığın üzerine iter ve ardından etiket işleneni tarafından belirtilen kod konumuna koşulsuz bir sıçrama gerçekleştirir. Basit atlama (`jump`) talimatlarının aksine çağrı (`call`)  talimatı, alt rutin tamamlandığında geri dönülecek konumu kaybeder.

Ret talimatı bir `alt rutin` dönüş mekanizması uygular. Bu komut ilk olarak donanım tarafından desteklenen bellek içi yığınındann bir kod konumunu çıkarır. Daha sonra alınan kod konumuna bir sıçrama gerçekleştirir.

Syntax;

```
call <label>
ret
```