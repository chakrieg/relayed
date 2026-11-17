<div align="center">

# Relayed

**Готовый Windows-клиент для VPN-сервисов и провайдеров обхода блокировок**

Ядро sing-box · оболочка на Rust и Tauri 2 · три варианта оформления

[Демо-сайт](https://chakrieg.github.io/relayed/) · [Возможности](#возможности) · [Варианты оформления](#варианты-оформления) · [Сборка под бренд](#сборка-под-бренд)

</div>

---

## Что это

Relayed — десктопный клиент для Windows, который ставится поверх уже работающего бэкенда: пользователь вставляет ссылку на подписку, видит список серверов с пингом и подключается одной кнопкой. Ядро туннелирования — **sing-box**, оболочка — **Rust + Tauri 2**: нативное приложение с веб-интерфейсом внутри, без Electron и встроенного браузера.

Клиент рассчитан на сценарий «белый лейбл»: провайдер получает рабочий клиент под своим брендом и раздаёт его своим клиентам.

## Стек

| Слой | Что используется |
|---|---|
| Ядро | [sing-box](https://github.com/SagerNet/sing-box) — туннелирование, маршрутизация, правила |
| Оболочка | Rust, [Tauri 2](https://tauri.app/) (`tray-icon`, `image-png`) |
| Интерфейс | TypeScript, React 19 |
| Плагины | shell, clipboard-manager, dialog, notification |
| ОС | Windows 10/11 x64 |

## Возможности

### Подключение и подписки

- **Импорт подписки** — вставка прямой ссылки на конфигурацию, загрузка файла или вставка из буфера обмена.
- **Форматы**: sing-box JSON, список URI и Clash YAML. Из URI-ссылок разбираются **VLESS** (включая REALITY с `pbk`), **Hysteria2**, VMess, Trojan, Shadowsocks, HTTP/HTTPS и SOCKS.
- **Список серверов** с живым пингом, поиском по названию и отметкой выбранного сервера.
- **Один сервер или вся подписка** — импорт отдельной ссылки не стирает уже загруженную подписку.
- **Сохранность настроек**: перед записью конфигурации предыдущий файл сохраняется отдельно, поэтому сбой не оставляет клиент без рабочих данных.

### Режимы работы

- **Системный прокси** — весь трафик системы идёт через выбранный сервер.
- **Локальный прокси** — настраиваемый порт для отдельных приложений и браузеров.
- **Трей** — подключение и отключение без открытия окна.
- **Автозапуск** при входе в систему.
- **График скорости** в реальном времени.
- **Журнал ядра** с выбором уровня подробности — помогает разобрать, почему сервер не поднялся.

## Варианты оформления

В исходниках три независимых шаблона. Каждый собирается своей командой и ставится рядом с другими — со своим идентификатором приложения, поэтому настройки и списки серверов не перетирают друг друга.

| Вариант | Оформление | Окно | Идентификатор |
|---|---|---|---|
| **Base** | Тёмная тема по умолчанию | 1060 × 760 | `app.relayed.desktop` |
| **Material** | Тональные поверхности, скругления, крупная круглая кнопка | 1060 × 760 | `app.relayed.desktop.material` |
| **Compact** | Маленькое окно, круг-индикатор состояния со шкалой скорости | 380 × 640 | `app.relayed.desktop.compact` |

Различается только оформление, размер окна и размещение элементов — логика подключения и работа с подпиской общие.

## Сборка

Требуется [Rust](https://rustup.rs/) (1.77.2 или новее), Node.js и зависимости Tauri для Windows.

```bash
npm install

# dev-режим
npm run dev      # интерфейс на http://127.0.0.1:1420
npm run tauri    # десктопное окно Tauri

# релизные сборки — установщик NSIS на каждый вариант
npm run tauri:base
npm run tauri:material
npm run tauri:compact
```

Скрипты `tauri:material` и `tauri:compact` включают нужный шаблон через feature-флаги `template-material` / `template-compact` в `Cargo.toml`; без флага собирается базовый вариант.

## Вес

Установщик NSIS — около **23 МБ**, распакованное приложение — около **16 МБ**. Такой размер — следствие выбора Tauri 2: используется системный WebView2 Windows вместо встроенного Chromium, как у Electron-клиентов.

## Сборка под бренд

В клиент можно вшить название, иконку, цветовую акцентную палитру, набор серверов по умолчанию и свой вариант оформления. В коммерческих сборках подключаются **ссылки вида `relayed://`** — чтобы пользователь открывал подписку одним кликом из браузера или от провайдера, — и **автообновление** клиента. В демонстрационной сборке этих двух возможностей пока нет, они реализуются при заказе под ключ.

Заказать кастомную сборку можно через контакт на [демо-сайте](https://chakrieg.github.io/relayed/).

## Ссылки

- **Демо-сайт**: <https://chakrieg.github.io/relayed/>
- **Ядро**: [sing-box](https://github.com/SagerNet/sing-box)
- **Фреймворк**: [Tauri 2](https://tauri.app/)

---

<div align="center">

## English

**Relayed** is a Windows VPN client for providers and services: a Rust + Tauri 2 shell around the sing-box core. Users paste a subscription link, pick a server from a ping-sorted list, and connect with one button.

- **Stack**: Rust, Tauri 2, React 19, sing-box.
- **Protocols**: VLESS (incl. REALITY), Hysteria2, VMess, Trojan, Shadowsocks, HTTP/SOCKS — plus sing-box JSON and Clash YAML subscriptions.
- **Modes**: system proxy, local proxy, tray, autostart, live traffic graph, kernel log.
- **Three interface templates**: Base, Material, Compact — installable side by side (separate app identifiers).
- **Size**: ~23 MB NSIS installer, ~16 MB unpacked.
- **White-label**: branding, default servers and a custom template. `relayed://` deep-link subscription import and auto-update are delivered in commercial builds; the public demo build does not include them yet.

**Demo**: <https://chakrieg.github.io/relayed/>

</div>
