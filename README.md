Yükleme
============

## Composer Kullanarak Yükleme

İlk olarak projenin bir klonunu indiriyoruz,
   ```
   git clone https://github.com/kouosl/app.git kouosl-app
   ```
Daha sonra proje klon dizinine geçiş yapıp composer ile bağımlılıklarını indiriyoruz,
   ```
   cd kouosl-app
   composer global require "fxp/composer-asset-plugin:^1.3.0"
   git config --global http.sslVerify false
   composer update
   ```
   
Proje bağımlılıkları indirildikten sonra ilk kullanıma hazır hale getirmek için,

   ```
   php init --env=Development --overwrite=All
   ```
   
Init işlemi bittiminden sonra veritabanı oluşturulur ve veritabanı bağlantı ayarları common/config/main-local.php dosyasına yazılır
   ```
   cd common/config
   ```
Veritabanı ayarları kayıt edildikten sonra migration işlemleri yapılır,

   ```
   php yii migrate --migrationPath=@vendor/kouosl/user/migrations --interactive=0
   php yii migrate --migrationPath=@vendor/kouosl/sample/migrations --interactive=0
   ```
   
Proje kurulumundan sonra apachenin vhost dosyası içine alttaki komut eklenir ve apache tekrar başlatılır
   ```
   <VirtualHost *:80>
       ServerName kouosl-app.dev
       
       ServerAdmin webmaster@localhost
       DocumentRoot "/path/to/kouosl-app"
       
       ErrorLog ${APACHE_LOG_DIR}/kouosl-error.log
       CustomLog ${APACHE_LOG_DIR}/kouosl.log combined	
       
       <Directory "/path/to/kouosl-app">
            AllowOverride All
       </Directory>
   </VirtualHost>
   ```
"/path/to" bölümüne apache server paylaşım dizininizi yazınız.("linux için : /var/www,windows için xampp:c:/xampp/htdocs/ gibi")

Host dosyasına da proje domaini alttaki gibi eklenir

```
127.0.0.1   kouosl-app.dev
```
   - Windows: `c:\Windows\System32\Drivers\etc\hosts`
   - Linux: `/etc/hosts`
   
Tarayıcıya http://kouosl-app.dev yazıldığı zaman projenin frontendi, http://kouosl-app.dev/admin yazıldığı zaman ise backendi,
http://kouosl-app.dev/api yazıldığında ise de api ye erişim sağlanmaktadır.

## Vagrant ile Yükleme

1. [VirtualBox](https://www.virtualbox.org/wiki/Downloads) Yüklemesi
2. [Vagrant](https://www.vagrantup.com/downloads.html) Yüklemesi
3. GitHub [personal API token](https://github.com/blog/1509-personal-api-tokens) Oluşturulması
3. Projenin indirilmesi:
   
   ```bash
   git clone https://github.com/kouosl/app.git kouosl-app
   cd kouosl-app/vagrant/config
   cp vagrant-local.example.yml vagrant-local.yml
   ```
   
4.  GitHub personal API tokenı `vagrant-local.yml` dosyasındaki yerine yapıştırın.
5. Proje dizinine tekrar gelin

   ```bash
   cd kouosl-app
   ```

5. Vagrant makinayı çalıştırın

   ```bash
   vagrant up
   ```
   
   
Vagrant makina kurulumu tamamlandıktan sonra
* frontend: http://kouosl-app.dev
* backend: http://kouosl-app.dev/admin
* api: http://kouosl-app.dev/api

ile erişilebilir

Cmd ile makinaya SSH erişimi için
   ```bash
   vagrant ssh
   ```
   
Hariçi bir programla ssh bağlantısı için bilgiler
* ip : 192.168.83.137
* user : vagrant
* password : vagrant
