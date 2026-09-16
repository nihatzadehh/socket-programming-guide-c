# Network Foundations & Protocols
Mövzumuz `socket programming` olduğuna görə, giriş səviyyəsində şəbəkə təməllərindən danışmağımız qaçınılmazdır. Bu modulda C ilə soket proqramlaşdırmasına giriş etməyin üçün lazım olan şəbəkə baza biliklərini ələ alacağıq.

## IP Addresses & Types
Böyük ehtimalla IP ünvanlarının nə olduğunu bilirsən, amma yenə də təməl bölməsində bu başlığa toxunmaq istədim. IP ünvanları OSI modelinin 3-cü qatında — Network (Şəbəkə) səviyyəsində kommunikasiyanı təmin edir.

Kompüterindən göndərdiyin hər hansı bir paketin istər dünyanın digər ucundakı, istərsə də yan otaqdakı kompüterə ən qısa və optimal yolla çatmasını təmin edən Internet Protocol məhz bu IP ünvanı ilə işləyir. Sadə dillə desək, IP ünvanı şəbəkədəki cihazların şəxsiyyət vəsiqəsidir.

## IPv4 vs IPv6
IPv4: 32-bitlik ünvanlama sistemidir. 4 ədəd 8-bitlik oktetdən ibarətdir (məsələn: 192.168.1.1). Hər oktet 0-255 arası bir ədədə uyğundur. Ümumilikdə təxminən 4.3 milyard (2^32) unikal ünvan yarada bilir. İnternetin ilk illərində yaradılan bu adresləmənin nə vaxtsa Dünyadakı təxminən 40 milyard cihazı kodlaşdırmaqda yetərsiz qalacağı heç kimin ağlına gəlməmişdi. Dünyada internetə qoşulan cihazların sayı bu limiti aşdığı üçün IPv6-ya keçid prosesi başlayıb.

IPv6: 128-bitlik ünvanlama sistemidir və 16-lıq (hexadecimal) say sistemində yazılır (məsələn: 2001:0db8:85a3::8a2e:0370:7334). Bu metodla 3,4*10^38 sayda adres yaratmaq olar. Bu sayda adres nəvələrinin nəvələri üçün belə yetərli olacaqdır.

## Public vs Private IP Addresses
Yaxşı, indi belə bir sual verə bilərsən ki, `IPv4 addressing` bu qədər cihazı kodlaşdırmaq üçün kifayət deyilsə, necə olur ki bu gün də IPv4 adreslərdən istifadə edə bilirik? Sualın cavabı `Public` və `Private` IP adreslərindədir.

### Private IP Addresses
Əlindəki cihazın şəbəkə interfeysi məlumatlarına baxsan, gördüyün adres mütləq şəkildə aşağıdakı aralıqlardan birinə aid olacaq:
    10.0.0.0 - 10.255.255.255
    172.16.0.0 – 172.31.255.255
    192.168.0.0 – 192.168.255.255
Bax o gördüyün adres, sənin `Private IP Adress`-indir. Yaxşı bəs nədir `Private` adres?

Private IP adresi sənin evindəki və ya ofisindəki routerin lokal şəbəkəndəki cihazları bir-birindən fərqləndirmək üçün onlara verdiyi IP adresləridir. Bu IP adresləri yuxarıda göstərilən `Class`-lardan birinə aid olmalıdır. Lokal şəbəkəndən qlobal internetə çıxan bütün cihazların göndərdiyi paketlər router tərəfindən dəyişdirilərək (bu prosesə `NAT` deyilir) tək bir IP adresi ilə qlobala çıxır. Bax bu adres `Public` adresdir.

### Public IP Addresses
`Public` IP adresi, lokaldakı cihazın qlobala çıxdığı adresdir. Eyni `private` IP adresi fərqli lokal şəbəkələrdə müxtəlif cihazlarda istifadə oluna bilər. Lakin `public` ip adresi unikaldır və sənin provayderin tərəfindən təmin olunur. Provayderin özünə isə bu adreslər IANA (Internet Assigned Numbers Authority) və ya RIR-lər (Regional Internet Registry) tərəfindən icarəyə verilir. Ərazidəki hər modemə görə bir IP adresi ayrılmaya bilər, əsasən provayderinin özü də öz IP adreslərinə qənaət etmək üçün bir neçə lokal şəbəkəni birləşdirən modemləri ilə özünün ayrı bir lokal şəbəkəsini yaradır və bu şəbəkəyə bir `public` adres verir. Bu texnologiya özü CGNAT (Carrier-Grade NAT) adlanır.

