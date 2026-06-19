## GPO Analysis — кратко

**Задача:** A network capture from the boot sequence of a domain-joined workstation has been provided. Analyze the traffic and extract the administrator’s password.

**1. Извлечение Groups.xml из SMB-трафика:**
```bash
tshark -r gpo.pcap -Y "smb2.cmd == 8 || smb.cmd == 0x2e" -T fields -e data | xxd -r -p | strings | grep -i cpassword
```

**2. Найдено два аккаунта с `cpassword`:**

| Учётка | cpassword | Пароль |
|---|---|---|
| Helpdesk | `PsmtscOuXqUMW6KQzJR8RWxCuVNmBvRaDElCKH+FU+w` | `R00tm333` |
| Administrateur | `LjFWQMzS3GWDeav7+0Q0oSoOM43VwD30YZDVaItj8e0` | **`TuM@sTrouv3`** |

**3. Расшифровка** (CVE-2014-1812 / MS14-025 — статический AES-ключ Microsoft):
```bash
gpp-decrypt LjFWQMzS3GWDeav7+0Q0oSoOM43VwD30YZDVaItj8e0
```

**Флаг: `TuM@sTrouv3`**
