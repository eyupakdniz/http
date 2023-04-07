# HTTP
## HTTP nedir?<br/>
HTTP, clientların ve serverların birbirleriyle iletişim kurma şeklini standartlaştıran, TCP/IP tabanlı bir uygulama katmanı iletişim protokolüdür. İçeriğin internet üzerinden nasıl talep edildiğini ve iletildiğini tanımlar. Uygulama katmanı protokolü ile, ana bilgisayarların (client ve server) nasıl iletişim kurduğunu standartlaştıran basit bir soyutlama katmanı olduğunu kastediyorum. HTTP'nin kendisi, client ve server arasında request ve response almak için TCP/IP'ye bağlıdır. Default olarak, TCP bağlantı noktası 80 kullanılır, ancak diğer bağlantı noktaları da kullanılabilir. Ancak HTTPS, 443 numaralı bağlantı noktasını kullanır.
<br/><br/>

## HTTP/0.9 - The One Liner (1991)<br/>
HTTP'nin belgelenen ilk sürümü, 1991'de ortaya atılan HTTP/0.9 idi. Şimdiye kadarki en basit protokoldü; GET adında tek bir yönteme sahip olmak. Bir müşteri sunucudaki bazı web sayfalarına erişmek zorunda olsaydı, aşağıdaki gibi basit bir istekte bulunurdu<br/>

`GET /index.html`<br/>

Ve sunucudan gelen yanıt aşağıdaki gibi görünürdü<br/>

`(response body)`<br/>
`(connection closed)`<br/>

isteğinde bulunabilirdi ve Sunucu isteği alacak, yanıt olarak HTML ile yanıt verecek ve içerik aktarılır aktarılmaz bağlantı kapatılacaktır.<br/>
* Başlık yok<br/>
+ `GET` izin verilen tek yöntemdi<br/>
- Yanıtın HTML olması gerekiyordu<br/><br/>
protokolün gerçekten de olacaklar için bir atlama taşı olmaktan başka bir şeyi yoktu.<br/><br/>

## HTTP/1.0 - 1996<br/>
HTTP/1.0 orijinal sürüme göre büyük ölçüde geliştirildi. TCP üzerine inşa edilmiştir. HTTP/0.9'dan farklı olarak HTML yanıtı için tasarlanmıştır.HTTP/1.0 ile resimler, video dosyaları, düz metin veya diğer herhangi bir içerik türü dönülebiliyor. Yeni metod eklendi(POST ve HEAD gibi), request/response formatları değiştirildi, hem request hem de response yanıtlara HTTP başlıkları eklendi, yanıt tanımlamak için status kodlar getirildi, karakter seti desteği getirildi, multi-part türler, yetkilendirme , önbelleğe alma, içerik kodlama ve daha fazlası dahil edildi.<br/>

Request<br/>
`GET / HTTP/1.0`<br/>
`Host: cs.fyi`<br/>
`User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_10_5)`<br/>
`Accept: */*`<br/>

Response<br/>
`HTTP/1.0 200 OK`<br/>
`Content-Type: text/plain`<br/>
`Content-Length: 137582`<br/>
`Expires: Thu, 05 Dec 1997 16:00:00 GMT`<br/>
`Last-Modified: Wed, 5 August 1996 15:55:28 GMT`<br/>
`Server: Apache 0.84`<br/>

`(response body)`<br/>
`(connection closed)`<br/><br/>

Request ve response başlıkları hala ASCII kodlu olarak tutuldu, ancak yanıt gövdesi herhangi bir türde olabilirdi, yani resim, video, HTML, düz metin veya başka herhangi bir içerik türü. Böylece, artık bu sunucu istemciye herhangi bir içerik türünü gönderebilir; HTTP'deki "Hyper Text" terimi değişmiş oldu.<br/><br/>

HTTP/1.0'ın en büyük dezavantajlarından biri, bağlantı başına birden çok isteğinizin olmamasıydı. Yani, bir istemci ne zaman sunucudan bir şeye ihtiyaç duysa, yeni bir TCP bağlantısı açması gerekecek ve bu tek istek yerine getirildikten sonra bağlantı kapatılacaktır. Ve bir sonraki gereksinim için, yeni bir bağlantıda olması gerekecekti. Bu çok sayıda bağlantı, performansın ciddi şekilde düşmesine neden olur.<br/><br/>

