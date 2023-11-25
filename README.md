# ENGRAM DENCUN GÜNCELLEMESİ

# ESKİ REPOYU SİLELİM


```
cd tokio-docker
docker compose down -v
cd
rm -rf tokio-docker
```


## YENİ REPOYU YÜKLEYELİM



```
git clone https://github.com/engram-network/tokio-docker.git 
cd tokio-docker
git checkout dencun
chmod +x ./scripts/*.sh
./scripts/install-docker.sh  
./scripts/install-asdf.sh
mkdir -p execution consensus
```

<h1 align="center">Deposit Bilgilerinin Kurulumu</h1>

```eth2-val-tools``` ve ```ethereal``` yüklü olduğunu kontrol ediyoruz.

```console
eth2-val-tools --help
ethereal version
```

```console
# Bu komut ile mnemonic oluşturuyor ve yedekliyoruz, sonrasında bir EVM cüzdana import ediyoruz.Önceki node kurulumunda oluşturduğunuz bir cüzdan ve mnemonicleriniz duruyorsa, bu kodu atlayıp,eski cüzdan bilgilerinizi girebilirsiniz.
# Admin bilgiler duruyorsa kullanabilirsiniz dedi.
 eth2-val-tools mnemonic
```
> Çıkan 24 kelimeyi metamaska import edin.

 ## METAMASKA YENİ RPC EKLEYELİM

Network id : `Engram`

RPC Endpoint : `https://tokioswift.engram.tech/`

ChainID : `131`

Token Name : `tGRAM`

https://faucet-tokio.engram.tech/   buradan cüzdan adresimize 33 tgram token alalım

```console
# İçersine giriyoruz
nano ./scripts/validator-deposit-data.sh

# Aşağıdaki kısımları değiştiriyoruz:
amount: # tGRAM deposit miktarı.32000000000 olarak karşınıza çıkacaktır.
smin: # Çıkan değeri değiştirmedim.
smax: # Çıkan değeri değiştirmedim.
withdrawals-mnemonic: # yukarda oluşturduğumuz mnemonicleri giriyoruz
validators-mnemonic: # yukarda oluşturduğumuz mnemonicleri giriyoruz
from: # mnemonicleri import ettiğimiz ve token aldığımız cüzdan adresi
privatekey: # mnemonicleri import ettiğimiz cüzdanın private keyi

# Deposit ile işlemi bitiriyoruz
bash ./scripts/validator-deposit-data.sh
```
>Deposit işleminin onaylanmasını bekleyin.
>Explorerdan kontrol edin:https://tokioscan-v2.engram.tech/




<h1 align="center">Public Key Oluşturma</h1>

```console
# On-chain deposit datasını düzenliyoruz.
docker run -it --rm -v $(pwd)/validator_keys:/app/validator_keys engramnet/staking-deposit-cli:dencun existing-mnemonic --num_validators=1 --validator_start_index=0
```

```console
# Kodu çalıştırdıktan sonra soruları cevaplıyoruz.
1.İngilizce için 3,Türkçe için 11 yazıp enter'a basın.
2.Deposit datası min. heighttan başlayacağı için 0 yazıp enter'a basın.
3.24 kelimeyi girip enter a basın.(İsterseniz her kelimenin yalnızca ilk 4 harfini girmeniz yeterlidir.)
4.Chain ismini seçiyoruz. testnet yazıp enter a basın.
5.Min. 8 karakter/sayıdan oluşan şifre oluşturun.(Şifre dosyasını tokio-docker/custom_config_data klasöründe bulabilirsiniz.)
6.Şifreyi tekrar girin.

# Daha sonra aşağıdaki gibi bir ekran karşınıza çıkacak.

███████╗███╗░░██╗░██████╗░██████╗░░█████╗░███╗░░░███╗  ████████╗░█████╗░██╗░░██╗██╗░█████╗░
██╔════╝████╗░██║██╔════╝░██╔══██╗██╔══██╗████╗░████║  ╚══██╔══╝██╔══██╗██║░██╔╝██║██╔══██╗
█████╗░░██╔██╗██║██║░░██╗░██████╔╝███████║██╔████╔██║  ░░░██║░░░██║░░██║█████═╝░██║██║░░██║
██╔══╝░░██║╚████║██║░░╚██╗██╔══██╗██╔══██║██║╚██╔╝██║  ░░░██║░░░██║░░██║██╔═██╗░██║██║░░██║
███████╗██║░╚███║╚██████╔╝██║░░██║██║░░██║██║░╚═╝░██║  ░░░██║░░░╚█████╔╝██║░╚██╗██║╚█████╔╝
╚══════╝╚═╝░░╚══╝░╚═════╝░╚═╝░░╚═╝╚═╝░░╚═╝╚═╝░░░░░╚═╝  ░░░╚═╝░░░░╚════╝░╚═╝░░╚═╝╚═╝░╚════╝░      
                                                                  
Creating your keys.
Creating your keys:               [####################################]  32/32          
Creating your keystores:          [####################################]  32/32          
Creating your depositdata:        [####################################]  32/32          
Verifying your keystores:         [####################################]  32/32

#İşlem tamamlanınca en altta "Press any key" yazısını görünce enter a basın.
```



