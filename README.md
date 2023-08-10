# Erbie_ManuelClientsSetup

İlk olarak, ufw aracını kurun (eğer daha önce kurulu değilse):

```
sudo apt update
sudo apt install ufw
```
Daha sonra, 8544 portunu açın:

```
sudo ufw allow 8544/tcp

```
Son olarak, ufw'yi etkinleştirin:

```
sudo ufw enable

```

 Gerekli Araçları Kurma
 Eğer sisteminizde Go dilini daha önce yüklemediyseniz, aşağıdaki adımları takip ederek Go dilini kurabilirsiniz

```
sudo apt update
sudo apt install golang
```
Erbie Kaynak Kodunu İndirme:

```
git clone https://github.com/erbieio/erbie.git
cd erbie

```
Erbie Derleme:

```
make erbie

```

Erbie'yi Çalıştırma
Erbie'yi çalıştırırken istediğiniz portu (örneğin, 8544) kullanabilirsiniz.
Erbie'yi 8544 Portuyla Çalıştırma:

```
./build/bin/erbie --devnet --http --mine --syncmode=full --http.port 8544

```
Çalışma Durumunu Kontrol Etme
Erbie'nin doğru bir şekilde çalıştığından emin olmak için aşağıdaki adımları takip edebilirsiniz:

```
curl http://ipadresi:8544
```

Özel Anahtarın Eklenmesi ve Erbie Başlatılması
addkey.sh script'i çalıştırarak özel anahtarınızı ekleyin:

```
bash ./addkey.sh

```
Ardından Erbie'yi aşağıdaki komutla başlatın:

```
./build/bin/erbie --devnet --http --mine --syncmode=full

```
Özel anahtarınızın düzgün bir şekilde bağlandığını kontrol edin.

```
./build/bin/erbie attach ./.erbie/erbie.ipc

```
Döndürülen adres sizinkiyle aynıysa, bağlama başarılıdır.

Daha sonra erbie'nin tam olarak nasıl çalışacağını yapılandıran birçok komut kombinasyonu vardır. Aynı bilgiler, Erbie bulut sunucunuzdan herhangi bir zamanda şu çalıştırılarak elde edilebilir:

```
./build/bin/erbie --help

```

${ipc dosya yolu} Bulunması için komutlar:

Özel Anahtarın Doğruluğunun Kontrol Edilmesi
Erbie istemcisine aşağıdaki komutla bağlanarak özel anahtarınızın bağlı olup olmadığını kontrol edin:
./build/bin/erbie attach ${ipc dosya yolu}/erbie.ipc
erbie dizininin içinde hangi klasörde erbie.ipc dosyasının bulunabileceğini tespit etmeye çalışın.

Terminali kullanarak erbie dizinine gidin. Örneğin:

```
cd ~/erbie

```
Ardından, aşağıdaki komutu kullanarak erbie.ipc dosyasının tam yolunu arayabilirsiniz:
```
find . -name erbie.ipc

```
komut bu şekilde olacaktır
```
./build/bin/erbie attach ./.erbie/erbie.ipc
```
