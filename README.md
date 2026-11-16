# Mars Kolonisi Yaşam Destek Simülatörü (C)

C programlama dilinin temel kontrol yapıları, döngüler, aritmetik/mantıksal operatörler ve sözde rastgele sayı üretimi (`rand`) kullanılarak geliştirilmiş metin tabanlı hayatta kalma simülasyonu.

## Proje Özeti
Oyuncu, Mars'taki bir araştırma istasyonunda mahsur kalan bir mühendisi yönetir. Amaç; **Sağlık**, **Enerji**, **Oksijen**, **Su**, **Yedek Parça** ve **Modül Hasarı** parametrelerini dengeleyerek kurtarma ekibi gelene kadar **15 gün** boyunca hayatta kalmaktır.

## Kullanılan C Yapıları ve Teknik Mimari
* **Rastgelelik ve Dinamik Eşik Kontrolü:** `srand(time(NULL))` ile başlatılan `rand() % 100` üreticisi, her eylemde dinamik hesaplanan başarı eşikleriyle karşılaştırılarak kullanıcıya şeffaf zar çıktısı sunar.
* **Komut ve Zorluk Yönetimi (`switch-case`):** Büyük/küçük harf duyarsızlığı `case 'g': case 'G':` fall-through yapısıyla sağlanmıştır. Geçersiz girişler `default` bloğu ile yakalanarak tur kaybı önlenir.
* **Kaynak Dönüşümü ve Zaman Akışı (`%` Mod Operatörü):**
  * Oksijen üretiminde (`O`) girilen su miktarının tek/çift durumu `su % 2` ile denetlenir; artık su depoda bırakılır.
  * Zaman akışında `tur % 3 == 0` koşuluyla her 3 geçerli komutta bir gün ilerletilir.
* **Kum Fırtınası Simülasyonu (`for`, `continue`, `break`):** Zorluk seviyesine göre belirlenen tur sayısı boyunca çalışır. Çift turlarda (`i % 2 == 0`) hasar iki katına çıkar. Hava kilidi mühürlüyse oksijen sızıntısı `continue` ile atlanır; yaşamsal değerler sıfırlanırsa döngü `break` ile sonlandırılır.
* **Hava Kilidi Şifre Mekanizması (`do-while`):** Gün sayısına göre aritmetik olarak türetilen (`'A' + (gun % 26)`) karakter şifresi, maksimum 3 deneme ve enerji kontrolü birleştirilmiş tek bir mantıksal koşul altında doğrulanır.

## Komut Listesi
| Komut | Açıklama | Kaynak Etkisi / Mekanik |
| :--- | :--- | :--- |
| `G` | Keşif Görevi | -15 Enerji, -10 Oksijen; su veya yedek parça bulma / yaralanma riski |
| `O` | Oksijen Üretimi | -10 Enerji; her 2 birim su karşılığı +3 Oksijen üretimi |
| `T` | Tamir | -10 Enerji; hasar kademesine göre (hafif/orta/ağır) yedek parça tüketimi |
| `D` | Durum Raporu | Sistem metriklerini ve kritik uyarıları listeler (Tur harcamaz) |
| `U` | Uyu / Dinlen | +25 Enerji, -8 Oksijen; oksijen < 20 ise sağlık kaybı |
| `K` | Kum Fırtınası | Çok turlu fırtına simülasyonunu tetikler |
| `H` | Hava Kilidi | 3 haklı alfabetik şifre doğrulaması ile kilidi mühürler |
| `Q` | Çıkış | Simülasyonu sonlandırır ve sonuç raporunu ekrana basar |

## Derleme ve Çalıştırma
```bash
gcc main.c -o mars_simulator
./mars_simulator
