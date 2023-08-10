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
enode=("e1389d912d3a698d71601d998520dc57a2800e417696bdf93553e63bcf31e597"
      "28192f4f62b8538db9ad9a5f107837ea83f8f06533ddd3fc39451cd0aa8da8bd"
      "1485287a41bc8bc95b1ef63e66f9e46b49eddc40f0da18d67c07ae755b3643ce"
      "9de7cb767b330068e376c59d84d84e6073a06b6e784241a9b13aa824ab455326"
      "d4bea76130db2e51273fa50e96f2a9f08c92c174700a0bdb452ea737633382a0"
      "04c7aa8da7ba470c8f40bae7a270bbdff450ebbc2d0413026de5545864a1b6d6"
      "78454a74ed32cf193fafecb53e6a45b12e2a4e25fb0176c7aa1855459e8e862b"
      "7577b2c26b704a7eae65f9e8db33217fe3f74bd41c550b06b624e23ab7f55d05"
      "ff031c02094a56842ed55db84d0a8127d3120684cc70ec12e6e8f44ee990b5ac"
      "8383a25545be7796ae8e676b52eae6d396e82358d703bedec2ab11e723127230"
      "18e395764e4576759f4d4a932dbdacc91fd967e2a5c3f04d321752d99a7741c8"
      "ab5053267a9d6e4e37586d3b36c1550f16c43b5dd85f1379e708d89da9789d9b"
      "5f7363273a1a7bce9e06ca8ebbae84e9879f908700c6ef5d15e928abfb556a21"
      "4c2f3b23553c8dd0610e7beaffac4cd934f026dcaf0f9d9eeddcd9af85d8943e"
      "fcdbd389487776e2f89c8429bad3f0edd751b3b8def4aaddbcf5533ec93452c2"
      "1fc8ece119b7122eb6fc386a7bf72621dd7c4fe4af77632e3177c08f53fdaf09"
      "7cd2ea1270fb9e56e1f051b180e36bcb85534939fbad02bef4589c7bbf7864d7"
      "f9d5094c9232b48c5cb05603e8bc0bb1bfb1adb7b063aa2ee3c8c9a3439f4d49"
      "c3bc1316d5048510ccb0c032aead286db1ed6cc6f6a0f68ef4cd482f85488edc"
      "ae03016db11b639bb9ed18a6ec39fbc79932572128b4a42ec232ba396e6a216d"
  )

function info(){
    cn=0
    while true
    do
        echo "$cn second."
        echo "node $1"
        rs1=$(curl -H "Content-Type: application/json" --data '{"jsonrpc":"2.0","method":"net_peerCount","id":64}' 127.0.0.1:$1 2>/dev/null)
        count=$(parse_json "$rs1" "result")
        echo "Number of node connections: $((16#${count:2}))"
        rs2=$(curl -H "Content-Type: application/json" --data '{"jsonrpc":"2.0","method":"eth_blockNumber","id":64}' 127.0.0.1:$1 2>/dev/null)
        blockNumber=$(parse_json "$rs2" "result")
        echo "Block height of the current peer: $((16#${blockNumber:2}))"
        rs3=$(curl -H "Content-Type: application/json" --data '{"jsonrpc":"2.0","method":"eth_getBlockByNumber","params":["'$blockNumber'",true],"id":64}' 127.0.0.1:$1 2>/dev/null)
        difficulty=$(parse_json "$rs3" "totalDifficulty")
        dy=$((16#${difficulty:2}))
        echo "current difficulty is $dy"
        peers=$(console $1 "admin.peers")
        echo "$peers" | sed -r 's/\x1B\[([0-9]{1,2}(;[0-9]{1,2})?)?[m|K]//g' | sed 's/,/\n/g' | while read line
        do
            if [[ $line =~ "id:" ]];then
                id=$(echo ${line##*:} | tr -d "\"")
                i=0
                while [ $i -lt ${#enode[@]} ]
                do
                    if [[ $id == ${enode[$i]} ]];then
                        echo -e "\033[45;36mid: ${id:0:4}.....${id:0-3:3}\033[0m"
                        break
                    fi
                    if [[ $i -eq $((${#enode[@]}-1)) ]];then
                        echo -e "id: ${id:0:4}.....${id:0-3:3}"
                    fi
                    let i++
                done
            elif [[ $line =~ "difficulty" ]];then
                df=$(echo ${line##*:})
                echo "difficulty: $df"
                if [[ $df -gt $dy ]];then
                    echo -e "\033[32m can sync\033[0m"
                fi
                echo ""
            fi
        done
        sleep 10
        clear
        let cn+=10
    done
}

function console(){
    expect -c "
    spawn ./erbie attach http://127.0.0.1:$1
    expect \"<\"
    send \"$2\r\"
    expect \"<\"
    send \"exit\r\"
    expect eof
    "
}

function parse_json(){
    if [[ $# -gt 1 ]] && [[ $1 =~ $2 ]];then
        echo "${1//\"/}" | sed "s/.*$2:\([^,}]*\).*/\1/"
    else
        echo "0x0"
    fi
}

function main(){
    if [[ ! -f erbie ]];then
        sudo docker cp erbie:/erb/erbie ./
    fi
    info 8544  # Always use port 8544
}

main "$@"

```
monitör başlatma

```
bash ./monitor.sh
```