```console
# Docker compose'u açalım:
sudo nano docker-compose.yml

# Aşağıdaki kısımları değiştirelim
ethstats=YourNameNodeHere  << YourNameNodeHere kısmını node/community adı ile değiştirin.
identity=elessar << Discord nickinizi girin.
graffiti=elessar << Discord nickinizi girin
enr-address=0.0.0.0 << IPAddresinizi girin.
suggested-fee-recipient=0xf39Fd6e51aad88F6F4ce6aB8827279cffFb92266 << Kendi cüzdan adresimizi giriyoruz.
graffiti=elessar << Nickname girin.

# Çalıştıralım
docker compose up -d

# Çalıştırdıktan sonra:
[+] Running 4/4
 ⠿ Network tokio_default_default                           Created
 ⠿ Container striatum_init                                 Exited
 ⠿ Container striatum_el                                   Started
 ⠿ Container lighthouse_init                               Exited
 ⠿ Container lighthouse_cl                                 Started
 ⠿ Container lighthouse_vc                                 Started
```
## Eğer ```docker logs -f lighthouse_vc``` komutu loglarınız aşağıdaki gibi ise ```password.txt``` dosyasını kontrol etmeniz gerekiyor.

```bash
Nov 23 16:34:30.001 INFO No validators present                   msg: see `lighthouse vm create --help` or the HTTP API documentation, service: notifier
Nov 23 16:34:42.000 INFO Connected to beacon node(s)             synced: 1, available: 1, total: 1, service: notifier
Nov 23 16:34:42.001 INFO No validators present                   msg: see `lighthouse vm create --help` or the HTTP API documentation, service: notifier
@@ -170,12 +174,27 @@ Nov 23 16:35:06.000 INFO Connected to beacon node(s)             synced: 1, avai
Nov 23 16:35:06.001 INFO No validators present                   msg: see `lighthouse vm create --help` or the HTTP API documentation, service: notifier
Nov 23 16:35:18.001 INFO Connected to beacon node(s)             synced: 1, available: 1, total: 1, service: notifier
Nov 23 16:35:18.001 INFO No validators present                   msg: see `lighthouse vm create --help` or the HTTP API documentation, service: notifier
```
### Dosyayı açtığınızda kurulumda oluşturduğunuz şifre yerine 0x.... şeklinde ise 0x.... i silip yerine şifrenizi girmeniz lazım.Bunu yapmanın 2 yolu var.
```console

# 1.Mobaxterm/Winscp ile direkt dosya üzerinden.
cd tokio-docker
docker compose down 
# Node u durdurduktan sonra <tokio-docker/custom_config_data> dizinine girip password.txt dosyasında düzenlemeyi yapıp kayıt edin.
docker compose up -d # ile node unuzu yeniden başlatın.

# 2.Terminal üzerinden.
cd tokio-docker
docker compose down 
cd tokio-docker/custom_config_data 
sudo nano password.txt #Düzeltmeyi yaptıktan sonra Ctrl X+Y enter yapın.
cd ..
docker compose up -d
```
Nodeun onaylanması 30dk--2sa arasında olabilir.

<h1 align="center">Log kontrolleri</h1>

