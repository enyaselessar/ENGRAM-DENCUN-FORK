# ENGRAM-GUNCEL-KURULUM


METAMASKA YENİ RPC EKLEYELİM

Network id : Engram

RPC Endpoint : https://tokioswift.engram.tech/

ChainID : 131

Token Name : tGRAM



https://faucet-tokio.engram.tech/   buradan cüzdan adresimize 33 tgram token alalım




ESKİ REPOYU SİLELİM


cd tokio-docker

docker compose down -v

cd

rm -rf "/root/tokio-docker/"




YENİ REPOYU YÜKLEYELİM


git clone https://github.com/engram-network/tokio-docker.git 

cd tokio-docker

git checkout dencun

chmod +x ./scripts/*.sh

./scripts/install-asdf.sh

mkdir -p execution consensus



CÜZDANIMIZDA BULUNAN TOKENLARI DESPOSIT EDELİM


bash ./scripts/validator-deposit-data.sh




DEPOSIT DOSYASINI YAPILANDIRALIM

nano ./scripts/validator-deposit-data.sh komutunu girip aşağıdaki kısımları değiştirelim


withdrawals-mnemonic: Tırnak içine mnemonicler

validators-mnemonic: Tekrar tırnak içine mnemonicler

from: cüzdan adresimiz

privatekey: cüzdan adresimizin private keyi




VALIDATORUMUZU OLUSTURALIM


./scripts/validator-build.sh komutunu girip soruları cevaplayalım


1.soru enterlayalım

2.soru 0

3. soru mnemoniclerimizi girelim

4.soru testnet

5.soru password oluşturalım

6.soru password tekrar yazalım




DOCKER DEGISKENLERINI AYARLAYIP, ÇALIŞTIRALIM


sudo nano docker-compose.yml

identity=nickname

enr-address=IpAdresimiz

graffiti=discordİsmi

ethstats=nickname


docker compose up -d komutu ile docker imagelerimizi çalıştıralım


https://nodewatch.engram.tech/ EXPLORERDAN KONTROL EDELİM



