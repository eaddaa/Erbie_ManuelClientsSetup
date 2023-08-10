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

Docker kurulumunda monitor scripti 8544 portuna göre 

```
nano monitor.sh

```
```
#!/bin/bash

function info(){
   cn=0
   vl=$(wget https://docker.erbie.io/version >/dev/null 2>&1 && cat version | awk '{print $2}')
   while true
   do
            echo "the monitor version is $vl"
            echo "$cn second."
            echo "node $1"
            rs1=$(curl -H "Content-Type: application/json" --data '{"jsonrpc":"2.0","method":"net_peerCount","id":64}' 127.0.0.1:$1 2>/dev/null)
            count=$(parse_json "$rs1" "result")
            echo "Number of node connections: $((16#${count:2}))"
            rs2=$(curl -H "Content-Type: application/json" --data '{"jsonrpc":"2.0","method":"eth_blockNumber","id":64}' 127.0.0.1:$1 2>/dev/null)
            blckNumber=$(parse_json "$rs2" "result")
            echo "Block height of the current peer: $((16#${blckNumber:2}))"
            sleep 5
            clear
            let cn+=5
   done
}

function parse_json(){
   echo "${1//\"/}" | sed "s/.*$2:\([^,}]*\).*/\1/"
}

function main(){
   if [[ $# -eq 0 ]];then
            info 8544
   else
            info $1
   fi
}

main "$@"


```
monitör başlatma

```
bash ./monitor.sh
```