```console
###Explorer:
https://beaconscan-v2.engram.tech
# striatum_el=Block
# lighthouse_cl=Slot

# İlk olarak buradan loglara bakıyoruz:
docker logs lighthouse_cl -f

INFO Subscribed to topics
INFO Sync state updated                      new_state: Evaluating known peers, old_state: Syncing Finalized Chain, service: sync
INFO Sync state updated                      new_state: Syncing Head Chain, old_state: Evaluating known peers, service: sync
INFO Sync state updated                      new_state: Synced, old_state: Syncing Head Chain, service: sync
INFO Subscribed to topics                    topics: ["/eth2/9c4e948f/bls_to_execution_change/ssz_snappy"]
INFO Successfully finalized deposit tree     finalized deposit count: 1, service: deposit_contract_rpc
```

```console
docker logs -f striatum_el 
```
```bash
INFO [09-26|19:28:45.046] Forkchoice requested sync to new head    number=30729 hash=a38be3..648659 finalized=30652
INFO [09-26|19:28:57.045] Forkchoice requested sync to new head    number=30730 hash=eb3642..45f557 finalized=30652
INFO [09-26|19:29:09.046] Forkchoice requested sync to new head    number=30731 hash=b9fd32..3748bd finalized=30652
INFO [09-26|19:29:21.046] Forkchoice requested sync to new head    number=30732 hash=51ff7b..803756 finalized=30652
INFO [09-26|19:29:33.046] Forkchoice requested sync to new head    number=30733 hash=f80ac7..19e5f7 finalized=30652
```

```console
docker logs -f lighthouse_vc
```
```bash
INFO Connected to beacon node(s)             synced: 1, available: 1, total: 1, service: notifier
INFO All validators active                   slot: 32836, epoch: 1026, total_validators: 32, active_validators: 32
INFO Connected to beacon node(s)             synced: 1, available: 1, total: 1,
INFO Validator exists in beacon chain        fee_recipient: 0x617b…063d,
INFO Awaiting activation                     slot: 17409, epoch: 544, validators: 32, service: notifier

Bendeki loglar bu şekilde:

Nov 23 16:34:30.001 INFO No validators present                   msg: see `lighthouse vm create --help` or the HTTP API documentation, service: notifier
Nov 23 16:34:42.000 INFO Connected to beacon node(s)             synced: 1, available: 1, total: 1, service: notifier
Nov 23 16:34:42.001 INFO No validators present                   msg: see `lighthouse vm create --help` or the HTTP API documentation, service: notifier
Nov 23 16:34:54.000 INFO Connected to beacon node(s)             synced: 1, available: 1, total: 1, service: notifier
Nov 23 16:34:54.000 INFO No validators present                   msg: see `lighthouse vm create --help` or the HTTP API documentation, service: notifier
Nov 23 16:35:06.000 INFO Connected to beacon node(s)             synced: 1, available: 1, total: 1, service: notifier
Nov 23 16:35:06.001 INFO No validators present                   msg: see `lighthouse vm create --help` or the HTTP API documentation, service: notifier
Nov 23 16:35:18.001 INFO Connected to beacon node(s)             synced: 1, available: 1, total: 1, service: notifier
Nov 23 16:35:18.001 INFO No validators present                   msg: see `lighthouse vm create --help` or the HTTP API documentation, service: notifier

```
Nodeun onaylanması 30dk--2sa arasında olabilir.

```bash
Peerlarda sorun olursa loglar aşağıdaki gibi olacaktır:

striatum_el
WARN [10-03|04:50:47.133] Beacon client online, but no consensus updates received in a while. Please fix your beacon client to follow the chain! 
WARN [10-03|04:55:47.172] Beacon client online, but no consensus updates received in a while. Please fix your beacon client to follow the chain!

lighthouse_cl
INFO Oct 03 04:59:39.001 WARN Low peer count                          peer_count: 0, service: slot_notifier
WARN Oct 03 04:59:39.001 INFO Searching for peers                     current_slot: 78259, head_slot: 5248, finalized_epoch: 162, finalized_root: 0xa9c8…f1f7, peers: 0, service: slot_notifier
WARN Oct 03 04:59:39.001 WARN Syncing deposit contract block cache    est_blocks_remaining: initializing deposits, service: slot_notifier
```

![image](https://github.com/KingsHarald0/engram-guncelleme/blob/b49ef2f0b6301b819181bcbea7e05ecefc00417f/Ekran%20g%C3%B6r%C3%BCnt%C3%BCs%C3%BC%202023-11-23%20172824.png)


https://nodewatch.engram.tech/ EXPLORERDAN KONTROL EDELİM



