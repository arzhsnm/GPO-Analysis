GPO Analysis
A network capture from the boot sequence of a domain-joined workstation has been provided. Analyze the traffic and extract the administrator’s password.

Как был получен пароль 
Анализ трафика
В дампе gpo.pcap отфильтрованы SMB-запросы на чтение файлов:

bash
tshark -r gpo.pcap -Y "smb2.cmd == 8 || smb.cmd == 0x2e" -T fields -e data | xxd -r -p | strings | grep -i cpassword
Обнаружен файл Groups.xml с двумя учётными записями. Для Administrateur найден атрибут:

text
cpassword="LjFWQMzS3GWDeav7+0Q0oSoOM43VwD30YZDVaItj8e0"
Расшифровка
Использована утилита gpp-decrypt (статический AES-ключ MS14-025):

bash
gpp-decrypt "LjFWQMzS3GWDeav7+0Q0oSoOM43VwD30YZDVaItj8e0"
Вывод:

text
TuM0sTrouv3