### Three-way Handshake<br/>
En basit haliyle three-way handshake, tüm TCP bağlantılarının, uygulama verilerini paylaşmaya başlamadan önce istemci ve sunucunun bir dizi paketi paylaştığı three-way handshake ile başlamasıdır.<br/>

SYN - İstemci rastgele bir sayı alır, x diyelim ve onu sunucuya gönderir.<br/>
SYN ACK - Sunucu, istemciye rasgele bir sayıdan oluşan bir ACK paketi göndererek isteği onaylar; diyelim ki sunucu tarafından alınan y ve x+1 sayısı burada x, istemci tarafından gönderilen sayıdır<br/>
ACK - İstemci, sunucudan aldığı y sayısını artırır ve y+1 numaralı bir ACK paketini geri gönderir.<br/>

Three-way handshake tamamlandıktan sonra, istemci ve sunucu arasındaki veri paylaşımı başlayabilir. İstemcinin, son ACK paketini gönderir göndermez uygulama verilerini göndermeye başlayabileceği, ancak sunucunun talebi yerine getirmek için yine de ACK paketinin alınmasını beklemesi gerekeceği unutulmamalıdır.<br/>

Ancak, HTTP/1.0'ın bazı uygulamaları, "Bağlantı: keep-alive" adlı yeni bir başlık tanıtarak bu sorunu aşmaya çalıştı. Bu başlık, sunucuya "Merhaba sunucu, bu bağlantıyı kapatma, tekrar ihtiyacım var" demek için tasarlanmıştı. Ancak yine de bu başlık çok yaygın olarak desteklenmedi ve sorun devam etti. HTTP'nin bağlantısız olmasının yanı sıra, durumsuz bir protokol olması da ayrı bir konudur. Yani, sunucu istemci hakkında bilgi saklamaz ve her bir istek, eski isteklerle hiçbir ilişkisi olmadan, sunucunun isteği yerine getirmesi için gerekli bilgiyi kendi içinde bulundurmalıdır. Bu nedenle, istemcinin açması gereken büyük sayıdaki bağlantılardan bağımsız olarak, gereksiz verileri ağda göndererek bant genişliği kullanımında artışa neden olur.<br/>


## HTTP/1.1 - 1999
HTTP/1.0 üzerine major iyileştirmeler oldu.
- PUT, PATCH, OPTIONS, DELETE'i tanıtan yeni HTTP metodlar eklendi<br/>
- Kalıcı Bağlantılar Yukarıda tartışıldığı gibi, HTTP/1.0'da bağlantı başına sadece bir istek vardı ve istek karşılandıktan hemen sonra bağlantı kapatılıyordu, bu da performans kaybına ve gecikme sorunlarına neden oluyordu. HTTP/1.1 kalıcı bağlantıları tanıttı, yani bağlantılar varsayılan olarak kapatılmadı ve ardışık çoklu isteklere izin verildi. Bağlantıları kapatmak için istekte Connection: close başlığı bulunması gerekiyordu. İstemciler genellikle bağlantıyı güvenli bir şekilde kapatmak için bu başlığı son istekte gönderirler.<br/>
- HTTP/1.1 ayrıca pipelining desteğini de tanıttı, burada istemci, aynı bağlantı üzerinde sunucudan yanıt beklemeksizin birden fazla istek gönderebilir ve sunucu istekleri alındığı sırayla yanıtı göndermek zorundaydı. Ancak ilk yanıtın indirilmesinin tamamlandığı ve sonraki yanıtın içeriğinin başladığı noktanın istemci tarafından nasıl bilineceği sorulabilir! Bu sorunu çözmek için, yanıtın nerede bittiğini ve bir sonraki yanıtın beklenebileceği yerin tespit edilebilmesi için İçerik Uzunluğu başlığı olmalıdır ve istemciler bunu kullanabilirler.<br/>
- Dinamik içerik durumunda, sunucu iletim başladığında gerçekten İçerik Uzunluğunu bulamazsa, içeriği parçalar halinde (parça parça) göndermeye başlayabilir ve her parça gönderildiğinde İçerik Uzunluğu ekleyebilir. Ve tüm parçalar gönderildiğinde, yani tüm iletişim tamamlandığında, boş bir parça gönderir, yani İçerik Uzunluğu sıfıra ayarlanmış olan bir parça gönderir, böylece istemciye iletişimin tamamlandığını bildirir. Sunucu, parçalı transfer hakkında istemciyi bilgilendirmek için Transfer-Encoding: chunked başlığını içerir.<br/>
- Yalnızca Temel kimlik doğrulaması olan HTTP/1.0'ın aksine, HTTP/1.1 özet ve proxy kimlik doğrulaması içeriyordu<br/>
- Caching<br/>
- Byte Ranges<br/>
- Character sets<br/>
- Language negotiation<br/>
- Client cookies<br/>
- Gelişmiş sıkıştırma desteği<br/>
- Yeni status kodları<br/>

