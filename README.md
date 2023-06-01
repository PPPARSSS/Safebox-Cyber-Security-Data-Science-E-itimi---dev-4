# Safebox-Cyber-Security-Data-Science-E-itimi---dev-4
Safebox Cyber Security Data Science Eğitimi - Ödev 4

1)Stored Procedure ve Trigger  neden kullanılır?  olumlu ve olumsuz etkileri nelerdir?

Stored Procedure (Depolanmış Prosedür): Bu, veritabanında saklanan ve tekrar tekrar kullanılabilen önceden tanımlanmış bir SQL kod parçasıdır.
Stored Procedure'lerin olumlu etkileri:

Performansı artırır: Stored Procedure'ler, veritabanı sunucusunda çalışır ve genellikle veritabanına karşı yapılan bir işlemi tek bir çağrı ile tamamlar. Bu, ağ trafiğini azaltır ve genel performansı artırır.
Güvenlik: Stored Procedure'ler, SQL enjeksiyon saldırılarına karşı koruma sağlayabilir çünkü doğrudan SQL kodu yerine parametrelerle çalışır.
Tekrar kullanılabilirlik ve tutarlılık: Stored Procedure'ler bir kere yazılır ve ardından her yerden çağrılabilir. Bu, tekrar kullanılabilirliği artırır ve veritabanı işlemlerinde tutarlılık sağlar.
Bakım kolaylığı: Stored Procedure'lerde bir değişiklik yapmak, her uygulamada SQL deyimlerini değiştirmekten daha kolaydır.

Stored Procedure'lerin olumsuz etkileri:
Karmaşıklık: Stored Procedure'ler, karmaşık hale gelip yönetilmesi zor olabilir, özellikle de büyük ve karmaşık SQL ifadelerini içeriyorsa.
Taşınabilirlik sorunları: Stored Procedure'ler, bir veritabanı yönetim sisteminden (DBMS) diğerine taşınırken sorunlara neden olabilir çünkü her DBMS'nin Stored Procedure dilinde hafif farklılıklar olabilir.

Trigger (Tetikleyici): Bu, belirli bir olay gerçekleştiğinde otomatik olarak çalıştırılan özel bir tür Stored Procedure'dir.

Trigger'ların olumlu etkileri:

Veri bütünlüğü: Trigger'lar, veri bütünlüğünü korumak için kullanılabilir. Örneğin, bir tabloya yapılan belirli değişikliklerin başka bir tabloya da uygulanmasını sağlamak için bir Trigger kullanılabilir.
Otomatikleşme: Trigger'lar, belirli olaylara yanıt olarak otomatik olarak çalıştırılır, bu da birçok işlemi otomatikleştirebilir.

Olumsuz etkileri:

Performans sorunları: Trigger'lar, beklenmedik bir şekilde ve bazen birden çok kez tetiklenebilir. Bu durum, performans sorunlarına yol açabilir.
Karmaşıklık: Trigger'lar, veritabanı işlemlerinin ne zaman ve nasıl çalışacağını anlamayı zorlaştırabilir. Bu, hata ayıklama ve yönetim sürecini karmaşıklaştırabilir.
Stored Procedure'lar ve Trigger'lar, veritabanı yönetiminde güçlü araçlardır ancak doğru ve dikkatli bir şekilde kullanılmalıdır. İhtiyaçlarına uygun şekilde tasarlanmalı ve gereksinimleri karşılamak için optimize edilmelidir.

Senaryo : Bir sosyal medya platformunda kullanıcıların hesap aktivitelerini takip etmek ve belirli bir eşik değerini aşan etkileşimleri bildirmek için bir Stored Procedure oluşturmak istiyoruz.
CREATE PROCEDURE CheckActivityThreshold
AS
BEGIN
    DECLARE @Threshold INT = 1000; -- Eşik değeri

    SELECT UserName, Likes, Comments
    FROM UserActivity
    WHERE Likes + Comments > @Threshold;
END


import pyodbc

conn = pyodbc.connect('Driver={SQL Server};Server=myServerAddress;Database=myDatabase;UID=myUsername;PWD=myPassword')

cursor = conn.cursor()

cursor.execute("EXEC CheckActivityThreshold")

results = cursor.fetchall()
for row in results:
    print(row)

cursor.close()
conn.close()




2)Python ile veri tabanı bağlantısı yapıldıktan sonra stored procedure yada trigger kullanıldığı durumda python içerisinde bunu nasıl çağırabiliriz/nasıl bir mantık kurulabilir?

Python, stored procedure ve trigger'lar arasındaki ilişki, bir veritabanı sunucusuna bağlanmak ve SQL sorgularını çalıştırmak için kullanılan bir Python kütüphanesi üzerinden yönetilir. Pyodbc, MySQLdb (MySQL için), psycopg2 (PostgreSQL için) ve sqlite3 (SQLite için) gibi birçok farklı kütüphane mevcuttur. Bu kütüphaneler, Python'un SQL stored procedure ve trigger'ları kullanarak veritabanı işlemlerini yönetmesine olanak sağlar.
Python içerisinden stored procedure çağırmak genellikle aşağıdaki gibi bir mantıkla kurulabilir:
İlk olarak, doğru veritabanı kütüphanesini import edin ve bir veritabanına bağlantı oluşturun, veritabanına bağlantı üzerinden bir cursor oluşturun. Cursor'un execute metodunu kullanarak stored procedure'ü çağırın. Stored Procedure parametrelerini belirtmek gerekebilir. Stored Procedure'den dönen sonuçları alın ve işleyin. Son olarak, işiniz bittiğinde bağlantıyı ve cursor'ı kapatın.

import pyodbc 
conn = pyodbc.connect('Driver={SQL Server};Server=myServerAddress;Database=myDatabase;UID=myUsername;PWD=myPassword')
cursor = conn.cursor()
cursor.execute("{CALL stored_procedure_name (?, ?)}", [param1, param2])
results = cursor.fetchall()
for row in results:
    print(row)
cursor.close()
conn.close()


Burada, stored_procedure_name stored procedure'ün adıdır ve param1, param2 stored procedure'ye verilen parametrelerdir. Bu parametreler stored procedure'ün ihtiyaç duyduğu argümanlara karşılık gelir.

Trigger'lar ise genellikle veritabanında belirli bir olay (bir satırın eklenmesi, güncellenmesi vb.) gerçekleştiğinde otomatik olarak çalışır. Bu nedenle, Python genellikle bir trigger'ı doğrudan çağırmaz. Ancak, trigger'ın tetiklenmesine neden olan bir SQL ifadesi Python'dan gönderilebilir. Python'dan bir trigger'ı tetiklemek için genellikle bir INSERT, UPDATE veya DELETE SQL ifadesini çalıştırmak gerekir.

import pyodbc
conn = pyodbc.connect('Driver={SQL Server};Server=server_address;Database=database_name;UID=user_id;PWD=password')
cursor = conn.cursor()
cursor.execute("INSERT INTO table_name (column1, column2) VALUES (?, ?)", ('value1', 'value2'))
cursor.close()
conn.close()
