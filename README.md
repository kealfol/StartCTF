# 🚩 CTF Checklist: Методология и Инструментарий

Систематизированный перечень первичных действий при решении задач в различных категориях CTF

---

## 1. 🖼️ Steganography (Стеганография)
*Поиск скрытой информации в медиафайлах.*

- [ ] **Первичный анализ файла:**
    - Определение реального типа данных через команду `file`.
    - Извлечение читаемых строк через `strings -n 6` (искать паттерны флага или пароли).
    - Анализ метаданных (EXIF, комментарии, GPS) через инструмент [ExifTool](https://exiftool.org/).
- [ ] **Работа с изображениями:**
    - Автоматический поиск скрытых вложений через `binwalk -e` или [Foremost](https://github.com/korczis/foremost).
    - Проверка LSB (наименее значимых битов) в PNG/BMP через [zsteg](https://github.com/zed-0xff/zsteg).
    - Перебор цветовых плоскостей и наложений в [StegSolve](https://github.com/zardus/ctf-tools/blob/master/stegsolve/install).
    - Извлечение данных из зашифрованных контейнеров через `steghide extract` (для брутфорса пароля использовать `stegseek`).
- [ ] **Аудиофайлы:**
    - Визуальный анализ спектрограммы звука в [Audacity](https://www.audacityteam.org/) (искать текст или QR-коды).
    - Проверка на DTMF-тоны (набор номера) или азбуку Морзе.
- [ ] **Текстовая стеганография:**
    - Проверка на скрытые символы (пробелы/табуляция) через [stegsnow](https://www.rootshell.be/~vyncke/stegsnow/index.html).

---

## 2. 🌐 Web (Веб-уязвимости)
*Анализ клиентской и серверной частей веб-приложений.*

- [ ] **Разведка и фаззинг:**
    - Проверка технических файлов: `/robots.txt`, `/sitemap.xml`, `/.well-known/`.
    - Поиск скрытых директорий через [ffuf](https://github.com/ffuf/ffuf) или [Gobuster](https://github.com/OJ/gobuster) с использованием словарей `directory-list-2.3-medium`.
    - Проверка открытых репозиториев: `/.git/`, `/.svn/` (для восстановления использовать [GitTools](https://github.com/internetwache/GitTools)).
- [ ] **Анализ трафика и запросов:**
    - Перехват и модификация запросов через [Burp Suite](https://portswigger.net/burp/communitydownload).
    - Исследование Cookies и JWT-токенов (декодирование на [jwt.io](https://jwt.io/)).
- [ ] **Эксплуатация серверных уязвимостей:**
    - Проверка на SQL-инъекции (автоматизация через [sqlmap](https://github.com/sqlmapproject/sqlmap)).
    - Тестирование на SSTI (шаблонные инъекции) через `${7*7}` или `{{7*7}}` (инструмент [tplmap](https://github.com/epinna/tplmap)).
    - Проверка LFI (чтение локальных файлов) через подстановку `../../../../etc/passwd`.
    - Поиск Command Injection через спецсимволы `;`, `|`, `$(whoami)`.

---

## 3. ⚙️ Reverse Engineering (Реверс-инжиниринг)
*Анализ логики работы скомпилированных программ.*

- [ ] **Статический анализ:**
    - Проверка защит бинарного файла (NX, Canary, PIE) через `checksec`.
    - Декомпиляция кода в Си-подобный вид через [Ghidra](https://ghidra-sre.org/) или [IDA Pro](https://hex-rays.com/ida-pro/).
    - Поиск жестко закодированных строк, ключей и паролей в секциях данных.
- [ ] **Динамический анализ:**
    - Отладка программы в [GDB](https://www.sourceware.org/gdb/) с расширениями [pwndbg](https://github.com/pwndbg/pwndbg) или [GEF](https://github.com/hugsy/gef).
    - Отслеживание библиотечных вызовов через `ltrace` и системных вызовов через `strace`.
- [ ] **Специфические среды:**
    - Декомпиляция .NET через [dnSpy](https://github.com/dnSpy/dnSpy).
    - Декомпиляция Java (JAR) через [JD-GUI](http://java-decompiler.github.io/).
    - Анализ Python bytecode (.pyc) через [uncompyle6](https://github.com/rocky/python-uncompyle6).

---

## 4. 🧩 Misc (Разное)
*Скриптинг, эзотерические языки, тюрьмы (jails).*

- [ ] **Взаимодействие с сервером:**
    - Подключение по TCP через `nc <host> <port>`.
    - Автоматизация ответов в интерактивных задачах через библиотеку [pwntools](https://github.com/Gallopsled/pwntools) (модуль `remote`).
- [ ] **Эзотерические языки программирования:**
    - Идентификация и исполнение кода на Brainfuck, Piet, Whitespace через [известные интерпретаторы](https://esolangs.org/wiki/Main_Page).
- [ ] **Обход ограничений (Jailbreak):**
    - Побег из ограниченных оболочек Python (PyJail) через исследование `__builtins__`.
    - Обход Bash-ограничений (поиск доступных команд через `TAB`).

---

## 5. 🔐 Cryptography (Криптография)
*Дешифровка данных и поиск уязвимостей в алгоритмах.*

- [ ] **Идентификация шифра:**
    - Автоматическое определение типа шифра через [CyberChef](https://gchq.github.io/CyberChef/) (функция Magic) или [dCode](https://www.dcode.fr/en).
    - Частотный анализ для классических шифров (Цезарь, Виженер).
- [ ] **Слабости RSA:**
    - Факторизация малых модулей `n` через [factordb.com](http://factordb.com/).
    - Атаки на малую экспоненту или общие модули через [RsaCtfTool](https://github.com/Ganapati/RsaCtfTool).
- [ ] **Хеши и пароли:**
    - Определение типа хеша и поиск в онлайн-базах [CrackStation](https://crackstation.net/).
    - Брутфорс хешей через [Hashcat](https://hashcat.net/hashcat/) или [John the Ripper](https://github.com/openwall/john).

---

## 6. 🔍 OSINT (Разведка)
*Поиск информации в открытых источниках.*

- [ ] **Поиск по людям и никнеймам:**
    - Проверка присутствия никнейма в соцсетях через [Sherlock](https://github.com/sherlock-project/sherlock) или [Maigret](https://github.com/soxoj/maigret).
- [ ] **Геолокация и изображения:**
    - Обратный поиск картинок через Google Lens, Яндекс Визуальный поиск или [TinEye](https://tineye.com/).
    - Определение местоположения по метаданным или визуальным ориентирам через [Google Street View].
- [ ] **Веб-история и домены:**
    - Поиск удаленного контента в [Wayback Machine](https://archive.org/web/).
    - Анализ DNS-записей и истории доменов через [ViewDNS.info](https://viewdns.info/).

---

## 7. 📂 Forensics (Форензика)
*Анализ цифровых улик, дампов памяти и трафика.*

- [ ] **Анализ сетевого трафика (PCAP):**
    - Исследование пакетов в [Wireshark](https://www.wireshark.org/).
    - Извлечение переданных файлов: `File -> Export Objects -> HTTP`.
- [ ] **Дампы оперативной памяти:**
    - Анализ процессов и сетевых соединений через [Volatility 3](https://github.com/volatilityfoundation/volatility3).
- [ ] **Образы дисков:**
    - Исследование файловых систем и восстановление удаленных файлов в [Autopsy](https://www.autopsy.com/).

---

## 8. 💣 PWN (Бинарная эксплуатация)
*Эксплуатация уязвимостей в памяти приложений.*

- [ ] **Анализ бинарного файла:**
    - Определение смещения до адреса возврата (EIP/RIP) через `cyclic` в [pwntools].
    - Поиск ROP-гаджетов через [ROPgadget](https://github.com/JonathanSalwan/ROPgadget).
- [ ] **Написание эксплойта:**
    - Использование шаблона `pwn template` для быстрой разработки.
    - Локальная и удаленная отладка через `gdb.attach(p)`.

---

## 9. 🔌 Hardware (Железо)
*Анализ прошивок и низкоуровневых сигналов.*

- [ ] **Работа с прошивками:**
    - Извлечение файловых систем из дампов памяти через `binwalk -Me`.
- [ ] **Анализ радиосигналов и шин данных:**
    - Визуализация и декодирование сигналов в [Universal Radio Hacker (URH)](https://github.com/jopohl/urh).
    - Просмотр дампов логического анализатора в [Saleae Logic](https://www.saleae.com/downloads/).