Tam olarak doğru. HTTP/1.1, aynı bağlantı üzerinden birden fazla isteğin gönderilmesine izin vermek için pipelining'i tanıtarak bu sorunu çözmeye çalıştı, ancak başlık satırı engelleme sorununu tamamen çözemedi. Bu, yavaş veya ağır bir isteğin arkasındaki istekleri engellediği durumlarda oluşur ve bir istek bir boruda sıkışırsa, sonraki isteklerin yerine getirilmesini beklemek zorunda kalacaktır. HTTP/1.1'in bu eksikliklerinin üstesinden gelmek için geliştiriciler geçici çözümleri uygulamaya başladı; örneğin hareketli grafik sayfalarının kullanımı, CSS'de kodlanmış görüntüler, tek büyük CSS/Javascript dosyaları, etki alanı parçalama vb.


### SPDY - 2009
Google devam etti ve web sayfalarının gecikmesini azaltırken web'i daha hızlı hale getirmek ve web güvenliğini iyileştirmek için alternatif protokoller denemeye başladı. Bant genişliğini artırmaya devam edersek, başlangıçta ağ performansının arttığı ancak bir noktada çok fazla performans kazancı olmadığı görüldü. Ama aynısını gecikme ile yaparsanız yani gecikmeyi düşürmeye devam edersek, sürekli bir performans artışı olur. Bu, SPDY'nin arkasındaki performans kazancı için temel fikirdi, ağ performansını artırmak için gecikmeyi azaltmaktı.

SPDY'nin özellikleri arasında, multiplexing, sıkıştırma, önceliklendirme, güvenlik vb. bulunur. SPDY'nin ayrıntılarına girmeden önce, HTTP/2'nin nitty gritty'sine girdiğimizde fikrinizi alacağınızı söylediğim için, HTTP/2'nin çoğunlukla SPDY'den ilham aldığını belirtmek istiyorum. SPDY, HTTP'nin yerini almaya çalışmamıştır; uygulama katmanında var olan HTTP'nin üzerine bir çeviri katmanı olarak tasarlanmıştır ve isteği tel örgüye göndermeden önce değiştirir. SPDY, giderek standart bir hale gelmiş ve çoğu tarayıcı tarafından uygulanmaya başlanmıştır. 2015 yılında, Google'da iki rekabet eden standart istemiyorlardı, bu nedenle SPDY'yi HTTP'ye birleştirmeye ve HTTP/2'yi doğurarak SPDY'yi kullanımdan kaldırmaya karar verdiler.


## HTTP/2 - 2015
HTTP/2, içeriğin düşük gecikmeli aktarımı için tasarlanmıştır. HTTP/2, HTTP'nin performans ve güvenliğini artırmak için birkaç yeni özellik sunmuştur, bunlar şunlardır:
- Binary instead of Textual: HTTP/2, metinsel kodlamayı bırakarak ikili kodlama kullanır, böylece daha kompakt ve verimli hale gelir.<br/>
- Multiplexing: Bu özellik, bir bağlantı üzerinden birden fazla HTTP isteği ve yanıtının asenkron olarak gönderilmesine izin verir, böylece birden çok bağlantıya ihtiyaç kalmaz.<br/>
- Header compression using HPACK: HTTP/2, HTTP istek ve yanıtlarının başlık bilgilerini sıkıştırmak için HPACK sıkıştırma algoritmasını kullanır, bu da ağ üzerinden iletilen veri miktarını azaltır.<br/>
- Server Push: HTTP/2, sunucuların tek bir istek için birden çok yanıt proaktif olarak göndermesine izin verir, bu da müşteri ve sunucu arasında gereken tur sayısını azaltır.<br/>
- Request Prioritization: HTTP/2, müşterilerin tek tek isteklere öncelik atamasına izin verir, bu da sunucunun öncelikli isteklere önce yanıt vermesini sağlar.<br/>
- Security: HTTP/2, tüm bağlantılar için SSL/TLS şifrelemesinin kullanımını zorunlu kılarak, HTTP/1.1'e göre daha yüksek bir güvenlik seviyesi sağlar.<br/>


