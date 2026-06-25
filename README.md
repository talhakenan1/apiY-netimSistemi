Merhaba,
Vaka çalışması dokümanını ve frontend tasarımlarını detaylıca inceledim.
İş ilanında da belirttiğiniz gibi "API-First" vizyonunda olmanız benim açımdan da çok uygun; çünkü ben de projelerimde koda girmeden önce her zaman işin contract bölümünü oturtmayı tercih eden birisiyim.
Özellikle farklı sektörlerdeki kobilere yaptığım 2 işte tecrübe ettiğim kadarıyla işi yaparken sürekli gelen talepler üzerine değişim yapabilmenin en önemli yolunun bu olduğunu düşünüyorum.
Tasarımı yaparken dikkat ettiğim ana mimari kararlarımı kısaca aşağıda özetlemek isterim:
1. Admin ve Son Kullanıcı (Client) Rotalarının Fiziksel Olarak Ayrılması: İlettiğim Swagger dosyasını incelerseniz, endpoint'leri /api/v1/client/* ve /api/v1/admin/* olarak iki ayrı gruba böldüğümü göreceksiniz.
2. Bunu yapmamın özel bir sebebi var: Geçmişte geliştirdiğim "Pikto CRM" ve "DijiTek" projelerimde, başlangıçta tüm kullanıcıları aynı endpoint üzerinden (sadece Role Guard'lar ekleyerek) yönetmeyi denemiştim.
Fakat proje büyüdükçe (özellikle admin'in görmek istediği fazladan istatistiksel veriler işin içine girdiğinde) aynı controller içindeki DTO'ların spagetti koda dönüştüğünü tecrübe ettim.
Bu yüzden son kullanıcılar ile yönetim panelini rotalar düzeyinde baştan ayırmak, sistemin ileride rahatça ölçeklenebilmesi için en temiz ve güvenli yol olduğunu düşünüyorum.
3. ID Yapılarında UUID Kullanımı ve Veri Güvenliği: PostgreSQL tarafında veri tiplerini belirlerken, Primary Key'ler için auto-increment Integer yerine UUID standardını tercih ettim.
Kendi yazdığım DijiTek sistemini ilk demosunda, sipariş numaralarını integer yaptığımız için rakiplerin veya kötü niyetli kullanıcıların günde kaç işlem yaptığımızı URL'deki id'leri değiştirerek
kolayca tahmin edebildiğini fark etmiştim. Bu tür durumları önlemek ve “Insecure Direct Object Reference” güvenlik açıklarının önünü tamamen kesmek için veri tiplerinde UUID kullanmak artık benim için vazgeçilmez bir standart.
5. Modülerlik ve Şemalar: Backend'de DRY prensibini uyguladığım gibi, API sözleşmesinde de bunu uygulamaya özen gösterirdim. Swagger dosyasının en altındaki components/schemas bölümünde
PaginationMeta, ErrorResponse, User gibi bütün platformların ortak kullanacağı nesneleri modüler kurgulardım. Bu sayede frontend ekibi, Swagger üzerinden kendi TypeScript interfacelerini otomatik generate ederken çok temiz bir veri tipine sahip olacaktır.
Talepleriniz doğrultusunda hazırladığım swagger.yaml dosyasını ekte iletiyorum.
İncelemeniz için teşekkürler.
İyi çalışmalar dilerim, Talha Kenan Yaylacık.
