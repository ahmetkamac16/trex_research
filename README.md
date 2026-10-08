# .NET Backend Geliştirme - Araştırma Notlarım

Bu çalışmada backend geliştirmede kullanılan temel kavramları, ne işe yaradıkları ve birbirleriyle nasıl bağlantılı oldukları üzerinden özetledim. Örneklerde çoğunlukla basit bir ürün ve sipariş uygulamasını düşündüm. Benim için önemli olan, bir kavramın tanımını bilmenin yanında hangi problemi çözdüğünü de açıklayabilmek.

## 1. Modern Yazılım Geliştirme Pratikleri

### Git ve GitHub
#### 1. Git Nedir?
Git, yazılım projelerinde yapılan değişiklikleri takip etmek için kullanılan dağıtık bir sürüm kontrol sistemidir. Bir projedeki dosyaların geçmişini saklar ve gerektiğinde önceki sürümlere dönülmesini sağlar.

Git sayesinde birden fazla yazılımcı aynı proje üzerinde çalışabilir. Her geliştirici projenin bir kopyasını kendi bilgisayarında bulundurur ve yaptığı değişiklikleri kaydeder. Böylece kimin, hangi dosyada, ne zaman ve ne tür bir değişiklik yaptığı takip edilebilir.

Git'in sağladığı başlıca avantajlar şunlardır: 
- Dosyalarda yapılan değişiklikleri kayıt altında tutar.
- Önceki sürümlere dönme imkanı sağlar.
- Birden fazla geliştiricinin aynı projede çalışmasını kolaylaştırır.
- Yeni özelliklerin ana projeyi etkilemeden geliştirilmesini sağlar.
- Hatalı değişikliklerin tespit edilmesine yardımcı olur.
- Projenin farklı sürümlerinin yönetilmesini kolaylaştırır.

Git çoğunlukla terminal komutlarıyla kullanılır. Bunun yanında Visual Studio, Visual Studio Code ve JetBrains Rider (PyCharm, IntelliJ IDEA vb.) gibi IDE'lerde grafiksel Git araçları da bulunmaktadır.

#### 2. GitHub Nedir? 
GitHub, Git ile yönetilen projelerin internet üzerinde saklanmasını ve ekip üyeleriyle paylaşılmasını sağlayan bulut tabanlı bir platformdur.

Git bir sürüm kontrol sistemi, GitHub ise Git depolarını barındıran ve ekip çalışmasını destekleyen bir hizmettir. Git internet bağlantısı olmadan yerel bilgisayarlarda kullanılabilir. GitHub ise projeyi uzaktan bir sunucuda saklamak, paylaşmak ve ekip üyeleriyle birlikte geliştirmek için kullanılır.

GitHub'ın sunduğu bazı özellikler şunlardır:
- Projelerin uzak depolarda saklanması.
- Proje kodlarının ekip üyeleri ile paylaşılması.
- Pull Request ile kod değişikliğinin incelenmesi. (Pull Request: Bir yazılımda yapılan değişikliklerin ana koda eklenmesi için proje yöneticilerine ve ekip arkadaşlarına gönderilen resmi bir birleştirme talebidir.)
- Issue sistemiyle görev ve hata takibi yapılması. (Issue: Bir yazılım projesindeki hataları, eksiklikleri, yeni özellik isteklerini veya yapılacak görevleri takip etmek için kullanılan yerleşik bir görev yönetim sistemidir.)
- Proje dokümantasyonunun tutulması.
- GitHub Actions ile CI/CD süreçlerinin oluşturulması. (GitHub Actions: Bir projedeki işleri belirli olaylar gerçekleştiğinde otomatik çalıştıran bir sistemdir.)
- Açık kaynak projelere katkıda bulunmayı kolaylaştırması.

GitHub dışında GitLab, Bitbucket ve Azure Repos gibi Git tabanlı alternatif platformlar da bulunmaktadır.

#### 3. Özetle Git ve GitHub

Git, dosyalardaki değişikliklerin geçmişini tutan dağıtık bir sürüm kontrol sistemidir. GitHub ise Git repolarını barındıran; pull request, kod incelemesi ve iş takibi gibi ekip çalışması özellikleri sunan bir platformdur. 

Git'i bilgisayarımda internet olmadan da kullanabilirim; GitHub, bu çalışmayı paylaşmamı kolaylaştırır.

### Temel Git komutları

| Komut | Ne için kullanırım? |
| --- | --- |
| `git init` | Bulunduğum klasörde Git takibini başlatırım. |
| `git clone <repo-url>` | Var olan bir repoyu geçmişiyle birlikte bilgisayarıma alırım. |
| `git add README.md` | Dosyadaki değişiklikleri bir sonraki commit için hazırlarım. |
| `git commit -m "Git notları eklendi"` | Hazırladığım değişiklikleri yerel geçmişe kaydederim. |
| `git push origin main` | Yerel commit'leri uzak repoya gönderirim. |
| `git pull` | Uzak daldaki değişiklikleri alıp mevcut dalımla birleştiririm; ayara göre merge veya rebase yapılabilir. |
| `git branch arastirma` | Yeni dal oluştururum; bu komut tek başına o dala geçirmez. |
| `git merge arastirma` | Belirtilen dalı o anda bulunduğum dala birleştiririm. |
| `git checkout -b arastirma` | Yeni dal oluşturma ve ona dala geçme yapılır. Dallar arası geçişlerde de kullanılır. |

Örneğin `git switch arastirma` ile araştırma dalına geçip çalışabilirim. `add`, `commit` ve `push` aynı işlem değildir: hazırlama, yerel kaydetme ve paylaşma adımlarıdır.

### Merge conflict

Merge Conflict, Git'in iki farklı değişikliği otomatik olarak birleştiremediği durumda oluşan çakışmadır.

Örneğin; iki geliştirici aynı dosyanın aynı satırını farklı şekilde değiştirirse Git hangi değişikliğin korunması gerektiğine kendisi karar vermez. Bu durumda geliştiricinin dosyayı inceleyerek doğru kodu seçmesi gerekir.

Bu durumda `git status` ile çakışan dosyaları bulurum; `<<<<<<<`, `=======`, `>>>>>>>` işaretlerinin bulunduğu bölümü okuyup doğru içeriği düzenlerim. Ardından işaretleri kaldırır, sonucu kontrol eder, `git add` ve merge işlemini tamamlayan `git commit` komutlarını kullanırım. Sadece kendi değişikliğimi seçmek yerine iki değişikliğin amacını anlamam gerekir.

### CI/CD ve pipeline örnekleri

**CI (Continuous Integration)**, geliştiricilerin değişikliklerini ortak kod tabanına sık sık entegre etmesi ve bu değişikliklerin otomatik derleme ve testlerle doğrulanmasıdır. 
**Continuous Delivery**, doğrulanan sürümün dağıtıma hazır tutulmasıdır; canlıya geçişte onay olabilir. 
**Continuous Deployment** ise başarılı değişikliklerin otomatik olarak canlıya dağıtılmasıdır.

**CI/CD'nin Faydaları**
- Tekrarlanan işlemler otomatikleştirilir.
- Derleme ve test hataları daha erken tespit edilir.
- Manuel işlem kaynaklı hatalar azalır.
- Yeni sürümler daha hızlı yayınlanır.
- Her değişiklik aynı standartlara göre kontrol edilir.
- Yazılım kalitesi ve ekip içindeki güven artar.
- Hatalı kodun üretim ortamına gönderilme ihtimali azalır.
- Dağıtım süreci kayıt altına alınır ve takip edilebilir hale gelir.