## HTTP/3 - 2022
HTTP/3, QUIC tabanlı bir protokoldür. QUIC, UDP üzerine inşa edilmiş ve TCP'nin yerini alacak şekilde tasarlanmış bir taşıma katmanı protokolüdür.Gecikmeyi azaltmak ve performansı artırmak için tasarlanmış çok katlı, güvenli, akış tabanlı bir protokoldür. TCP ve HTTP/2'nin halefidir. QUIC, gecikmeyi azaltmak ve performansı artırmak için tasarlanmış çok katlı, güvenli, akış tabanlı bir protokoldür. TCP ve HTTP/2'nin halefidir.
- Çoğullama : QUIC, çoklanmış bir protokoldür, yani tek bir bağlantı üzerinden birden çok akış gönderilebilir. Bu, tek bir bağlantı üzerinden birden çok akışın gönderilebildiği HTTP/2'ye benzer. Ancak, HTTP/2'den farklı olarak QUIC, HTTP ile sınırlı değildir. Veri akışlarının güvenilir, düzenli ve kayba dayanıklı teslimini gerektiren herhangi bir uygulama için kullanılabilir.<br/>
- Akış tabanlı : QUIC, akış tabanlı bir protokoldür; bu, verilerin akış biçiminde gönderildiği anlamına gelir. Her akış, benzersiz bir akış kimliği ile tanımlanır. QUIC, verileri her iki yönde göndermek için tek bir akış kullanır. Bu, her akışın benzersiz bir akış kimliğiyle tanımlandığı ve her akışın çift yönlü olduğu HTTP/2'ye benzer.<br/>
- Güvenilmez Datagram : QUIC, veri göndermek için güvenilmez datagramlar kullanır. Bu, QUIC'in verilerin alıcıya teslim edileceğini garanti etmediği anlamına gelir. Ancak QUIC, verilerin gönderildiği sırayla teslim edileceğini garanti eder. Bu, verilerin datagramlar biçiminde gönderildiği ve datagramların alıcıya iletilmesinin garanti edilmediği UDP'ye benzer.<br/>
- Bağlantı Geçişi : QUIC, bağlantı geçişini destekler; bu, QUIC bağlantısının bir IP adresinden başka bir IP adresine taşınabileceği anlamına gelir. Bu, bir TCP bağlantısının bir IP adresinden başka bir IP adresine taşınabildiği TCP'ye benzer.<br/>
- Kayıp Kurtarma : QUIC, paket kaybından kurtarmak için kayıp kurtarmayı kullanır. QUIC, paket kaybından kurtulmak için tıkanıklık kontrolü ve kayıp kurtarmanın bir kombinasyonunu kullanır. Bu, TCP'nin paket kaybından kurtarmak için tıkanıklık kontrolü ve kayıp kurtarmanın bir kombinasyonunu kullandığı TCP'ye benzer.<br/>
- Tıkanıklık Kontrolü : QUIC, verilerin ağ üzerinden gönderilme hızını kontrol etmek için tıkanıklık kontrolünü kullanır. QUIC, paket kaybından kurtulmak için tıkanıklık kontrolü ve kayıp kurtarmanın bir kombinasyonunu kullanır. Bu, TCP'nin paket kaybından kurtarmak için tıkanıklık kontrolü ve kayıp kurtarmanın bir kombinasyonunu kullandığı TCP'ye benzer.<br/>
- El sıkışma : QUIC, istemci ile sunucu arasında güvenli bir bağlantı kurmak için bir el sıkışma kullanır. QUIC, istemci ile sunucu arasında güvenli bir bağlantı kurmak için TLS 1.3'ü kullanır. Bu, istemci ile sunucu arasında güvenli bir bağlantı kurmak için TLS 1.2'nin kullanıldığı HTTP/2'ye benzer.<br/>
- Başlık Sıkıştırma : QUIC, başlıkların boyutunu azaltmak için başlık sıkıştırmasını kullanır. QUIC, başlıkları sıkıştırmak için HPACK'i kullanır. Bu, başlıkları sıkıştırmak için HPACK'in kullanıldığı HTTP/2'ye benzer.<br/>
- Güvenlik : QUIC, istemci ile sunucu arasında güvenli bir bağlantı kurmak için TLS 1.3'ü kullanır. Bu, istemci ile sunucu arasında güvenli bir bağlantı kurmak için TLS 1.2'nin kullanıldığı HTTP/2'ye benzer.<br/>


