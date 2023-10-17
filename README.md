# Jarkom-Modul-2-IT01-2023

11. Soal : Selain menggunakan Nginx, lakukan konfigurasi Apache Web Server pada worker Abimanyu dengan web server www.abimanyu.yyy.com. Pertama dibutuhkan web server dengan DocumentRoot pada /var/www/abimanyu.yyy    
install yang diperlukan di node abimanyu
```
apt-get install -y apache2
apt-get install libapache2-mod-php7.0
```
unduh resource ke /var/www
```
url="https://drive.google.com/uc?export=download&id=1a4V23hwK9S7hQEDEcv9FL14UkkrHc-Zc"
curl -kLo "abimanyu.IT01.com.zip" "$url" && unzip -q "abimanyu.IT01.com.zip" && rm "abimanyu.IT01.com.zip"
mv abimanyu.yyy.com abimanyu.IT01
mv abimanyu.IT01 /var/www/
```
Konfigurasikan di file /etc/apache2/sites-available/abimanyu.IT01.com.conf
```
<VirtualHost *:80>
    ServerAdmin webmaster@abimanyu.IT01.com
	ServerName abimanyu.IT01.com
	ServerAlias www.abimanyu.IT01.com
    DocumentRoot /var/www/abimanyu.IT01

    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```