## Ports
İndi isə gəlin soket proqramlaşdırması üçün ən azı IP adresləri qədər vacib olan `port` anlayışına keçək. `Portlar` verdiyi mənaya uyğun olaraq müəyyən IP adresinə təyin olunmuş cihaza gələn data paketinin hansı proqrama aid olduğunu müəyyən etməyimizi təmin edir. Yəni, bizə necə olur ki, kompüterə gələn data hansı prosesə aid olduğunu bilir sualının cavabını `portlar` verir. Qısaca bir IP adresini bir bina bloku, `portları` isə blokdakı mənzillərin qapı nömrələri kimi düşünə bilərsən. TCP/UDP protokollarında standart olaraq 65536 port vardır və bunlardan 0-1023 intervalında olanlar (Well-Known Ports) sadəcə əməliyyat sistemi tərəfindən adminstrativ funksiyalar üçün istifadə olunur. Bizim yazacağımız və digər `third party` proqramlar adətən 1024-49151 və üzəri `portları` (Registered Ports) istifadə edir.

## TCP & UDP
IP adreslərini istifadə edən protokol olan `IP Protocol` sadəcə datanın bir cihazdan digərinə ən uyğun marşrut ilə çatmasını (host-to-host communication) təmin edərkən, datanın hansı prosesə çatdırılması və hansı metodla ötürülməsi (process-to-process communication) isə OSI modelinin (indilik nə olduğunu boş verə bilərsən, amma internetdən araşdırsan yaxşı olardı) `Transport Layer`-ində yerləşən `TCP` və `UDP` adlandırdığımız protokollarla təyin olunur.

### TCP Protocol
`TCP Protocol` (Transmission Control Protocol) - şəbəkə üzərindən məlumatın etibarlı, ardıcıl və xətasız ötürülməsini təmin edən `connection-oriented` (bağlantı yönümlü) protokoldur. Əsas xüsusiyyəti, məlumatların itki olmadan, etibarlı şəkildə ünvana çatdırılmasıdır. Bağlantı yaradılarkən `3-Way Handshake` (3 addımlı əl sıxma) dediyimiz proses baş verir və cihazlar bir-birinin kommunikasiyaya uyğun olduğunu aydınlaşdırır. Sürət baxımından `UDP Protocol`-dan yavaş işləyir, səbəbi isə `UDP`-dən fərqli olaraq göndərilən paketlərin əksiksiz ünvana çatıb-çatmadığını yoxlaması və paket sırasını təmin etməsidir. `TCP` bağlantısı yaratmağımız üçün bizə lazım olan `socket` tipi `Stream Socket`-lərdir (SOCK_STREAM).

### UDP Protocol
`UDP Protocol` (User Datagram Protocol) - `TCP`-nin əmisi oğlu olub, şəbəkə üzərindən məlumatın minimum ləngimə (latency) və maksimum sürətlə ötürülməsini təmin edən `connectionless` (bağlantısız) bir protokoldur. Bağlantısızdır, yəni, `TCP` kimi paketləri göndərməzdən əvvəl qarşı tərəflə `3-Way Handshake` etmir. Özünü "Mənə nə var ki? Gəlib dərsimi deyib maaşımı alıb gedirəm, qulaq asmasanız siz itirəcəksiniz, ay bədbəxtlər!" deyən müəllim kimi aparır. `TCP`-dən fərqli olaraq çox sürətlidir, çünki məlum olduğu kimi göndərdiyi paketlərin ünvana çatıb-çatmadığını yoxlamır. Əsasən canlı yayım və online oyunlar kimi sürət tələb edən və gecikməyə səbəb olunmamalı olan yerlərdə istifadə olunur. `UDP` bağlantımız üçün bizə lazım olan `socket` tipi `Datagram Socket`-dir (SOCK_DGRAM).