## DNS nedir?
DNS (Domain Name System), internet üzerindeki cihazların (bilgisayarlar, telefonlar, sunucular vb.) IP adreslerini, daha anlaşılır ve hatırlanması kolay alan adlarıyla eşleştiren bir sistemdir.

İnternet üzerinde bir web sitesine erişmek istediğinizde, o web sitesinin IP adresine ihtiyacınız vardır. Ancak IP adresleri, hatırlanması zor sayı dizileridir. Bu nedenle, kullanıcıların hatırlaması kolay olan ve anlaşılır olan alan adlarına ihtiyaçları vardır. Örneğin, www.google.com alan adı, Google'ın IP adresine dönüştürülür ve cihazlar arasındaki iletişim sağlanır.

DNS, bu alan adı-IP adresi eşleştirmesini gerçekleştirmek için kullanılır. İnternet servis sağlayıcıları (ISP'ler) genellikle kendi DNS sunucularına sahiptir ve bu sunucular, kullanıcının isteğini alır, alan adını IP adresine çevirir ve kullanıcının cihazına geri gönderir.


## Domain name nedir?
Domain name (alan adı), internet üzerinde bir web sitesinin adresidir. Bu adresler, internet protokolü (IP) adresleri ile ilişkilidir ve insanların daha kolay hatırlayabilecekleri şekilde tasarlanmıştır.

Domain name, genellikle bir web sitesinin adı ve genişletmesi (örneğin .com, .org, .net, .edu, .gov vb.) ile oluşur. Örneğin, "google.com" bir domain name'dir. Bu ad, internet üzerindeki Google'ın web sitesinin adresini temsil eder. Kullanıcılar bu adı kullanarak web sitesine erişebilirler.

Domain name'ler, her biri benzersiz olan IP adreslerine karşılık gelirler. IP adresleri, internet bağlantılı cihazların tanımlanması için kullanılan sayısal adreslerdir. Ancak, insanlar için bu sayılar zor ve hatırlanması güç olabilir. Bu nedenle, domain name'ler, internet kullanıcılarına, IP adreslerini hatırlamadan web sitelerine erişme kolaylığı sağlar.

## Web hosting nedir?
Web hosting, bir web sitesinin internet üzerinde yayınlanması için gerekli olan hizmeti sağlayan bir servistir. Bu hizmet, web sitesinin dosyalarının ve verilerinin internete erişilebilir hale getirilmesini ve web sitenizin erişilebilir olması için sunucuların sağlanmasını içerir.

Web hosting hizmeti, web sitesinin barındırıldığı sunucuların yönetimini, bakımını ve güncellemelerini içerir. Web hosting hizmeti, genellikle bir web hosting şirketi tarafından sunulur ve kullanıcılara bir web sitesinin internete erişilebilir olması için gerekli olan her şeyi sağlar.

Web hosting hizmetleri, genellikle farklı özellikler, bant genişliği, depolama alanı ve veri tabanı seçenekleri gibi farklı seviyelerde sunulur. Kullanıcılar, web sitesinin ihtiyaçlarına göre farklı web hosting planları arasından seçim yapabilirler. Örneğin, küçük bir blog sitesi, paylaşımlı bir web hosting hizmetiyle barındırılabilirken, büyük bir e-ticaret sitesi, özel bir sunucuda barındırılması gerekebilir.







## Kaynak

[HTTP article](https://cs.fyi/guide/http-in-depth)



















