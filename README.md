# nebminer — NEB (Neural Engine Byte) GPU miner for NVIDIA

**[English](#english) · [Русский](#русский)**

---

## English

GPU miner for **NEB** (nebhash v2, Kepler and TESS datasets) for NVIDIA cards from Turing to Blackwell: RTX 20/30/40/50, GTX 16, CMP 30HX–170HX, A100/H100. Linux x86_64, a single binary. The miner picks the fastest kernel for your card by itself.

### Known hashrate
Our measurements, stock power limit, 19 Nov 2026.

| Card | Kepler, MH/s | TESS, MH/s |
|---|---|---|
| RTX 5090 (575 W) | 202–203 | 223–225 |
| RTX 4090 (450 W) | 160–166 | 181 |
| RTX 3070 Ti (310 W) | 26–29 | 42–45 |
| CMP 90HX (230 W) | ~28 | 32–45 |

The network alternates Kepler and TESS block by block, and the model changes every block, so the speed moves within these ranges.

### Pools
| Pool | Address |
|---|---|
| NEB pool (begemod) | `neb.begemod.xyz:3470` — https://neb.begemod.xyz |
| AriaPool | `neb.ariabrain.com:3370` — https://pool.ariabrain.com/neb.html |

### Launch examples
Download and unpack:
```
wget https://github.com/Pro7as/nebminer/releases/download/v0.7.2/nebminer-0.7.2-linux-x86_64.tar.gz
tar -xzf nebminer-0.7.2-linux-x86_64.tar.gz && cd nebminer-0.7.2
```
`neb1YOUR_ADDRESS` — your NEB address, `rig1` — your rig name.

**NEB pool (begemod)** — `neb.begemod.xyz:3470`
```
# all cards
./nebminer --pool neb.begemod.xyz:3470 --user neb1YOUR_ADDRESS.rig1
# only cards 0 and 2
./nebminer --pool neb.begemod.xyz:3470 --user neb1YOUR_ADDRESS.rig1 --devices 0,2
# in the background with a log
nohup ./nebminer --pool neb.begemod.xyz:3470 --user neb1YOUR_ADDRESS.rig1 > nebminer.log 2>&1 &
```

**AriaPool** — `neb.ariabrain.com:3370`
```
# all cards
./nebminer --pool neb.ariabrain.com:3370 --user neb1YOUR_ADDRESS.rig1
# only cards 0 and 2
./nebminer --pool neb.ariabrain.com:3370 --user neb1YOUR_ADDRESS.rig1 --devices 0,2
# in the background with a log
nohup ./nebminer --pool neb.ariabrain.com:3370 --user neb1YOUR_ADDRESS.rig1 > nebminer.log 2>&1 &
```

**Any pool**
```
# stats file for monitoring (JSON every 10 s)
./nebminer --pool neb.begemod.xyz:3470 --user neb1YOUR_ADDRESS.rig1 --stats /tmp/nebminer.json
```
As a service (starts with the system, restarts after errors): put your pool and address into the `ExecStart` line of `nebminer.service`, then
```
sudo mkdir -p /opt/nebminer && sudo tar -xzf nebminer-0.7.2-linux-x86_64.tar.gz -C /opt/nebminer --strip-components=1
sudo cp /opt/nebminer/nebminer.service /etc/systemd/system/
sudo systemctl daemon-reload && sudo systemctl enable --now nebminer
journalctl -u nebminer -f
```
### Requirements
Linux x86_64 (Ubuntu 22.04 / 24.04), NVIDIA driver 575.57 or newer recommended. No CUDA toolkit needed.

### Developer fee
5 %.

### Verify the download
`sha256sum -c SHA256SUMS`

---

## Русский

Майнер **NEB** (nebhash v2, наборы Kepler и TESS) для видеокарт NVIDIA от Turing до Blackwell: RTX 20/30/40/50, GTX 16, CMP 30HX–170HX, A100/H100. Linux x86_64, один исполняемый файл. Самое быстрое ядро под вашу карту майнер выбирает сам.

### Известный хешрейт
Наши замеры, заводской лимит мощности, 19.11.2026.

| Карта | Kepler, MH/s | TESS, MH/s |
|---|---|---|
| RTX 5090 (575 Вт) | 202–203 | 223–225 |
| RTX 4090 (450 Вт) | 160–166 | 181 |
| RTX 3070 Ti (310 Вт) | 26–29 | 42–45 |
| CMP 90HX (230 Вт) | ~28 | 32–45 |

Сеть чередует Kepler и TESS от блока к блоку, а модель меняется каждый блок, поэтому скорость гуляет в этих пределах.

### Пулы
| Пул | Адрес |
|---|---|
| Пул NEB (begemod) | `neb.begemod.xyz:3470` — https://neb.begemod.xyz |
| AriaPool | `neb.ariabrain.com:3370` — https://pool.ariabrain.com/neb.html |

### Примеры запуска
Скачать и распаковать:
```
wget https://github.com/Pro7as/nebminer/releases/download/v0.7.2/nebminer-0.7.2-linux-x86_64.tar.gz
tar -xzf nebminer-0.7.2-linux-x86_64.tar.gz && cd nebminer-0.7.2
```
`neb1ВАШ_АДРЕС` — ваш адрес NEB, `rig1` — имя рига.

**Пул NEB (begemod)** — `neb.begemod.xyz:3470`
```
# все карты
./nebminer --pool neb.begemod.xyz:3470 --user neb1ВАШ_АДРЕС.rig1
# только карты 0 и 2
./nebminer --pool neb.begemod.xyz:3470 --user neb1ВАШ_АДРЕС.rig1 --devices 0,2
# в фоне с журналом
nohup ./nebminer --pool neb.begemod.xyz:3470 --user neb1ВАШ_АДРЕС.rig1 > nebminer.log 2>&1 &
```

**AriaPool** — `neb.ariabrain.com:3370`
```
# все карты
./nebminer --pool neb.ariabrain.com:3370 --user neb1ВАШ_АДРЕС.rig1
# только карты 0 и 2
./nebminer --pool neb.ariabrain.com:3370 --user neb1ВАШ_АДРЕС.rig1 --devices 0,2
# в фоне с журналом
nohup ./nebminer --pool neb.ariabrain.com:3370 --user neb1ВАШ_АДРЕС.rig1 > nebminer.log 2>&1 &
```

**Любой пул**
```
# файл статистики для мониторинга (JSON каждые 10 с)
./nebminer --pool neb.begemod.xyz:3470 --user neb1ВАШ_АДРЕС.rig1 --stats /tmp/nebminer.json
```
Службой (запуск вместе с системой, перезапуск после сбоев): впишите пул и свой адрес в строку `ExecStart` файла `nebminer.service`, затем
```
sudo mkdir -p /opt/nebminer && sudo tar -xzf nebminer-0.7.2-linux-x86_64.tar.gz -C /opt/nebminer --strip-components=1
sudo cp /opt/nebminer/nebminer.service /etc/systemd/system/
sudo systemctl daemon-reload && sudo systemctl enable --now nebminer
journalctl -u nebminer -f
```
### Требования
Linux x86_64 (Ubuntu 22.04 / 24.04), драйвер NVIDIA от 575.57. CUDA ставить не нужно.

### Комиссия разработчика
5 %.

### Проверка архива
`sha256sum -c SHA256SUMS`