lakukan ```a2ensite abimanyu.IT01.com.conf``` untuk mengaktifkan config tersebut  
lakukan ```lynx www.abimanyu.it01.com``` di client nakula/sadewa
![lynx abimanyuit01](https://github.com/Koro129/Jarkom-Modul-2-IT01-2023/assets/113784446/5b22b122-406c-42e0-8d87-7f8cae56c17e)  

12. Soal: Setelah itu ubahlah agar url www.abimanyu.yyy.com/index.php/home menjadi www.abimanyu.yyy.com/home.    
Tambahkan config berikut ke file /etc/apache2/sites-available/abimanyu.IT01.com.conf
```
<Directory /var/www/abimanyu.IT01>
     Options +FollowSymLinks -Multiviews
     AllowOverride All
    </Directory>
```
lalu buat /var/www/abimanyu.IT01/.htaccess yang berisi
```
RewriteEngine On
RewriteBase /
RewriteRule ^home$ index.php/home [L]
```
lakukan ```a2enmod rewrite``` untuk mengaktifkan rewrite  
lakukan ```lynx www.abimanyu.it01.com/home``` di client nakula/sadewa  
![lynx abimanyuit01home](https://github.com/Koro129/Jarkom-Modul-2-IT01-2023/assets/113784446/5b22b122-406c-42e0-8d87-7f8cae56c17e)  
  
13. Soal: Selain itu, pada subdomain www.parikesit.abimanyu.yyy.com, DocumentRoot disimpan pada /var/www/parikesit.abimanyu.yyy    
unduh resource ke /var/www 
```
url="https://drive.google.com/uc?export=download&id=1LdbYntiYVF_NVNgJis1GLCLPEGyIOreS"
curl -kLo "parikesit.abimanyu.IT01.com.zip" "$url" && unzip -q "parikesit.abimanyu.IT01.com.zip" && rm "parikesit.abimanyu.IT01.com.zip"
mv parikesit.abimanyu.yyy.com parikesit.abimanyu.IT01
mv parikesit.abimanyu.IT01 /var/www/
```
Konfigurasikan di file /etc/apache2/sites-available/parikesit.abimanyu.IT01.com.conf
```
<VirtualHost *:80>
    ServerAdmin webmaster@abimanyu.IT01.com
    ServerName parikesit.abimanyu.IT01.com
    ServerAlias www.parikesit.abimanyu.IT01.com
    DocumentRoot /var/www/parikesit.abimanyu.IT01

    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```    
lakukan ```a2ensite parikesit.abimanyu.IT01.com.conf``` untuk mengaktifkan config tersebut  
lakukan ```lynx www.parikesit.abimanyu.it01.com``` di client nakula/sadewa  
![lynx parikesitabimanyuit01](https://github.com/Koro129/Jarkom-Modul-2-IT01-2023/assets/113784446/3bc32a7c-16a8-4316-a605-e3cb27d51e66)

14. Soal: Pada subdomain tersebut folder /public hanya dapat melakukan directory listing sedangkan pada folder /secret tidak dapat diakses (403 Forbidden).    
Buat direktori secret di /var/www/parikesit.abimanyu.IT01
```
mkdir /var/www/parikesit.abimanyu.IT01/secret
```
Tambahkan config ke file /etc/apache2/sites-available/parikesit.abimanyu.IT01.com.conf
```
<Directory /var/www/parikesit.abimanyu.IT01/public>
        Options +Indexes
    </Directory>

    <Directory /var/www/parikesit.abimanyu.IT01/secret>
        Options -Indexes
</Directory>
```  
lakukan ```lynx www.parikesit.abimanyu.it01.com/public``` di client nakula/sadewa  
![lynx parikesitabimanyuit01public](https://github.com/Koro129/Jarkom-Modul-2-IT01-2023/assets/113784446/62d210cc-70a2-4b63-a43f-a28d8c3669ac)  
lakukan ```lynx www.parikesit.abimanyu.it01.com/secret``` di client nakula/sadewa  
![lynx parikesitabimanyuit01secret](https://github.com/Koro129/Jarkom-Modul-2-IT01-2023/assets/113784446/93236f22-5b49-442a-bef9-c22349ae54b8)
  
15. Soal: Buatlah kustomisasi halaman error pada folder /error untuk mengganti error kode pada Apache. Error kode yang perlu diganti adalah 404 Not Found dan 403 Forbidden.    
Tambahkan config ke file /etc/apache2/sites-available/parikesit.abimanyu.IT01.com.conf
```
ErrorDocument 403 /error/403.html
ErrorDocument 404 /error/404.html
```
lakukan ```lynx www.parikesit.abimanyu.it01.com/random``` di client nakula/sadewa  
![lynx parikesitabimanyuit01notfound](https://github.com/Koro129/Jarkom-Modul-2-IT01-2023/assets/113784446/e32036dd-3dec-45e0-b417-89a7a45f700c)  
lakukan ```lynx www.parikesit.abimanyu.it01.com/secret``` di client nakula/sadewa  
![lynx parikesitabimanyuit01forbidden](https://github.com/Koro129/Jarkom-Modul-2-IT01-2023/assets/113784446/c7b24372-7e39-4f33-95ac-5e8caa919cf0)
  
16. Soal: Buatlah suatu konfigurasi virtual host agar file asset www.parikesit.abimanyu.yyy.com/public/js menjadi www.parikesit.abimanyu.yyy.com/js    
Tambahkan config ke file /etc/apache2/sites-available/parikesit.abimanyu.IT01.com.conf
```
Alias "/js" "/var/www/parikesit.abimanyu.IT01/public/js"
```
lakukan ```lynx www.parikesit.abimanyu.it01.com/js``` di client nakula/sadewa
[lynx parikesitabimanyuit01js](https://github.com/Koro129/Jarkom-Modul-2-IT01-2023/assets/113784446/1868fa4b-6665-4ff0-ae4d-8ace986606aa)
  
17. Soal: Agar aman, buatlah konfigurasi agar www.rjp.baratayuda.abimanyu.yyy.com hanya dapat diakses melalui port 14000 dan 14400.
Tambahkan ke /etc/apache2/ports.conf
```
Listen 14000
Listen 14400
```
Unduh resource ke /var/www  
```
curl -kLo "rjp.baratayuda.abimanyu.IT01.com.zip" "$url" && unzip -q "rjp.baratayuda.abimanyu.IT01.com.zip" && rm "rjp.baratayuda.abimanyu.IT01.com.zip"

mv rjp.baratayuda.abimanyu.yyy.com rjp.baratayuda.abimanyu.IT01

rm -r /var/www/rjp.baratayuda.abimanyu.IT01

mv rjp.baratayuda.abimanyu.IT01 /var/www/
```  
Konfigurasikan di file /etc/apache2/sites-available/rjp.baratayuda.abimanyu.IT01.com.conf  
```
<VirtualHost *:14000 *:14400>
    ServerAdmin webmaster@rjp.baratayuda.abimanyu.IT01.com
    ServerName rjp.baratayuda.abimanyu.IT01.com
	ServerAlias www.rjp.baratayuda.abimanyu.IT01.com
    DocumentRoot /var/www/rjp.baratayuda.abimanyu.IT01

    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```  
Aktifkan dengan ```a2ensite rjp.baratayuda.abimanyu.IT01.com.conf```   
Lakukan ```lynx rjp.baratayuda.abimanyu.IT01.com:14000 atau port :14400``` di client nakula/sadewa  
![lynx rjpabimanyuit01](https://github.com/Koro129/Jarkom-Modul-2-IT01-2023/assets/113784446/de5f82f0-f888-48de-91ae-d1f1a62c17a5)  
  
18. Soal Untuk mengaksesnya buatlah autentikasi username berupa “Wayang” dan password “baratayudayyy” dengan yyy merupakan kode kelompok. Letakkan DocumentRoot pada /var/www/rjp.baratayuda.abimanyu.yyy.
Gunakan htpasswd
```
htpasswd -b -c /etc/apache2/.htpasswd Wayang baratayudaIT01
```
tambahkan konfig ke /etc/apache2/sites-available/rjp.baratayuda.abimanyu.IT01.com.conf
```
<Directory /var/www/rjp.baratayuda.abimanyu.IT01>
        AuthType Basic
        AuthName "Restricted Content"
        AuthUserFile /etc/apache2/.htpasswd
        Require user Wayang
</Directory>
```
Lakukan ```lynx rjp.baratayuda.abimanyu.IT01.com:14000 atau port :14400``` di client nakula/sadewa  
![lynx rjpabimanyuit01username](https://github.com/Koro129/Jarkom-Modul-2-IT01-2023/assets/113784446/4b916d13-cbff-47e5-a286-34129b98b662)  
![lynx rjpabimanyuit01password](https://github.com/Koro129/Jarkom-Modul-2-IT01-2023/assets/113784446/deb888ab-0e8b-4604-a5b2-64a80b7b7bc0)  
![lynx rjpabimanyuit01](https://github.com/Koro129/Jarkom-Modul-2-IT01-2023/assets/113784446/de5f82f0-f888-48de-91ae-d1f1a62c17a5)  
  
19. Soal: Buatlah agar setiap kali mengakses IP dari Abimanyu akan secara otomatis dialihkan ke www.abimanyu.yyy.com (alias)  
Tambahkan ```10.64.3.5``` di ```ServerAlias``` di file /etc/apache2/sites-available/abimanyu.IT01.com.conf  
Lakukan ```lynx 10.64.3.5``` di client nakula/sadewa  
![lynx abimanyuit01](https://github.com/Koro129/Jarkom-Modul-2-IT01-2023/assets/113784446/5b22b122-406c-42e0-8d87-7f8cae56c17e)
   
20. Soal: Karena website www.parikesit.abimanyu.yyy.com semakin banyak pengunjung dan banyak gambar gambar random, maka ubahlah request gambar yang memiliki substring “abimanyu” akan diarahkan menuju abimanyu.png.  
Tambahkan konfig ke /etc/apache2/sites-available/parikesit.abimanyu.IT01.com.conf
```
RewriteEngine On
RewriteCond %{REQUEST_URI} ^.*abimanyu.*
RewriteRule ^(.*)$ /public/images/abimanyu.png [L]
```
lakukan ```lynx www.parikesit.abimanyu.it01.com/randomabimanyu.png``` di client nakula/sadewa  
Sebelum di konfig, akan muncul not found, namun setelah disetting akan diminta mendownload gambar  
![lynx parikesitabimanyuit01download](https://github.com/Koro129/Jarkom-Modul-2-IT01-2023/assets/113784446/8fdcac44-6757-4ef2-9c1e-de482abc0792)
