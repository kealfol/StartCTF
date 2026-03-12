# 🚩 CTF Checklist: Ultimate Preparation Guide

Этот репозиторий содержит пошаговый алгоритм действий ("First Steps") для решения задач в различных категориях CTF (Capture The Flag). Используй это как шпаргалку, когда не знаешь, с чего начать.

---

## 📑 Оглавление
1. [Steganography](#1-steganography)
2. [Web](#2-web)
3. [Reverse Engineering](#3-reverse-engineering)
4. [Misc](#4-misc)
5. [Cryptography](#5-cryptography)
6. [OSINT](#6-osint)
7. [Forensics](#7-forensics)
8. [PWN](#8-pwn)
9. [Hardware](#9-hardware)

---

## 1. 🖼️ Steganography
*Скрытие информации внутри медиафайлов.*

- [ ] **Базовый осмотр:**
    - `file <file>` — проверка реального расширения (может быть переименованный zip).
    - `strings -n 7 <file>` — поиск читаемых строк (ищи флаг или подсказки).
    - `exiftool <file>` — анализ метаданных (автор, GPS, комментарии).
- [ ] **Изображения (PNG/JPG):**
    - `binwalk -e <file>` — автоматическое извлечение скрытых вложений.
    - `zsteg -a <file>` — (для PNG/BMP) лучший инструмент для поиска данных в LSB (Least Significant Bit).
    - `steghide extract -sf <file>` — проверка на скрытый зашифрованный контейнер (часто требует пароль).
    - **StegSolve:** Просмотр цветовых каналов и наложение (XOR/AND) изображений друг на друга.
    - **Check PNG Corruptions:** Проверка чанков (IHDR, IDAT) через `pngcheck`.
- [ ] **Аудио (WAV/MP3):**
    - **Audacity:** Открой файл и переключись в режим **Spectrogram**. Ищи текст прямо в визуализации звука.
    - `stegseek` — сверхбыстрый брутфорс паролей для Steghide.
- [ ] **Текст:**
    - Проверка на нулевые символы или скрытые пробелы (`stegsnow`).

---

## 2. 🌐 Web
*Анализ уязвимостей веб-приложений.*

- [ ] **Разведка (Recon):**
    - `robots.txt`, `sitemap.xml`, `.well-known/`.
    - Проверка на наличие папок `.git/`, `.svn/`, `.env`, `.bash_history`.
    - `ffuf -u http://site.com/FUZZ -w wordlist.txt` — поиск скрытых директорий и файлов.
- [ ] **Клиентская часть (DevTools F12):**
    - **Debugger/Sources:** Изучи JS-файлы на предмет жестко закодированных API-ключей или логики проверки флага.
    - **Storage:** Проверь `Cookies` (может быть base64 или JWT) и `LocalStorage`.
- [ ] **Серверная часть (Backend):**
    - **SQL Injection:** Проверка `'` или `admin'--`. Используй `sqlmap -u <url> --batch`.
    - **LFI/RFI:** Попробуй прочитать файлы: `?page=../../../../etc/passwd`.
    - **SSTI:** Попробуй `{{7*7}}` или `${7*7}` в полях ввода (Jinja2, Twig, Smarty).
    - **Command Injection:** Попробуй `; ls`, `| id`, `` `whoami` ``.
    - **XXE:** Если сайт принимает XML, попробуй вставить внешнюю сущность для чтения файлов.
- [ ] **Уязвимости логики:** Попробуй обойти авторизацию, подменив `Role: user` на `Role: admin` в JSON-запросе или Cookie.

---

## 3. ⚙️ Reverse Engineering
*Анализ скомпилированного ПО.*

- [ ] **Статика:**
    - `checksec --file=<bin>` — проверка защит (NX, PIE, Canary, ASLR).
    - `Ghidra` / `IDA Pro` — декомпиляция кода в C. Изучай функцию `main` или обработчики ввода.
    - `nm <bin>` — просмотр имен функций (ищи `win`, `give_flag`).
- [ ] **Динамика:**
    - `gdb -q <bin>` (с плагином `GEF` или `pwndbg`). Ставь брейкпоинты (`b main`) и смотри значения регистров.
    - `ltrace ./bin` — перехват вызовов библиотечных функций (например, `strcmp`).
    - `strace ./bin` — перехват системных вызовов (`open`, `read`, `write`).
- [ ] **Специфика:**
    - **Python:** Если `.pyc`, используй `pycdc` или `uncompyle6`.
    - **Java:** Использй `jd-gui` или `bytecode viewer`.
    - **.NET:** Используй `dnSpy`.

---

## 4. 🧩 Misc
*Задачи на логику, скрипты и нестандартные форматы.*

- [ ] **Netcat:** `nc <host> <port>` — часто это первый шаг. Если это игра, автоматизируй её на Python с помощью `pwntools`.
- [ ] **PyJail / BashJail:** Обход ограничений интерпретатора (использование `__builtins__`, `os.system`).
- [ ] **Эзотерические языки:** Проверь код на [Esoteric Languages Wiki](https://esolangs.org/). Часто встречаются: Brainfuck, Befunge, Malbolge.
- [ ] **Архивы и файлы:**
    - Попробуй `zipdetails` или `7z l -slt`.
    - Брутфорс паролей: `john --wordlist=rockyou.txt archive.zip`.
- [ ] **QR-коды:** Если код поврежден, попробуй восстановить его в графическом редакторе или через онлайн-сервисы.

---

## 5. 🔐 Cryptography
*Расшифровка данных и анализ протоколов.*

- [ ] **Идентификация (Cipher ID):**
    - Используй **CyberChef** (функция Magic).
    - Проверь частотный анализ (для простых шифров замены).
- [ ] **Классика:**
    - Base64, Base32, Base58 (ищи по набору символов).
    - Шифр Цезаря, Виженер, Роторные машины (Enigma).
- [ ] **Современная крипта:**
    - **RSA:** Если даны `n`, `e`, `c`. Проверь `n` на `factordb.com`. Используй `RsaCtfTool`.
    - **XOR:** Если известен кусок флага (`flag{`), можно вычислить ключ: `cipher ^ "flag{"`.
    - **AES:** Проверь режим (ECB — одинаковые блоки шифруются одинаково, CBC — нужна инъекция IV).
- [ ] **Хеши:** Определи тип (MD5, SHA1, SHA256) и ищи в `CrackStation` или бруть через `hashcat`.

---

## 6. 🔍 OSINT
*Поиск по открытым источникам.*

- [ ] **Поиск людей/ников:**
    - `Sherlock` или `Maigret` для поиска профилей в соцсетях по никнейму.
    - Поиск в Google: `"nickname"`, `site:instagram.com "nickname"`.
- [ ] **Геолокация:**
    - Google Maps / Yandex Maps (Street View).
    - Поиск по уникальным объектам: вывески, номера машин, тип электрических розеток, форма деревьев.
    - Изучи метаданные фото (если есть).
- [ ] **Сайты и домены:**
    - `Whois`, `ViewDNS`.
    - `Wayback Machine` (web.archive.org) — ищи удаленную информацию.
- [ ] **Email:** Проверка утечек через `HaveIBeenPwned` или `Epieos`.

---

## 7. 📂 Forensics
*Анализ дампов системы и следов активности.*

- [ ] **Анализ трафика (PCAP):**
    - **Wireshark:** `Statistics -> HTTP -> Export Objects`.
    - Фильтры: `tcp.stream eq 1`, `dns`, `http.request.method == "POST"`.
    - Поиск файлов в потоках данных (ищи сигнатуры `PK`, `PNG`, `PDF`).
- [ ] **Дампы памяти (RAM):**
    - `Volatility 3`: `windows.pslist` (список процессов), `windows.filescan` (поиск файлов в памяти), `windows.dumpfiles`.
- [ ] **Файловые системы:**
    - `Autopsy` — профессиональный инструмент для анализа образов дисков.
    - Изучи `$MFT`, `LNK` файлы, Prefetch и реестр Windows.

---

## 8. 💣 PWN
*Эксплуатация бинарных уязвимостей.*

- [ ] **Классика (Buffer Overflow):**
    - Нахождение `offset` (дистанция до адреса возврата) через `cyclic`.
    - Перезапись `EIP/RIP`.
- [ ] **Техники:**
    - **Ret2Win:** Прямой переход на функцию, которая печатает флаг.
    - **Ret2Libc:** Обход NX (запрета исполнения в стеке) через вызов `system("/bin/sh")`.
    - **Format String:** Чтение/запись памяти через `%p`, `%x`, `%n` в функциях типа `printf`.
- [ ] **Инструменты:**
    - `pwntools` (Python библиотека) — мастхэв для написания эксплойтов.
    - `ROPgadget` — поиск гаджетов для построения цепочек.

---

## 9. 🔌 Hardware
*Реверс железа и прошивок.*

- [ ] **Прошивки (Firmware):**
    - `binwalk -Me <file>` — рекурсивная распаковка файловой системы.
    - Поиск паролей в конфигах `/etc/shadow`, `/etc/passwd` внутри прошивки.
- [ ] **Анализ сигналов (SDR):**
    - `GQRX` или `CubicSDR` для захвата.
    - `Universal Radio Hacker (URH)` для декодирования протоколов.
- [ ] **Интерфейсы:**
    - Знание распиновок: UART (TX/RX), SPI, I2C, JTAG.
    - Анализ дампов логического анализатора в `Saleae Logic`.

---

## 🛠 Общие инструменты (Must Have)
| Категория | Инструмент |
|-----------|------------|
| **Multi** | [CyberChef](https://gchq.github.io/CyberChef/) |
| **Web** | Burp Suite, ffuf, sqlmap |
| **Reverse** | Ghidra, GDB, IDA |
| **Stego** | StegSolve, binwalk, exiftool |
| **Pwn** | Pwntools, ROPgadget |
