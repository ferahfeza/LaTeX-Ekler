# ekler Paketi: Türkçe Otomatik Ek Oluşturucu

**Sürüm:** 2.0  
**Yazar:** Ali İhsan Çanakoğlu

`ekler` paketi, metin içerisindeki çapraz başvurulara (denklem, şekil, tablo, bölüm vb.) Türkçe dilbilgisi kurallarına, ses uyumuna ve ünsüz benzeşmesine (fıstıkçı şahap) uygun olarak doğru son eklerin **otomatik olarak** eklenmesini sağlar.

---

## 1. Giriş ve Motivasyon

### Neden Bu Pakete İhtiyacımız Var?
Normal şartlarda LaTeX'te (5) numaralı bir denkleme atıf yaparken sonuna "te" eki getirerek `(5)'te` yazmanız gerekir. Ancak belgenize ilerleyen aşamalarda yeni bir denklem eklediğinizde ve bahsettiğiniz denklemin numarası (6) olduğunda, daha önce el ile yazdığınız ek hatalı olarak `(6)'te` şeklinde kalacaktır. Sizin bunu tek tek bulup `(6)'da` olarak düzeltmeniz gerekir. Uzun tezlerde veya makalelerde bu durum ciddi bir zaman kaybı ve hata kaynağıdır.

### Çözüm: `ekler` Paketi
Bu paket `expl3` (LaTeX3) programlama altyapısı sayesinde referans numarasının son hanesini analiz eder ve doğru eki otomatik olarak yerleştirir. **En büyük avantajı**, diğer alternatiflerin aksine pdfLaTeX, XeLaTeX ve LuaLaTeX dâhil olmak üzere tüm motorlarla tam uyumlu çalışmasıdır.

## 2. Kurulum ve Hızlı Başlangıç

### Kurulum
Paketi kullanmak için `ekler.sty` dosyasını derleme yaptığınız klasöre (ana `.tex` dosyanızın yanına) kopyalamanız yeterlidir. Dilerseniz yerel `texmf` dizininize de kurabilirsiniz.

### Kullanım
Belgenizin başlangıç (preamble) kısmında paketi çağırın:
```latex
\usepackage{ekler}
```
Paket arka planda Türkçe karakterlerin ve `babel` paketinin özel karakterleriyle (örneğin eşittir veya iki nokta işaretiyle) oluşabilecek çakışmaları otomatik olarak engeller. `hyperref` paketiyle tam uyumludur; ekler otomatik olarak tıklanabilir bağlantının bir parçası olur.

## 3. Komutlar ve Açıklamaları

Pakette referansın türüne göre (parantezli mi parantezsiz mi olacağına bağlı olarak) iki farklı komut ailesi bulunmaktadır.

### Denklem / Eşitlik Referansları (Parantezli)
Denklem numaraları genellikle parantez içinde yazılır. Aşağıdaki komutlar, numarayı parantez içine alır ve uygun eki kesme işareti ile ekler.

* **`\ede{etiket}`** : Bulunma Hâli (*-de, -da, -te, -ta*)
  > *Örnek çıktı:* **(1)'de**, **(3)'te**, **(6)'da**
* **`\eden{etiket}`** : Ayrılma Hâli (*-den, -dan, -ten, -tan*)
  > *Örnek çıktı:* **(1)'den**, **(3)'ten**, **(6)'dan**
* **`\edeki{etiket}`** : Aitlik Eki (*-deki, -daki, -teki, -taki*)
  > *Örnek çıktı:* **(1)'deki**, **(3)'teki**, **(6)'daki**

### Bölüm, Şekil ve Tablo Referansları (Parantezsiz)
Şekil, tablo veya bölümlere atıf yaparken parantez kullanılmaz. Aşağıdaki komutlar doğrudan sayının yanına kesme işareti ile uygun eki getirir.

* **`\fde{etiket}`** : Bulunma Hâli (*-de, -da, -te, -ta*)
  > *Örnek çıktı:* **1'de**, **3'te**, **6'da**
* **`\fden{etiket}`** : Ayrılma Hâli (*-den, -dan, -ten, -tan*)
  > *Örnek çıktı:* **1'den**, **3'ten**, **6'dan**
* **`\fdeki{etiket}`** : Aitlik Eki (*-deki, -daki, -teki, -taki*)
  > *Örnek çıktı:* **1'deki**, **3'teki**, **6'daki**

## 4. Kapsamlı Örnekler

Aşağıdaki örnek, denklemlere ve şekillere atıf yaparken arka planda hiçbir ek ayarlaması yapmanıza gerek kalmadığını gösterir.

```latex
\begin{equation}
E = mc^2 \label{eq:enerji}
\end{equation}

Eşitlik \ede{eq:enerji} kütle-enerji eşdeğerliği ifade edilir. 
Bu formülün detayları için Bölüm \fden{sec:detay} yararlanabilirsiniz.
```

### Otomatik Test Tablosu
Paketin 1'den 10'a kadar tüm durumlarda ses uyumlarını nasıl sorunsuzca hallettiğini gösteren tablo:

| Sayının Okunuşu | Beklenen Durum | Bulunma Hâli (`\ede`) | Ayrılma Hâli (`\eden`) |
| :--- | :--- | :---: | :---: |
| **Bir** | İnce + Titreşimli | (1)'de | (1)'den |
| **İki** | İnce + Ünlü | (2)'de | (2)'den |
| **Üç** | İnce + Sert Ünsüz (ç) | (3)'te | (3)'ten |
| **Dört** | İnce + Sert Ünsüz (t) | (4)'te | (4)'ten |
| **Beş** | İnce + Sert Ünsüz (ş) | (5)'te | (5)'ten |
| **Altı** | Kalın + Ünlü | (6)'da | (6)'dan |
| **Yedi** | İnce + Ünlü | (7)'de | (7)'den |
| **Sekiz** | İnce + Titreşimli | (8)'de | (8)'den |
| **Dokuz** | Kalın + Titreşimli | (9)'da | (9)'dan |
| **On** | Kalın + Titreşimli | (10)'da | (10)'dan |

*Tablodan da görüleceği üzere `3'te` (sertleşme), `6'da` (kalınlaşma), `9'da` (kalınlaşma) gibi Türkçenin tüm istisnai durumları kusursuzca ele alınmıştır.*

## 5. Sıkça Sorulan Sorular

* **Paket hangi LaTeX motorlarında çalışır?**
  `ekler` paketi hiçbir özel motor sınırlaması olmadan, standart `pdfLaTeX`, modern `XeLaTeX` ve `LuaLaTeX` motorlarının hepsinde sorunsuz çalışır.

* **Sınıf (class) bağımlılığı var mıdır?**
  Hayır. `article`, `book`, `report` gibi tüm standart ve özel belge sınıflarıyla uyumludur.

* **`hyperref` paketindeki bağlantılara renk uygulanır mı?**
  Evet. `\ede` komutu gibi komutlar arka planda standart `\ref` komutunu çağırdığı için belgenizde `hyperref` tanımlıysa ekler ve numaralar üzerine tıklanabilir (linklenebilir) hale gelir.
