# nebminer — NEB (Neural Engine Byte) GPU miner for NVIDIA

**[English](#english) · [Русский](#русский)**

---

## English

nebminer is a GPU miner for **NEB** (nebhash v2, KEP1 network: Kepler and TESS datasets) for NVIDIA cards from **Turing to Blackwell**: RTX 20/30/40/50, GTX 16, CMP 30HX–170HX, A100/H100. Linux x86_64, one static-dependency binary.

### Highlights
- **Own CUDA kernels.** Layer 5 of the training step runs on integer tensor cores (`mma u8×s8`); the small layers use a per-sample cache built on the card. The miner measures every kernel variant on **every new model** (each block) and keeps the fastest one for your card.
- **Works on cut-down CMP cards.** Some CMP 90HX run with tensor instructions throttled ~30× by the driver; the miner detects it by measurement and switches to its tensor-free kernel.
- **Every share is checked** on the CPU against the reference before it is sent; on every model change the card runs a self-check against the reference.
- **Two pool protocols, one binary:** `neb-pool/1` (JSON stratum, e.g. `neb.begemod.xyz:3470`) and the NEB node work-service frames used by **AriaPool** (`neb.ariabrain.com:3370`, chosen automatically by the pool name or with `--proto aria`).
- Low CPU use (blocking waits, ~1–2 % of one core per process).

### Quick start
```
tar -xzf nebminer-0.7.2-linux-x86_64.tar.gz && cd nebminer-0.7.2
./nebminer --pool neb.begemod.xyz:3470 --user neb1YOUR_ADDRESS.rig1
# AriaPool:
./nebminer --pool neb.ariabrain.com:3370 --user neb1YOUR_ADDRESS.rig1
```
Options: `--devices 0,1` (default: all cards), `--stats FILE` (JSON every 10 s, HiveOS-style), `--stripes -1|0|3|6|nt|v26|v26b` (default `-1`: automatic choice by measurement).
A systemd unit is in the archive (`nebminer.service`, runs without root).

### Requirements
- Linux x86_64 with glibc ≥ 2.34 (Ubuntu 22.04 / 24.04).
- NVIDIA driver only, no CUDA toolkit. Recommended **≥ 575.57** (needed for the PTX fallback built with CUDA 12.9); the prebuilt cubins should also load on 525.60+ per NVIDIA's CUDA 12.x compatibility rules — not tested by us. Tested: 595.84, 595.91, 595.99, 615.71, 617.14, 620.70.

### Measured speed (our measurements, 19 Nov 2026, stock power limit, our pool, 150 s windows)
| Card | Kepler, MH/s | TESS, MH/s |
|---|---|---|
| RTX 5090 (575 W) | 202–203 | 223–225 |
| RTX 4090 (450 W) | 160–166 | 181 |
| RTX 3070 Ti (310 W) | 26–29 | 42–45 |
| CMP 90HX (230 W, tensors not throttled) | ~28 | 32–45 |

Speed depends on the network model, which changes every block, and on clocks and power limits. Turing cards: supported, speed not measured yet.

### Developer fee — 5 %
- `neb-pool/1` pools: about 5 of every 100 shares are submitted under the login `neb1lc7rf0xv0tpwv3vga20hmsx4c0xnv2cwgpdn5fvzzaz9wsspye5qv0zv0y.devfee`.
- AriaPool: a separate connection under the same `….devfee` login mines 5 seconds of every 100.
The miner prints the fee login at start.

### AriaPool notes
AriaPool credits only shares of a job's reference header (zero extranonce), at most 2³² attempts per job, and sends a new job every 30 s (≈143 MH/s of credited work per connection). nebminer therefore opens **two connections per card**, so RTX 4090/5090 lose no credited work. Shares are not acknowledged by AriaPool; the miner shows "shares sent", and the pool's page shows what it credited.

### Verify the download
```
sha256sum -c SHA256SUMS
```

### Changelog
See [CHANGELOG](CHANGELOG.md).

---

## Русский

nebminer — майнер **NEB** (nebhash v2, сеть KEP1: наборы Kepler и TESS) для видеокарт NVIDIA **от Turing до Blackwell**: RTX 20/30/40/50, GTX 16, CMP 30HX–170HX, A100/H100. Linux x86_64, один исполняемый файл.

### Главное
- **Свои ядра CUDA.** Слой 5 шага обучения считается на целочисленных тензорных блоках (`mma u8×s8`), малые слои — по кэшу образцов, который строится на карте. На **каждой новой модели** (каждый блок) майнер сам меряет все варианты ядра и берёт самый быстрый для вашей карты.
- **Работает на урезанных CMP.** На части CMP 90HX драйвер режет тензорные команды примерно в 30 раз; майнер видит это по замеру и сам переходит на ядро без тензорных команд.
- **Каждая доля проверяется** эталоном на процессоре перед отправкой; при каждой смене модели карта проходит самопроверку против эталона.
- **Два протокола пулов в одном файле:** `neb-pool/1` (JSON stratum, например `neb.begemod.xyz:3470`) и кадры службы работы узла NEB, на которых работает **AriaPool** (`neb.ariabrain.com:3370`; выбирается сам по имени пула или ключом `--proto aria`).
- Почти не грузит процессор (ожидание карты — сон потока, ~1–2 % одного ядра).

### Быстрый старт
```
tar -xzf nebminer-0.7.2-linux-x86_64.tar.gz && cd nebminer-0.7.2
./nebminer --pool neb.begemod.xyz:3470 --user neb1ВАШ_АДРЕС.риг1
# AriaPool:
./nebminer --pool neb.ariabrain.com:3370 --user neb1ВАШ_АДРЕС.риг1
```
Ключи: `--devices 0,1` (по умолчанию все карты), `--stats ФАЙЛ` (JSON каждые 10 с, как в HiveOS), `--stripes -1|0|3|6|nt|v26|v26b` (по умолчанию `-1` — выбор замером).
В архиве есть служба systemd (`nebminer.service`, работает не от root).

### Требования
- Linux x86_64, glibc ≥ 2.34 (Ubuntu 22.04 / 24.04).
- Только драйвер NVIDIA, CUDA ставить не нужно. Рекомендуется **≥ 575.57** (нужен для запасного PTX, собранного CUDA 12.9); готовые cubin по правилам совместимости CUDA 12.x должны грузиться и на 525.60+ — нами не проверено. Проверено на: 595.84, 595.91, 595.99, 615.71, 617.14, 620.70.

### Скорость (наши замеры 19.11.2026, заводской лимит мощности, наш пул, окна по 150 с)
| Карта | Kepler, MH/s | TESS, MH/s |
|---|---|---|
| RTX 5090 (575 Вт) | 202–203 | 223–225 |
| RTX 4090 (450 Вт) | 160–166 | 181 |
| RTX 3070 Ti (310 Вт) | 26–29 | 42–45 |
| CMP 90HX (230 Вт, тензоры не урезаны) | ~28 | 32–45 |

Скорость зависит от модели сети (меняется каждый блок), частот и лимита мощности. Карты Turing поддерживаются, скорость пока не замерена.

### Комиссия разработчика — 5 %
- Пулы `neb-pool/1`: примерно 5 долей из каждых 100 отправляются под логином `neb1lc7rf0xv0tpwv3vga20hmsx4c0xnv2cwgpdn5fvzzaz9wsspye5qv0zv0y.devfee`.
- AriaPool: отдельное соединение под тем же логином `….devfee` майнит 5 секунд из каждых 100.
Логин комиссии майнер пишет при старте.

### Про AriaPool
AriaPool засчитывает только доли опорного заголовка работы (extranonce из нулей) — не больше 2³² попыток на работу, а новая работа приходит раз в 30 с (≈143 MH/s засчитываемой работы на соединение). Поэтому nebminer открывает **два соединения на карту** — RTX 4090/5090 не теряют зачёт. AriaPool доли не подтверждает: майнер пишет «доли отправлено», засчитанное видно на странице пула по адресу.

### Проверка архива
```
sha256sum -c SHA256SUMS
```

### История версий
См. [CHANGELOG](CHANGELOG.md).
