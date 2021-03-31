# Lab4: SQL injection UNION attack, retrieving multiple values in a single column

Bu yazıda, PortSwingger tarafından oluşturulan SQLi lablarının dördüncüsünü inceleyeceğiz.

Lab yönergesi:


This lab contains an SQL injection vulnerability in the product category filter. The results from the query are returned in the application's response so you can use a UNION attack to retrieve data from other tables.

The database contains a different table called users, with columns called username and password.

To solve the lab, perform an SQL injection UNION attack that retrieves all usernames and passwords, and use the information to log in as the administrator user.

Sorgudan elde edilen sonuçlar uygulamanın yanıtında döndürülür, böylece diğer tablolardan veri almak için bir UNION saldırısı kullanabilirsiniz.Veritabanı, kullanıcı adı ve parola adı verilen sütunlarla birlikte kullanıcılar adında farklı bir tablo içermektedir. Laboratuvarı çözmek için, tüm kullanıcı adlarını ve şifreleri alan bir SQL injection UNION saldırısı gerçekleştirmemiz ve bilgileri yönetici kullanıcı olarak oturum açmak için kullanmamız istenmektedir. Bunun için Lab1 ve Lab2’deki gibi UNION saldırı tekniklerini kullanacağız ancak sunucunun TABLE kullanıcılarının tüm kullanıcı adlarını ve şifrelerini döndürmesi için bir sorgu ekleyeceğiz.

Sırasıyla aşağıdaki adımları uyguluyoruz:

-Önce bir filtre uygularız (“Aramanızı daraltın” bölümünde belirli bir öğeye tıklayın).
![image](https://user-images.githubusercontent.com/70814577/113106848-e76e6780-920b-11eb-9436-a16725c56284.png)
-Sonra, bir sorgu olduğunda döndürülen sütunların sayısını kontrol etmek, burada ‘UNION SELECT null, null– sorgusuyla 2 sütun buluyorum.
![image](https://user-images.githubusercontent.com/70814577/113107164-459b4a80-920c-11eb-8590-a82c4a4cb9dd.png)
![image](https://user-images.githubusercontent.com/70814577/113107200-50ee7600-920c-11eb-9567-20fcd22d96dd.png)
-Gerçek sütun sayısını öğrendikten sonra, bu sütunların veri türlerini kontrol edeceğiz. ‘UNION SELECT’ abc ‘ sorgusuyla öncelikle ilk sütunun veri türünün string olup olmadığını kontrol ediyoruz. Hata alıyoruz yani ilk sütün string türünde değil. İlk satırı NULL döndürerek ikinci satır için deniyoruz.İkinci satırın sonuç verdiğini, string türünde olduğunu görüyoruz.
![image](https://user-images.githubusercontent.com/70814577/113107284-69f72700-920c-11eb-9acc-f83a4f9badd8.png)
![image](https://user-images.githubusercontent.com/70814577/113107373-85623200-920c-11eb-8ae4-7a05089e3927.png)
-'UNION SELECT 1, username||'~ '||password FROM users-- yani || ile string birleştirme yöntemi uyguluyoruz. Kullanıcı adı sütununun dönüş sonucuyla '~' (amaç, kolay görüntüleme için kullanıcı adı ve parolayı ayırmaktır) ve ardından parola sütununun dönüş sonucu '~' ile birleştiriyoruz.( ~ yerine : gibi diğer özel karakterleri de kullanabilirsiniz.)
![image](https://user-images.githubusercontent.com/70814577/113107943-251fc000-920d-11eb-8354-6289cea2a945.png)
![image](https://user-images.githubusercontent.com/70814577/113108343-8e9fce80-920d-11eb-9076-3fd6c4de2710.png)
![image](https://user-images.githubusercontent.com/70814577/113108378-99f2fa00-920d-11eb-9dc7-0bdc45549264.png)
-Sorgunun sonucu olarak sadece yöneticinin şifresini değil, aynı zamanda kullanıcılar tablosunda saklanan tüm kullanıcı adını da aldık. Son olarak, laboratuvarı tamamlamak için giriş yapın ve yöneticinin kullanıcı adını ve şifresini kullanın.
![image](https://user-images.githubusercontent.com/70814577/113108581-cb6bc580-920d-11eb-9aef-90d177edbd5d.png)