Bir .NET projesinde akışı (pipeline'ı) `restore → build → test → publish → deploy` şeklinde kurabilirim. `dotnet publish` dağıtım dosyalarını hazırlar; tek başına uygulamayı sunucuya kurmaz.

**.NET projesinde yaygın olarak kullanılan komutlar:**
- `dotnet restore`: Projenin ihtiyaç duyduğu NuGet paketlerini indirir.
- `dotnet build --no-restore`: Projeyi derler ve derleme hatalarını kontrol eder.
- `dotnet test --no-build`: Projedeki otomatik testleri çalıştırır.
- `dotnet publish`: Uygulamanın dağıtıma hazır dosyalarını oluşturur.


**GitHub Actions:** Aşağıdaki örnek `.github/workflows/dotnet.yml` içinde kullanılabilecek bir CI akışıdır.

```yaml
name: .NET CI
on: [push, pull_request]
permissions:
  contents: read
jobs:
  kontrol:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v6
      - uses: actions/setup-dotnet@v4
        with:
          dotnet-version: '10.0.x'
      - run: dotnet restore
      - run: dotnet build -c Release --no-restore
      - run: dotnet test -c Release --no-build
```

**Azure DevOps:** Aynı kontrolü `azure-pipelines.yml` ile tanımlayabilirim.

```yaml
trigger:
  - main
pool:
  vmImage: ubuntu-latest
steps:
  - task: UseDotNet@2
    inputs:
      packageType: sdk
      version: '10.0.x'
  - script: dotnet restore
  - script: dotnet build -c Release --no-restore
  - script: dotnet test -c Release --no-build
```

Bu örnekler kök dizinde uygun bir .NET çözümü/projesi ve test projesi olduğunu varsayar; yalnızca README bulunan bu araştırma reposunda çalıştırılacak akışlar değildir. CD için başarılı testlerden sonra API projesini `dotnet publish` ile paketleyip hedef ortama dağıtan bir aşama eklerim. Canlı ortam erişim bilgilerini YAML içine yazmam; secret veya service connection kullanırım.


### SDLC (Software Development Life Cycle), Agile, Scrum ve Kanban

SDLC, yazılımın fikirden bakım sürecine kadar olan yaşam döngüsüdür:

1. **Planlama:** Hedef, kapsam ve öncelikler belirlenir.
2. **Analiz:** Kullanıcının ihtiyacı ve kabul koşulları netleştirilir.
3. **Tasarım ve geliştirme:** Veri modeli, mimari ve kod oluşturulur.
4. **Test:** Beklenen davranışlar ve hata senaryoları kontrol edilir.
5. **Dağıtım:** Doğrulanan sürüm kullanıcıya sunulur.
6. **Bakım:** Hatalar giderilir, geri bildirimlerle ürün geliştirilir.

Yazılımcının rolünü yalnızca kod yazmak olarak görmüyorum. Gereksinimi sorgulamak, test yazmak ve canlıdaki sorunu incelemek de bu işin parçası.

- **Agile Metodolojisi**
Agile, yazılım geliştirme sürecinin küçük ve yönetilebilir parçalara ayrılmasını temel alan çevik bir çalışma yaklaşımıdır. Geleneksel yöntemlerde projenin tüm gereksinimleri başlangıçta belirlenebilir ve ürün uzun bir geliştirme sürecinin sonunda teslim edilebilir. Agile yaklaşımında ise çalışan yazılım küçük parçalar hâlinde ve sık aralıklarla kullanıcıya sunulur.

- **Scrum Metodolojisi**
Scrum, Agile yaklaşımıyla uyumlu, hafif bir çalışma çerçevesidir. Proje, sprint adı verilen kısa çalışma dönemlerine ayrılır. Sprint, bir ay veya daha kısa süren sabit uzunlukta bir çalışma dönemidir. Sprint sonunda kullanılabilir bir ürün artışı hedeflenir; ekip ilerlemeyi düzenli olarak gözden geçirir.

- **Kanban Metodolojisi** 
Kanban, işlerin görsel olarak takip edilmesini ve iş akışının sürekli iyileştirilmesini amaçlayan bir yöntemdir. Görevler genellikle bir Kanban panosu üzerinde gösterilir. 

  Basit bir pano şu sütunlardan oluşabilir: 

  Yapılacak → Devam ediyor → Test ediliyor → Tamamlandı

  Yazılımcı, üzerinde çalıştığı görevin durumunu panoda günceller. Böylece ekip üyeleri hangi işlerin beklediğini, devam ettiğini veya tamamlandığını kolayca görebilir. 

  Yazılımcı olarak panoyu güncel tutmanın yanında devam eden iş sayısını (WIP) sınırlamaya, tıkanıklıkları görünür kılmaya ve başladığım işi bitirmeye dikkat ederim.

Planlı ürün geliştirmede Scrum, sürekli gelen bakım taleplerinde Kanban bana daha uygun başlangıçlar gibi geliyor; seçim ekibin çalışma biçimine bağlıdır.

## 2. .NET Ekosistemi

### .NET nedir, neden kullanılır?

.NET, farklı türlerde uygulamalar geliştirmek ve çalıştırmak için çalışma zamanı (runtime), kütüphaneler ve geliştirme araçlarından oluşan bir yazılım platformudur. Microsoft ve açık kaynak topluluğu tarafından geliştirilen bu yapıda; C# projeleri yazmaya yarayan programlama dili, .NET ise bu kodun derlenmesini, yürütülmesini ve yönetilebilmesini sağlayan ekosistemdir. Sahip olduğu temel sınıf kütüphaneleri (BCL), otomatik bellek yönetimi (Garbage Collector) ve ASP.NET Core gibi modern çatıları sayesinde platformlar arası (cross-platform), güvenli ve yüksek performanslı backend sistemleri geliştirmeyi kolaylaştırır.

2002'de .NET Framework ile başlayan yapı, 2016'da .NET Core ile platformlar arası bir yöne ilerledi. 2020'de .NET 5 ile ürünün adından “Core” kaldırıldı. Bu çizgi .NET 6, 7, 8, 9 ve 10 ile devam etti.

### .NET Framework, .NET Core ve modern .NET

| Özellik | .NET Framework | .NET Core | .NET 7/8 ve sonrası |
| --- | --- | --- | --- |
| Konum | Eski Windows uygulamalarında yaygın | Modern .NET'in 1.x–3.x dönemindeki adı | .NET Core'un devamı |
| Platform | Windows | Windows, Linux, macOS | Windows, Linux, macOS |
| Web tarafı | Klasik ASP.NET | ASP.NET Core | ASP.NET Core |
| Tercih | Mevcut Framework bağımlılıklarını sürdürmek | Eski Core projelerini anlamak | Yeni projede desteklenen sürümü seçmek |

Platform desteği, her uygulama türünün her yerde çalışacağı anlamına gelmez. Örneğin WPF/Windows Forms Windows'a bağlıdır; bir ASP.NET Core API ise uygun bağımlılıklarla Linux üzerinde de çalışabilir.

### `dotnet --info` çıktısı ve yorumu

Bu komut, .NET kurulumu ve bilgisayar ortamı hakkında bilgi verir. Sürüm ve yol bilgileri bilgisayara göre değişir.

Çalışma ortamında alınan gerçek çıktının kısaltılmış hali:

```text
.NET SDK:
 Version: 10.0.401

Çalışma Zamanı Ortamı:
 OS Name: Windows
 RID: win-x64

.NET SDKs installed:
 10.0.301
 10.0.401

.NET runtimes installed:
 Microsoft.AspNetCore.App 10.0.9
 Microsoft.AspNetCore.App 10.0.12
 Microsoft.NETCore.App 10.0.9
 Microsoft.NETCore.App 10.0.12
 Microsoft.WindowsDesktop.App 10.0.9
 Microsoft.WindowsDesktop.App 10.0.12

global.json file:
 Not found
```

SDK geliştirme ve derleme araçlarını, runtime uygulamayı çalıştıran bileşenleri gösteriyor. `win-x64`, ortamın Windows ve 64 bit olduğunu belirtiyor. Birden fazla sürüm birlikte kurulabiliyor. Bu konumda `global.json` bulunmadığından projeye özel SDK seçimi yapılmamış; komutun seçtiği SDK `10.0.401`.

### Senkron ve Asenkron programlama

#### Senkron Programlama
Senkron işlemde çağrılan işlem tamamlanmadan çağıran kod sonraki adıma geçmez. Örneğin bir dosya okunacaksa:
- Dosya okuma başlar.
- Çağıran iş parçacığı işlem tamamlanana kadar bekler.
- Dosya okunduktan sonra sonraki satır çalışır.

Kısa ve basit işler için anlaşılır bir yaklaşımdır. Uzun süren işlemler kullanıcı arayüzünün iş parçacığında yapılırsa arayüzün geç yanıt vermesine (donmasına) neden olabilir.

#### Asenkron Programlama
Asenkron yaklaşımda, tamamlanmamış bir işlemin sonucu beklenirken çağıran iş parçacığı bloke olmaz. Örneğin sunucudan veri bekleyen bir masaüstü uygulamasında kullanıcı arayüzü bu sırada etkileşimlere yanıt vermeye devam edebilir. 

Asenkronluk; işlemin kendiliğinden daha hızlı biteceği veya mutlaka ayrı bir iş parçacığında çalışacağı anlamına gelmez. Senkron bir çağrıda thread sonuç gelene kadar kilitli beklerken, asenkron I/O'da bekleme esnasında thread serbest bırakılarak başka istekleri karşılamak üzere havuza döner. Bu yapı sorgu süresini kısaltmaz; sistem kaynaklarının bekleme süresini çok daha verimli yönetmesini sağlar. Özellikle ağ, dosya ve veritabanı gibi giriş/çıkış (I/O) işlemlerinde faydalıdır.

```csharp
// EF Core kullanılan bir metot içinde:
var urunler = db.Urunler.ToList();             // Senkron
var urunlerAsync = await db.Urunler.ToListAsync(); // Asenkron
```

- `async`, metotta `await` kullanılabilmesini sağlar; tek başına yeni thread açmaz.
- `Task`, işlemin tamamlanmasını; `Task<T>`, buna ek olarak sonucunu temsil eder.
- `await`, işlem bitmediyse thread'i bloke etmeden tamamlanmasını bekler.
- `ConfigureAwait(false)`, devamın yakalanan bağlama dönmesini istemediğimi belirtir. Genel amaçlı kütüphanelerde anlamlı olabilir; ASP.NET Core'da varsayılan özel bir `SynchronizationContext` olmadığından her satıra eklenmesi gereken bir performans kuralı değildir.

Benim için tipik örnek, sipariş kaydedilirken veritabanı cevabını asenkron beklemektir. Yoğun bir hesaplama ise CPU işidir; yalnızca `async` yazmak onu paralel yapmaz. 

### C#'ta `=>` ifadesi

JavaScript'teki “arrow function” ifadesine benzese de C#'ta `=>`, lambda ve expression-bodied üye yazımında kullanılır; asenkronluk anlamına gelmez.

```csharp
Func<int, int> ikiKat = sayi => sayi * 2; // Lambda
decimal KdvEkle(decimal fiyat) => fiyat * 1.20m; // Kısa metot gövdesi
```
### `=>` ile async kullanımı

=> tek başına asenkronluk sağlamaz. Bir lambda ya da kısa metot gövdesini asenkron yapmak için başına async anahtar kelimesi eklenir ve dönüş tipi Task / Task<T> olur. Gövdede await kullanılabilir.

```csharp
// Asenkron lambda
Func<int, Task<int>> ikiKatAsync = async sayi =>
{
    await Task.Delay(100); // Örnek bekleme
    return sayi * 2;
};

// Asenkron kısa metot gövdesi
async Task<string> SayfaGetirAsync(string url) =>
    await httpClient.GetStringAsync(url);

// Kullanım
int sonuc = await ikiKatAsync(5);          // 10
string html = await SayfaGetirAsync("https://ornek.com");
```

Bu örneklerde =>, sol taraftaki parametreleri sağ taraftaki gövdeye bağlayarak yazımı kısaltır; asenkronluğu sağlayan ise başa eklenen async anahtar kelimesidir. ikiKatAsync lambdasında await Task.Delay(100) satırı, iş parçacığını kilitlemeden 100 ms bekler ve ardından sayi * 2 sonucunu Task<int> olarak döndürür. SayfaGetirAsync metodunda ise =>, süslü parantez ve return yazmadan gövdeyi tek satırda tanımlar; metot, sayfa içeriğini indirirken programın geri kalanının çalışmasına izin verir. Kullanım kısmındaki await ifadeleri, görevler tamamlanana kadar bekleyip Task içindeki gerçek değeri değişkenlere aktarır; await olmasaydı değişkenlere sonuç yerine henüz bitmemiş bir Task nesnesi atanırdı.

## 3. Backend Geliştirme Temelleri

### Backend, frontend, web sunucusu ve API

#### Backend Nedir?

Backend, bir uygulamanın sunucu tarafında çalışan bölümdür. Kullanıcıdan gelen istekleri işler, uygulamanın kurallarını uygular, veritabanıyla iletişim kurar ve gerekli sonuçları kullanıcıya gönderir.

Örneğin bir alışveriş uygulamasında backend şu işlemleri gerçekleştirir:
- Kullanıcının giriş bilgilerini kontrol eder.
- Ürünleri ve stok bilgilerini veritabanından getirir.
- Sipariş oluşturur.
- Kullanıcının hangi işlemleri yapabileceğini kontrol eder.

#### Frontend Nedir?

Frontend, kullanıcının gördüğü ve etkileşim kurduğu bölümdür. Sayfa tasarımı, menüler, butonlar ve formlar frontend kapsamında yer alır.

Bir giriş ekranı üzerinden farkı şöyle açıklayabiliriz:
- Kullanıcı, frontend üzerindeki forma e-posta ve şifresini girer.
- Frontend, bu bilgileri backend'e gönderir.
- Backend, bilgileri kontrol eder ve giriş sonucunu döndürür.
- Frontend, sonuca göre kullanıcıya mesaj gösterir veya ilgili sayfayı açar.

> Frontend geliştirmede HTML, CSS ve JavaScript; Backend geliştirmede ise JavaScript/Node.js, Python, Java, C# ve PHP gibi teknolojiler kullanılır.

#### Web Sunucusu Nedir?

Web sunucusu; istemcilerden gelen HTTP isteklerini karşılayıp yanıtlayan yazılımdır (aynı zamanda bu yazılımın çalıştığı sunucu donanımını da tanımlar). ASP.NET Core mimarisinde istekleri karşılayan ve işleyen temel yerleşik sunucu ise Kestrel'dir.

İstemci, genellikle bir tarayıcı veya mobil uygulamadır. Kullanıcı bir web adresini açtığında istemci sunucuya bir istek gönderir. Sunucu da HTML sayfası, görsel, dosya veya JSON verisi gibi bir yanıt döndürür.

Nginx, Apache ve IIS yaygın web sunucusu örnekleridir.

#### API Nedir? 

API (Application Programming Interface), Türkçesiyle Uygulama Programlama Arayüzü, yazılımların birbiriyle belirlenmiş kurallar üzerinden iletişim kurmasını sağlar.

Örneğin bir hava durumu uygulaması, başka bir sistemin API'sine şehir bilgisi göndererek sıcaklık verisini alabilir.

Bir API'nin belirli bir işlem için sunduğu adrese endpoint, yani uç nokta denir.
>https://api.ornek.com/urunler
Bu adres, ürünlerle ilgili işlemlere erişmek için kullanılabilir.

Erişim Durumlarına Göre API Türleri: 
- Açık API: Dış geliştiricilerin kullanımına sunulur. Yine de API anahtarı veya ücretli erişim gerektirebilir.
- Özel API: Bir kurumun kendi uygulamaları veya ekipleri tarafından kullanılır.
- İş Ortağı API'si: Belirli şirketlere veya iş ortaklarına erişim sağlar.

İletişim Yaklaşımına Göre API Türleri:
- REST: Kaynaklara odaklanan bir mimari yaklaşımdır.
- SOAP: XML tabanlı mesajlarla çalışan bir iletişim protokolüdür.
- GraphQL: İstemcinin istediği veri alanlarını belirtmesini sağlayan bir sorgu dili ve çalışma sistemidir.

API kavramı yalnızca web servislerini kapsamaz; işletim sistemleri ve yazılım kütüphaneleri de API sunabilir.

### HTTP ve temel metotlar

HTTP - Hypertext Transfer Protocol, istemci ile sunucu arasındaki istek/yanıt protokolüdür. HTTPS, bu iletişimi TLS ile korur.

| Metot | Örnek | Amaç |
| --- | --- | --- |
| GET | `/api/urunler/5` | Ürün bilgisini okumak |
| POST | `/api/urunler` | Gönderilen bilgilerle ürün oluşturmak |
| PUT | `/api/urunler/5` | Ürünün güncellenebilir temsilini bütünüyle değiştirmek |
| DELETE | `/api/urunler/5` | Ürünü silmek |

GET, PUT ve DELETE'in tekrarı aynı amaçlanan sunucu durumunu korur; buna **idempotency** denir. Yanıt kodlarının aynı olması gerekmez. POST'ta tekrar istek yeni kayıt oluşturabilir. Kısmi güncelleme için PATCH kullanılabilir. 

#### GET - Veri Okuma
Bir kaynağı veya kaynak listesini almak için kullanılır.

**GET /urunler/15** (Bu istek, kimliği 15 olan ürünün bilgilerini ister.)

Örnek yanıt:

```json
{
  "id": 15,
  "ad": "Klavye",
  "fiyat": 1200
}
```

#### POST - Veri Gönderme ve Kaynak Oluşturma
Genellikle yeni bir kaynak oluşturmak veya sunucuda bir işlem başlatmak için kullanılır.

**POST /urunler**

```json
{
  "ad": "Mouse",
  "fiyat": 650
}
```

İstek başarıyla işlendiğinde adı Mouse ve fiyatı 650 olan bir ürün oluşturulabilir.

#### PUT - PATCH Kullanımı ve Farkı 
Ürünün fiyatını 1500 yapmak istediğimiz zaman PUT ve PATCH şu şekilde kullanılır:

**PUT /urunler/15**

```json
{
  "ad": "Mekanik Klavye",
  "fiyat": 1500,
  "stok": 20
}
```

**PATCH /urunler/15**

```json
{
  "fiyat": 1500
}
```
PUT ile baştan revize et, PATCH ile sadece istenilen veriyi değiştir. Buradaki PATCH gövdesi, API bu biçimi destekliyorsa anlamlıdır.


#### DELETE - Kaynak Silme
Belirli bir kaynağı silmek için kullanılır.

**DELETE /urunler/15** (Bu istek, kimliği 15 olan ürünü silmeyi talep eder.)

#### Yaygın HTTP Durum Kodları

- 200: İstek başarıyla işlendi.
- 201: Yeni bir kaynak oluşturuldu.
- 204: İşlem başarılı; yanıt gövdesi yok.
- 400: İstek hatalı veya geçersiz.
- 401: Kimlik doğrulaması gerekiyor.
- 403: İstemcinin işlem için yetkisi yok.
- 404: İstenen kaynak bulunamadı.
- 500: Sunucuda beklenmeyen bir hata oluştu.

### RESTful Servislerin Mantığı

REST'te ürün gibi kaynakları URL ile tanımlar, işlemi HTTP metoduyla belirtirim. İstekler kendi başına anlaşılabilmelidir; sunucunun önceki isteğe ait istemci oturum bağlamına dayanması gerekmez. Bu **stateless** olma kuralı, sunucuda veritabanı bulunamayacağı anlamına gelmez. Kaynaklar, standart metotlar ve uygun durum kodları API'yi daha anlaşılır yapar.

### OpenAPI Nedir?

OpenAPI, bir API'nin nasıl kullanılacağını tanımlar. Hangi adreslere istek gönderilebileceğini, hangi bilgilerin gerektiğini ve nasıl yanıt alınacağını YAML veya JSON formatında açıklar.

### Swagger Nedir?

Swagger OpenAPI tanımlarını oluşturmak, görüntülemek ve test etmek için kullanılan araçlar ailesidir. Sık karşılaşılan Swagger araçları şunlardır:
- Swagger Editör: OpenAPI dosyasını yazmana ve tanımdaki hataları görmemize yardımcı olur.
- Swagger UI: Tanımı okuyup endpoint'leri, parametreleri ve yanıt örneklerini tarayıcıda gösterir.
- Swagger Codegen: API tanımından istemci kodu ve sunucu iskeleti üretmek için kullanılır.

### JSON

JSON (JavaScript Object Notation), veri alışverişinde kullanılan metin tabanlı bir formattır:

```json
{
  "id": 5,
  "ad": "Klavye",
  "fiyat": 850.50,
  "stoktaVar": true,
  "etiketler": ["bilgisayar", "aksesuar"]
}
```

Burada nesne içinde sayı, metin, boolean ve dizi var. Alan adları çift tırnakla yazılır. JSON'u frontend ile backend arasında ortak bir veri gösterimi olarak düşünüyorum; tek başına bir API veya veritabanı değildir.

### REST, SOAP ve GraphQL Karşılaştırması

| Yaklaşım | Temel özellik | Örnek tercih |
| --- | --- | --- |
| REST | Mimari stildir; kaynaklar ve HTTP metotları öne çıkar, genellikle JSON kullanılır. | Standart ürün/sipariş API'si |
| SOAP | XML mesajları kullanan bir protokoldür; WSDL ve WS-* standartlarıyla kullanılabilir. | SOAP sözleşmesi gerektiren kurumsal entegrasyon |
| GraphQL | Sorgu dili ve çalışma modelidir; istemci istediği alanları seçer. | Ekranların farklı veri birleşimlerine ihtiyaç duyması |

Ben basit bir CRUD uygulamasında REST ile başlardım. GraphQL'de gereksiz veri aktarımı azalabilir ama sorgu maliyeti ve yetkilendirme yine dikkat ister.

## 4. ASP.NET

### ASP.NET
Klasik ASP.NET, .NET Framework üzerinde Windows odaklı web geliştirme teknolojisidir. C# kullanarak internet siteleri, yönetim panelleri ve API’ler geliştirmeyi sağlar.

"Klasik ASP.NET" denildiğinde genellikle .NET Framework üzerinde çalışan ASP.NET 4.x kastedilir. Bu tarafta Web Forms, ASP.NET MVC 5 ve Web API 2 gibi teknolojiler bulunur. 

### ASP.NET Core
ASP.NET Core modern .NET üzerinde çalışan, platformlar arası ve modüler web çatısıdır. Yerleşik DI, middleware yapısı ve API geliştirme desteği işimi kolaylaştırır. Windows, Linux ve macOS üzerinde çalışılabilir.

### Öne Çıkan Avantajları
- Platform Bağımsızlığı: Farklı işletim sistemlerinde çalışır.
- Performans: Yüksek performanslı Kestrel sunucusunu kullanır.
- Modüler Yapı: HTTP işlem hattı uygulamanın ihtiyacına göre düzenlenebilir.
- Yerleşik DI: Bağımlılıkları yönetmek için hazır bir altyapı sunar.
- Test Edilebilirlik: Bileşenleri ayrı ayrı test etmeyi kolaylaştırır.
- Bulut ve Container Uyumu: Farklı dağıtım ortamlarında kullanılabilir.

### Temel Farklar
- Çalışma Ortamı: Klasik ASP.NET, .NET Framework ve Windows temellidir; ASP.NET Core modern .NET üzerinde farklı platformlarda çalışır.
- İstek İşleme: Klasik ASP.NET'te HTTP modülleri ve handler'lar yaygındır. ASP.NET Core middleware işlem hattını kullanır.
- Bağımlılık Yönetimi: ASP.NET Core yerleşik bir DI container sunar.
- Web Geliştirme Modeli: Klasik tarafta MVC ve Web API ayrı çatılardı; Core tarafında controller tabanlı web sayfaları ve API'ler aynı MVC altyapısını paylaşır.

### MVC

MVC, sorumlulukları **Model**, **View** ve **Controller** olarak ayırır. Model veri ve davranışı temsil eder, View arayüzü üretir, Controller isteği alıp uygun akışı başlatır. Örneğin ürün listesini Controller hazırlatır, View gösterir. Bir Web API'de HTML View yerine JSON döndürülebilir; iş kurallarını Controller içinde büyütmemeye çalışırım.

### Middleware ve sıralama

Middleware, HTTP isteği işlenirken devreye giren bileşendir. İsteği sonraki bileşene iletebilir veya yanıtı kendisi tamamlayabilir. Aşağıdaki sıra, ilgili servis kayıtları yapılmış bir uygulama içindir:

```csharp
app.UseExceptionHandler("/hata"); // Bu adresi karşılayan bir hata endpoint'i gerekir.
app.UseHttpsRedirection();
app.UseRouting();
app.UseAuthentication();
app.UseAuthorization();
app.MapControllers();
```

Hata yakalayıcı başta olduğunda sonraki bileşenlerden gelen hataları yakalayabilir. Routing uç noktayı belirler; authentication kimliği oluşturur, authorization erişimi kontrol eder. Bu yüzden kimlik doğrulama yetkilendirmeden önce gelmelidir. Yanıt, çağrı zincirinde geri döner. Modern projelerde bunlar `Program.cs`, eski düzenlerde `Startup.Configure` içinde görülebilir.

### Background Worker 

Background worker, bir uygulamada işleri arka planda yapan yardımcı süreçtir. Kullanıcı uygulamayı kullanmaya devam ederken zaman alan görevleri yürütür.
Örneğin bir sitede “Rapor oluştur” düğmesine bastığında:
1. Uygulama rapor isteğini bir iş kuyruğuna ekler.
2. Background worker bu işi alıp raporu hazırlar.
3. İş bitince rapor indirilebilir hâle gelir veya sana bildirim gönderilir.
E-posta gönderme, video işleme ve dosya dönüştürme gibi görevlerde kullanılır. Böylece uygulama, uzun süren işlerin bitmesini beklemeden yanıt verebilir.

### Background Job

Background job, bir olay, kullanıcı işlemi veya belirlenmiş zaman tarafından tetiklendiğinde arka planda çalışan görevdir. Kullanıcının uygulamayı kullanmasını engellemeden e-posta gönderme, rapor hazırlama veya yedekleme gibi işleri gerçekleştirir.
Önceki kavramla ilişkisi şöyle:
- Background job: Yapılacak iş.
- Background worker: O işi yapan süreç.
Örneğin “Bu kullanıcıya hoş geldin e-postası gönder” bir job’dır. Kuyruktan bu görevi alıp e-postayı gönderen süreç ise worker’dır.

### Dependency Injection (DI)

DI'da sınıf, bağımlılığını kendi içinde `new` ile üretmek yerine dışarıdan alır. Böylece gerçek servisi testte sahte bir servisle değiştirmek kolaylaşır.

```csharp
var builder = WebApplication.CreateBuilder(args);
builder.Services.AddScoped<IFiyatService, FiyatService>();
var app = builder.Build();

app.MapGet("/api/fiyat", (IFiyatService servis) => servis.KdvEkle(100m));
app.Run();

public interface IFiyatService
{
    decimal KdvEkle(decimal fiyat);
}

public class FiyatService : IFiyatService
{
    public decimal KdvEkle(decimal fiyat) => fiyat * 1.20m;
}
```

Endpoint somut sınıfı oluşturmaz; DI container gerekli nesneyi sağlar. `Transient` her çözümlemede, `Scoped` scope başına (web'de genellikle istek başına), `Singleton` uygulamanın container'ı boyunca bir örnek üretir. Yaşam süresini sadece hız için seçmem; özellikle `DbContext` gibi scoped bir nesneyi singleton içinde tutmam.

Katmanlı bir uygulamadaki tipik çağrı akışı:

```text
HTTP isteği → Controller → Service → Repository / DbContext → Veritabanı
              sunum       iş kuralı       veri erişimi
```

### ABP Framework

ABP Framework, C# ve ASP.NET Core üzerine kurulmuş, özellikle kurumsal web uygulamaları geliştirmeyi kolaylaştıran açık kaynaklı bir yazılım altyapısıdır. Sık tekrarlanan teknik işleri hazır sunarak geliştiricinin uygulamanın asıl işlevlerine odaklanmasını sağlar.

Başlıca sundukları:
- Kullanıcı ve yetki yönetimi: Giriş, roller ve izinler.
- Modüler yapı: Uygulamayı bağımsız geliştirilebilen parçalara ayırma.
- Çok kiracılı yapı (multi-tenancy): Aynı uygulamayı birden fazla müşteri şirketin kullanabilmesi.
- Ortak teknik servisler: İşlem kayıtları, arka plan görevleri, önbellekleme ve veri erişimi.
- Mimari düzen: Karmaşık iş kurallarını yönetmek için katmanlı mimari ve Domain-Driven Design (DDD) desteği.

## 5. Veritabanı ve ORM

### SQL ve veritabanı türleri

SQL, ilişkisel veritabanlarında veri sorgulamak ve düzenlemek için kullanılan dildir. İlişkisel veritabanları veriyi tablolar, anahtarlar ve ilişkilerle düzenler; SQL Server ve PostgreSQL örnektir. NoSQL sistemler belge, anahtar-değer veya graf gibi farklı modeller kullanır; MongoDB ve Redis buna örnektir. NoSQL'i “ilişki veya şema hiç yok” şeklinde düşünmüyorum; veri modeli ve tutarlılık özellikleri ürüne göre değişir.

Ürün, müşteri ve sipariş ilişkilerinin belirgin olduğu bir uygulamada ilişkisel veritabanı benim için anlaşılır bir başlangıçtır.

### ORM, Entity Framework Core ve DbContext

ORM (Object-Relational Mapping), uygulamadaki nesnelerle veritabanı kayıtları arasında eşleme sağlar. EF Core, .NET için bu işi yapan araçlardan biridir. `DbContext`, sorgulama, değişiklik takibi ve kayıt işlemlerinin merkezidir. `DbSet<Urun>` ürünleri sorgulamamı sağlar; `SaveChangesAsync()` izlenen değişiklikleri veritabanına yazar.

```csharp
using Microsoft.EntityFrameworkCore;

public class MagazaDbContext(DbContextOptions<MagazaDbContext> options)
    : DbContext(options)
{
    public DbSet<Urun> Urunler => Set<Urun>();
}

public class Urun
{
    public int Id { get; set; }
    public string Ad { get; set; } = "";
    public decimal Fiyat { get; set; }
}
```

Bu sınıflara ek olarak veritabanı sağlayıcısı ve bağlantı ayarı gerekir. Örneğin SQL Server için `AddDbContext<MagazaDbContext>(o => o.UseSqlServer(connectionString))` kaydı yapılır. Aynı `DbContext` üzerinde eşzamanlı işlemler çalıştırmam; context thread-safe değildir.

#### DbContext nasıl kullanılır?


```csharp

// DbContext, DI üzerinden servise constructor ile enjekte edilir.
public class UrunServisi(MagazaDbContext db)
{
    // Ekleme: Add ile nesne takibe alınır, SaveChangesAsync ile veritabanına yazılır.
    public async Task UrunEkleAsync(string ad, decimal fiyat)
    {
        db.Urunler.Add(new Urun { Ad = ad, Fiyat = fiyat }); // Değişiklik takibe alınır
        await db.SaveChangesAsync();                          // INSERT çalışır
    }

    // Güncelleme: Takip edilen nesnede yapılan değişiklik SaveChangesAsync ile yazılır.
    public async Task FiyatGuncelleAsync(int id, decimal yeniFiyat)
    {
        var urun = await db.Urunler.FindAsync(id);            // Kayıt bulunur ve takibe alınır
        if (urun is null) return;

        urun.Fiyat = yeniFiyat;                               // Değişiklik takip edilir
        await db.SaveChangesAsync();                          // UPDATE çalışır
    }

    // Okuma: DbSet üzerinden sorgu yazılır.
    public Task<List<Urun>> HepsiniGetirAsync()
        => db.Urunler.ToListAsync();                          // SELECT çalışır
}
```


### LINQ ve SQL karşılıkları

LINQ, C# içinde veri sorgulamamı sağlayan yapıdır. Bellekteki koleksiyonlarda da kullanılabilir; EF Core'da desteklenen ifadeler sağlayıcı tarafından SQL'e çevrilir.

```csharp
var urunler = await db.Urunler
    .Where(u => u.Fiyat > 500)
    .OrderBy(u => u.Fiyat)
    .Select(u => new { u.Ad, u.Fiyat })
    .ToListAsync();
```

Mantıksal SQL karşılığı:

```sql
SELECT Ad, Fiyat
FROM Urunler
WHERE Fiyat > 500
ORDER BY Fiyat;
```

`Where` filtreler, `Select` alan seçer, `OrderBy` sıralar. `ToListAsync` sonucu getirir; önceki adımlar sorguyu kurar. Üretilen SQL'in ayrıntıları sağlayıcıya göre değişebilir.

| LINQ örneği | SQL'deki anlamı |
| --- | --- |
| `CountAsync()` | `COUNT(*)` ile sayma |
| `AnyAsync(u => u.Fiyat > 500)` | `EXISTS` mantığıyla kayıt var mı kontrolü |
| `FirstOrDefaultAsync(u => u.Id == 5)` | Filtreleyip en fazla bir kayıt alma; yoksa varsayılan değer |
| `OrderBy(u => u.Id).Skip(10).Take(10)` | Sıralı sonuçta `OFFSET/FETCH` veya `LIMIT/OFFSET` ile sayfalama |
| `GroupBy(u => u.Ad).Select(g => new { Ad = g.Key, Adet = g.Count() })` | `GROUP BY Ad` ve `COUNT(*)` ile gruplama |

### Code-First ve Database-First

| Yaklaşım | Başlangıç | Ne zaman düşünürüm? |
| --- | --- | --- |
| Code-First | C# modelleri ve yapılandırmalar; migration ile şema değişiklikleri | Veri modelini uygulamayla birlikte geliştirdiğim yeni projede |
| Database-First | Mevcut veritabanından model ve context üretme (scaffolding) | Hazır veya başka ekipçe yönetilen bir veritabanında |

Code-First'te de oluşturulan migration'ı kontrol ederim; yanlış bir şema değişikliği veri kaybettirebilir. Database-First'te yeniden üretilecek dosyalara özel kod eklerken ezilme riskini düşünürüm.

### Dört temel SQL sorgusu

```sql
SELECT Id, Ad, Fiyat FROM Urunler WHERE Fiyat > 500;
INSERT INTO Urunler (Ad, Fiyat) VALUES ('Klavye', 850.50);
UPDATE Urunler SET Fiyat = 900 WHERE Id = 5;
DELETE FROM Urunler WHERE Id = 5;
```

Sırasıyla okuma, ekleme, güncelleme ve silme yapılıyor. `Id` alanının otomatik üretildiğini varsaydım. `UPDATE` ve `DELETE` içinde `WHERE` unutulursa tüm satırlar etkilenebilir. Kullanıcıdan gelen değerleri SQL metnine birleştirmek yerine parametreli sorgu kullanırım.

## 6. Güvenlik ve Performans

### Authentication ve authorization

**Authentication**, kullanıcının kim olduğunu doğrular. **Authorization**, o kullanıcının hangi işlemi yapabileceğini belirler (Kullanıcı yetkisi). Giriş yapmış bir müşterinin başka müşterinin siparişini görememesi bir yetkilendirme konusudur; yalnızca giriş kontrolü yeterli değildir.

### JWT (JSON Web Token)

JWT, claim adı verilen bilgileri taşıyan token formatıdır. Yaygın imzalı JWT biçimi `header.payload.signature` şeklindedir. Header algoritma gibi bilgileri, payload kullanıcı/rol/süre gibi claim'leri, signature ise bütünlük ve kaynak doğrulamasını sağlar.

Kullanıcı giriş yapar. Sunucu bilgileri doğrular, token'ı imzalayıp istemciye verir. İstemci access token'ı `Authorization: Bearer ...` başlığıyla gönderir. API; imzayı, süreyi, issuer ve audience değerlerini doğrular, ardından yetki kontrolü yapar. Payload'ın Base64URL ile kodlanması şifreleme değildir; içine parola koymam. Her access token JWT olmak zorunda değildir.

### OAuth, OAuth 2.0, OpenID ve OpenIddict
OAuth, bir uygulamanın kullanıcı adına başka bir servise erişebilmesi için geliştirilmiş bir yetkilendirme protokolüdür. Buradaki asıl amaç, kullanıcının parolasını o uygulamayla paylaşmadan sınırlı bir erişim izni verebilmesidir.

OAuth 1.0 ve OAuth 2.0 bu protokolün iki farklı nesli. OAuth 2.0, 1.0 ile geriye dönük uyumlu değil. 1.0'da her isteğin imzalanması gerekirken 2.0 bunun yerine HTTPS ve bearer token kullanıyor, ayrıca farklı senaryolar için farklı akışlar tanımlıyor. Şunu da belirtmek gerekiyor: OAuth 2.0 tek başına bir kullanıcı giriş protokolü değil, sadece yetkilendirme yapıyor.

Kimlik doğrulama tarafını OpenID Connect (OIDC) üstleniyor. OIDC, OAuth 2.0'ın üzerine kimlik doğrulama katmanı ekliyor ve bunun için ID token kullanıyor. Eski OpenID (OpenID 2.0) ile OIDC'yi aynı şey olarak düşünmüyorum. Eski OpenID, OAuth'tan bağımsız eski bir kimlik doğrulama protokolüydü ve bugün yerini büyük ölçüde OIDC'ye bırakmış durumda.

OpenIddict ise bir protokol değil, .NET için yazılmış bir kütüphane grubu. OAuth 2.0 ve OIDC için sunucu, istemci ve token doğrulama bileşenleri sağlıyor.

"Başka bir hesapla giriş yap" gibi bir senaryoda kullanıcının kimliğini OIDC ile, API'ye erişimi ise access token ile ilişkilendiririm. ID token'ı API'ye erişmek için access token yerine kullanmam.

Kısacası aralarındaki ilişki şöyle: OAuth 2.0 yetkilendirmeyi sağlıyor, OIDC bunun üzerine kimlik doğrulamayı ekliyor, OpenIddict de bu ikisini .NET'te uygulamamızı sağlayan kütüphane oluyor.

### Performans için kullanabileceğim teknikler

- **AsNoTracking:** Sadece okunacak EF Core sorgularında değişiklik takibini kapatır; takip maliyetini azaltır.
- **Alan seçme ve sayfalama:** Tüm tabloyu almak yerine `Select`, `Skip`, `Take` ile gereken veriyi getiririm; sorguya uygun indeksleri de incelerim.
- **IAsyncEnumerable:** Veriyi asenkron olarak parça parça tüketmeyi sağlar. Tüm sonuçları listeye almamak bellek kullanımını azaltabilir; veri kaynağı ve sağlayıcı yine tamponlama yapabilir.
- **Caching / Redis:** Sık okunan ve az değişen veriyi önbellekte tutarım. Redis, birden fazla uygulama örneğinin ortak önbelleği olabilir; süre dolumu ve veri değişince temizleme stratejisi gerekir.
- **Profiling:** Önce yavaşlığın sorgudan mı, CPU'dan mı, dış servisten mi geldiğini ölçerim. Ölçmeden cache eklemek yanlış noktayı iyileştirebilir.

### OWASP Top 10

Burada **2025 listesini** esas aldım. Bunlar tek tek açık adlarından çok risk kategorileridir.

| Risk | Kısa açıklamam |
| --- | --- |
| A01 - Broken Access Control | Kullanıcının yetkisi dışındaki kayda veya işleme erişmesidir. Sipariş sahipliği sunucuda kontrol edilmelidir. |
| A02 - Security Misconfiguration | Hatalı izinler veya açık debug ayarlarıdır. Canlı ortam yapılandırması kontrol edilmelidir. |
| A03 - Software Supply Chain Failures | Paket, araç veya dağıtım zincirinin güvenliğinin bozulmasıdır. Bağımlılıklar ve kaynakları denetlenmelidir. |
| A04 - Cryptographic Failures | Hassas verinin yetersiz korunmasıdır. HTTPS ve uygun parola hash mekanizmaları kullanılmalıdır. |
| A05 - Injection | Girdinin komut gibi yorumlanmasıdır. SQL injection ve XSS bu kapsamda düşünülür. |
| A06 - Insecure Design | Güvenliğin tasarımda eksik kalmasıdır. İş akışının kötüye kullanım senaryoları da düşünülmelidir. |
| A07 - Authentication Failures | Giriş ve oturum mekanizmalarındaki zayıflıklardır. MFA ve deneme sınırları riski azaltır. |
| A08 - Software or Data Integrity Failures | Yazılımın veya verinin doğruluğuna kontrolsüz güvenmektir. Güncellemelerin ve gelen verinin bütünlüğü doğrulanmalıdır. |
| A09 - Security Logging and Alerting Failures | Saldırı belirtilerinin kaydedilmemesi veya fark edilmemesidir. Güvenlik olaylarına anlamlı alarm üretilmelidir. |
| A10 - Mishandling of Exceptional Conditions | Beklenmeyen durumların güvensiz işlenmesidir. Hata oluşunca erişim kontrolü atlanmamalıdır. |

**SQL injection**, girdinin sorgu yapısını değiştirmesidir; parametreli sorgularla önlerim. **XSS**, saldırgan içeriğinin tarayıcıda kod olarak çalışmasıdır; çıktı kodlama kullanırım, güvenilmeyen içeriği `Html.Raw` ile basmam. HTML kabul ediliyorsa uygun bir sanitizer gerekir.

**CSRF**, tarayıcının otomatik gönderdiği cookie gibi kimlik bilgileri kullanılarak kullanıcı adına istek yaptırılmasıdır. Cookie tabanlı, durum değiştiren işlemlerde antiforgery doğrulaması kullanırım. **Broken authentication**, zayıf giriş/oturum yönetimidir; güvenli parola saklama, MFA ve oturum süresi kontrolleriyle azaltılır.

ASP.NET Core'da DTO ve model validation ile alanların biçimini/sınırlarını doğrular, authorization policy ile yetkiyi kontrol ederim. Model validation tek başına SQL injection veya XSS koruması sağlamaz; her riskin önlemi ayrıdır.

## 7. Logging ve Hata Yönetimi

### Loglama ve log seviyeleri

Loglar, uygulamada ne olduğunu sonradan anlamamı sağlar. Hataları ayıklamak, uygulamayı canlı ortamda izlemek ve güvenlik olaylarını takip etmek için kullanılır. ASP.NET Core'da `ILogger<T>` ortak arayüzdür; console gibi provider'lar kayıtların nereye yazılacağını belirler.

Log seviyesi, bir kaydın önem derecesini belirtir. `appsettings.json` içinde minimum seviye belirleyerek hangi kayıtların yazılacağını filtreleyebilirim; örneğin canlı ortamda `Information` altındaki kayıtları kapatabilirim.

```json
{
  "Logging": {
    "LogLevel": {
      "Default": "Information",
      "Microsoft.AspNetCore": "Warning"
    }
  }
}
```

| Seviye | Kullanım |
| --- | --- |
| Trace | Çok ayrıntılı akış takibi |
| Debug | Geliştirme sırasında teşhis bilgisi |
| Information | Sipariş oluşturulması gibi normal, anlamlı olay |
| Warning | Uygulama sürse de dikkat isteyen durum |
| Error | Bir işlemin başarısız olması |
| Critical | Uygulamanın çalışmasını ciddi etkileyen hata |
| None | Loglamayı kapatan filtre değeri |

Örneğin `logger.LogInformation("Sipariş oluşturuldu: {SiparisId}", id)` kullanırım. İsimli alanlar aramayı kolaylaştırır. Parola ve access token gibi bilgileri loglamam.

### Global exception handling ve UseExceptionHandler

Her metoda aynı `try/catch` bloğunu eklemek yerine beklenmeyen HTTP işlem hatalarını merkezi bir noktada ele alabilirim. Aşağıdaki örnek bağımsız bir `Program.cs` akışıdır:

```csharp
var builder = WebApplication.CreateBuilder(args);
var app = builder.Build();

app.UseExceptionHandler(handler => handler.Run(async context =>
{
    // UseExceptionHandler hatayı framework loglarına da kaydeder.
    await Results.Problem(
        statusCode: 500,
        title: "İşlem sırasında beklenmeyen bir hata oluştu.")
        .ExecuteAsync(context);
}));

app.MapGet("/api/durum", (ILogger<Program> logger) =>
{
    logger.LogInformation("Durum kontrolü yapıldı.");
    return Results.Ok(new { durum = "çalışıyor" });
});

app.MapGet("/api/hata", IResult () =>
{
    throw new InvalidOperationException("Test hatası");
    // UseExceptionHandler bu hatayı yakalar ve istemciye 500 problem yanıtını döner.
  
});

app.Run();
```

`UseExceptionHandler`, sonraki HTTP bileşenlerinden gelen işlenmemiş hataları ele alır. Örneğin `/api/hata` isteğinde fırlatılan hata endpoint'te yakalanmadığı için bu middleware'e ulaşır. İstemciye stack trace yerine standart bir problem yanıtı dönerim; teknik ayrıntıyı sunucu loglarında incelerim. Geçersiz giriş için `400`, bulunamayan ürün için `404` döndürmek gibi beklenen sonuçları ise doğrudan ele alırım. Bu middleware, ayrı çalışan worker hatalarını kapsamaz.

## 8. Yazılım Geliştirme Prensipleri

### SOLID

| Prensip | Anladığım ve kısa örneğim |
| --- | --- |
| **S - Single Responsibility** | Bir sınıfın değişmek için tek temel nedeni olmalı. Sipariş kurallarını `SiparisService`, e-posta gönderimini `EmailService` yönetir. |
| **O - Open/Closed** | Yeni davranış eklerken çalışan kodu sürekli değiştirmemeliyim. Yeni indirim türü için mevcut hesaplayıcıyı büyütmek yerine `IIndirim` uygulaması ekleyebilirim. |
| **L - Liskov Substitution** | Alt tür, üst türün beklenen sözleşmesini bozmamalı. Salt okunur depoyu, `Kaydet` sözü veren bir arayüze bağlayıp çağrıda hata fırlatmamalıyım. |
| **I - Interface Segregation** | Sınıf, kullanmadığı işlemlere bağımlı olmamalı. Okuma ve yazma ihtiyaçlarını `IUrunOkuyucu` ve `IUrunYazici` ile ayırabilirim. |
| **D - Dependency Inversion** | İş kuralları somut altyapıya değil soyutlamalara dayanmalı. Sipariş servisi `SmtpEmailService` yerine `IBildirimGonderici` alabilir. |

DI, son prensibi uygulamamı kolaylaştırır; fakat sadece DI kullanmak tasarımın otomatik olarak SOLID olduğu anlamına gelmez.

### Singleton, Repository ve Factory

- **Singleton:** Tek örneği paylaştırır. Değiştirilebilir ortak durum varsa eşzamanlı erişimi düşünmem gerekir; kullanıcıya özel veriyi singleton serviste saklamam.
- **Repository:** Veri erişimini belirli işlemler arkasında toplar. Örneğin `SiparisleriMusteriyeGoreGetir` sorgusunu soyutlayabilir. EF Core zaten benzer sorumluluklar sunduğundan, sadece her `DbSet` metodunu tekrar eden bir katmanı zorunlu görmüyorum.
- **Factory:** Nesne oluşturma kararını bir noktada toplar. Ödeme yöntemine göre uygun `IOdemeSaglayici` uygulamasını seçmek buna örnektir.

### Clean Code

Clean Code'u, kodu bir sonraki okuyanın niyetini kolayca anlayabilmesi olarak görüyorum. Anlamlı isimler, küçük metotlar, gereksiz tekrarları azaltmak ve erken dönüş kullanmak buna yardımcı olur.

```csharp
// Koşulun anlamı belirsiz:
if (s > 0 && a) { /* ... */ }

// Aynı niyet daha açık:
bool satinAlinabilir = stokAdedi > 0 && urunAktif;
if (!satinAlinabilir)
    return;
```

Yorumlarda kodun zaten söylediğini tekrarlamak yerine iş kuralının nedenini açıklarım. Kısa kod ile anlaşılır kod her zaman aynı şey değildir.

### Yazılım mimarileri

| Mimari | Temel yaklaşım | Hangi durumda düşünürüm? |
| --- | --- | --- |
| Layered | Sunum, iş kuralları ve veri erişimini katmanlara ayırır. | Küçük/orta ölçekli, anlaşılır bir iş uygulaması |
| Clean Architecture | İş kurallarını merkezde tutar, bağımlılıkları içeri yönlendirir. | İş kuralları kapsamlı ve uzun ömürlü projeler |
| Microservices | İş yeteneklerini bağımsız dağıtılabilen servislere böler. | Ekiplerin ve bileşenlerin bağımsız ölçeklenmesi/dağıtılması gerektiğinde |
| Event-Driven | Bileşenler olay yayımlayıp tüketerek haberleşir. | Sipariş sonrası fatura/bildirim gibi bağımsız süreçlerde |
| Hexagonal | Uygulama çekirdeğini portlar ve adaptörlerle dış sistemlerden ayırır. | Veritabanı, ödeme veya mesajlaşma altyapısını değiştirmeyi/test etmeyi kolaylaştırmak için |
| Onion Architecture | Amacı uygulamanın temel mantığını veritabanı, arayüz ve dış servislerden bağımsız tutmaktır. | Karmaşık iş kurallarında ve uygulamanın uzun süre geliştirilmesi beklendiğinde |
| Monolit | Farklı işlevlerin tek bir bütün olarak geliştirilip çalıştırılır. | Yeni bir projeye başlarken ve ekip küçükse tercih ederim |

### Katmanlı mimari: Service ve Repository

**Presentation**, HTTP isteğini ve yanıtını yönetir. **Business**, sipariş verilebilir mi gibi kuralları uygular. **Data Access**, veritabanı işlemlerini yürütür. Service iş akışını koordine eder; Repository veri erişimini soyutlar. Örneğin stok kontrolü ve sipariş oluşturma akışı Service'te, sipariş kaydına erişim Repository'de yer alabilir.

### Clean Architecture ve bağımlılık yönü

**Domain**, temel varlıkları ve iş kurallarını; **Application**, kullanım senaryolarını ve ihtiyaç duyulan arayüzleri; **Infrastructure**, EF Core/e-posta gibi teknik uygulamaları; **API**, HTTP girişini barındırır.

```text
Kaynak kod bağımlılıkları:

API ─────────────→ Application ─────────────→ Domain
Infrastructure ──→ Application / Domain
```

Buradaki ok “bağımlıdır” anlamına gelir. Clean Architecture’da bağımlılıklar **dış katmanlardan iç katmanlara** yönelir. Ödevde “dışa akması” denmiş olsa da bu ilkeyi içe yönelme olarak ele aldım. Application, Infrastructure’daki somut sınıfı bilmez; kendi tanımladığı arayüzü kullanır. Infrastructure bu arayüzü uygular, API tarafında da DI kayıtlarıyla eşleştirilir. Böylece iş kuralları teknik ayrıntılara bağımlı olmaz. Burada çalışma anındaki çağrı akışıyla kaynak kod bağımlılık yönünü birbirinden ayırmak gerekir.

Bu ayrımların benim için amacı katman sayısını artırmak değil, bir değişikliğin etkilediği alanı daraltmak. Veritabanı veya e-posta servisi değiştiğinde sipariş kurallarını yeniden yazmamam, tasarımın sağladığı somut faydadır.


