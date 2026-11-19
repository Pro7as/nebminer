# Changelog / История версий

## 0.7.2 — 2026-11-19
- **Fix:** the miner could drop the model the pool had just sent and sit at 0 MH/s until the next block (1–4 min dips on rigs running longer than ~10 min; present since 0.3.0). Oldest models are now dropped; the newest and the current ones never are.
  **Исправлено:** майнер мог удалить только что присланную пулом модель и стоять на 0 MH/s до следующего блока (провалы 1–4 мин у ригов, работающих дольше ~10 мин; ошибка с 0.3.0). Теперь удаляются самые старые модели, новая и текущие — никогда.
- **New:** AriaPool support (`--pool neb.ariabrain.com:3370`), two connections per card, 5 % fee as a separate connection.
  **Новое:** поддержка AriaPool, два соединения на карту, комиссия 5 % — отдельным соединением.

## 0.7.1 — 2026-11-19
- Automatic kernel choice measures each candidate ≥ 40 ms after a warm-up (0.7.0 sometimes picked a slower kernel on RTX 5090).
  Автовыбор ядра меряет каждый вариант ≥ 40 мс после прогрева (0.7.0 на RTX 5090 иногда брал более медленное ядро).

## 0.7.0 — 2026-11-19
- Kernel v2.6: shorter layer-5 epilogue (~25 % fewer instructions), two variants (5 and 4 blocks per SM). RTX 5090 up to ~203/225 MH/s, RTX 4090 up to ~166/181 (Kepler/TESS).
  Ядро v2.6: эпилог слоя 5 короче на ~25 % команд, два варианта (5 и 4 блока на SM).

## 0.6.0 — 2026-11-19
- Kernel v2.4 (RTX 4090/5090 +7–11 %); tensor-free mode for CMP cards whose driver throttles tensor instructions.
  Ядро v2.4; режим «без тензоров» для CMP с урезанными тензорными командами.

## 0.4.0 — 2026-11-19
- Turing (sm_75) support; systemd unit runs without root.
  Поддержка Turing (sm_75); служба systemd без root.

## 0.3.0 — 2026-11-19
- First public release: neb-pool/1, kernels v2.1/v2.2, CPU check of every share, 5 % developer fee.
  Первый выпуск: neb-pool/1, ядра v2.1/v2.2, проверка каждой доли на ЦП, комиссия 5 %.
