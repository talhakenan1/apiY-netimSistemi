# 🚀 Ayrınılı Proje Dokümantasyonu

Merhaba, vaka çalışması dokümanını ve frontend tasarımlarını detaylıca inceledim. İş ilanında da belirttiğiniz gibi **"API-First"** vizyonunda olmanız benim açımdan da çok uygun; çünkü ben de projelerimde koda girmeden önce her zaman işin *contract* bölümünü oturtmayı tercih eden birisiyim. 

Özellikle farklı sektörlerdeki KOBİ'lere yaptığım 2 işte tecrübe ettiğim kadarıyla; işi yaparken sürekli gelen talepler üzerine değişime adapte olabilmenin en önemli yolunun bu olduğunu düşünüyorum. 

Tasarımı yaparken dikkat ettiğim ana mimari kararlarımı kısaca aşağıda özetlemek isterim:

### 1️⃣ Admin ve Son Kullanıcı (Client) Rotalarının Fiziksel Olarak Ayrılması
İlettiğim Swagger dosyasını incelerseniz, endpoint'leri `/api/v1/client/*` ve `/api/v1/admin/*` olarak iki ayrı gruba böldüğümü göreceksiniz. 

**Neden?** 
Geçmişte geliştirdiğim **"Pikto CRM"** ve **"DijiTek"** projelerimde, başlangıçta tüm kullanıcıları aynı endpoint üzerinden (sadece Role Guard'lar ekleyerek) yönetmeyi denemiştim. Fakat proje büyüdükçe (özellikle admin'in görmek istediği fazladan istatistiksel veriler işin içine girdiğinde) aynı controller içindeki DTO'ların spagetti koda dönüştüğünü tecrübe ettim. Bu yüzden son kullanıcılar ile yönetim panelini rotalar düzeyinde baştan ayırmak, sistemin ileride rahatça ölçeklenebilmesi (scalability) için en temiz ve güvenli yoldur.

### 2️⃣ ID Yapılarında UUID Kullanımı ve Veri Güvenliği
PostgreSQL tarafında veri tiplerini belirlerken, Primary Key'ler için auto-increment Integer yerine `UUID` standardını tercih ettim. 

**Neden?** 
Kendi yazdığım **DijiTek** sisteminin ilk demosunda, sipariş numaralarını integer yaptığımız için rakiplerin veya kötü niyetli kullanıcıların günde kaç işlem yaptığımızı URL'deki id'leri değiştirerek kolayca tahmin edebildiğini fark etmiştim. Bu tür durumları önlemek ve **Insecure Direct Object Reference (IDOR)** güvenlik açıklarının önünü tamamen kesmek için veri tiplerinde UUID kullanmak artık benim için vazgeçilmez bir standarttır.

### 3️⃣ Modülerlik ve Şemalar (Reusability)
Backend'de DRY (Don't Repeat Yourself) prensibini uyguladığım gibi, API sözleşmesinde de bunu uygulamaya özen gösterdim. 

Swagger dosyasının en altındaki `components/schemas` bölümünde `PaginationMeta`, `ErrorResponse`, `User` gibi bütün platformların ortak kullanacağı nesneleri modüler kurguladım. Bu sayede frontend ekibi, Swagger üzerinden kendi TypeScript interface'lerini otomatik generate ederken çok temiz bir veri tipine sahip olacaktır.

---

Talepleriniz doğrultusunda hazırladığım `case_swagger.yaml` dosyası repoda mevcuttur. İncelemeniz için teşekkürler.

İyi çalışmalar dilerim,  
**Talha Kenan Yaylacık**
