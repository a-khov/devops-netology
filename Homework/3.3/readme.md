> 1. `chdir("/tmp")`

> 2. `/usr/share/misc/magic.mgc`

> 3. В первом терминале запускаем процесс с записью в файл<br>
    `ping 127.0.0.1 > ping.log`<br>
    Во втором смотрим<br>
    `ps -aux | grep ping` <br>
    ~``vagrant     1328  0.0  0.0   9692   936 pts/0    S+   21:39   0:00 ping 127.0.0.1``<br>
    `ls -lh`<br>
        `~-rw-rw-r-- 1 vagrant vagrant 10K Dec  5 21:42 ping.log`<br>
    Удаляем процесс<br>
    `sudo rm -f ping.log`<br>
    Проверяем файл <br>
    `tail ping.log` <br>
        `~tail: cannot open 'ping.log' for reading: No such file or directory`<br>
    Проверяем процесс <br>
    `sudo lsof -l -p 1328`<br>
        `~COMMAND  PID     USER   FD   TYPE DEVICE SIZE/OFF   NODE NAME`<br>
        `~ping    1328     1000    1w   REG  253,0    15163 131090 /home/vagrant/ping.log (deleted)`<br>
    Для обнуления отправляем пустую строку в файл <br>
    `echo " " | sudo tee /proc/1328/fd/1` <br>
    Проверяем размер файла <br>
    `sudo lsof -l -p 1328` <br>
        `~COMMAND  PID     USER   FD   TYPE DEVICE SIZE/OFF   NODE NAME` <br>
        `~ping    1328     1000    1w   REG  253,0    28261 131090 /home/vagrant/ping.log (deleted)`<br>
> 4. Зомби процессы не используют ресурсы, однако оставляют запись в таблице процессов.

> 5. При команде  `strace opensnoop-bpfcc`
    `openat(AT_FDCWD, "/etc/ld.so.cache", O_RDONLY|O_CLOEXEC) =`
    `openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libc.so.6", O_RDONLY|O_CLOEXEC) = 3`
    `openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libpthread.so.0", O_RDONLY|O_CLOEXEC) = 3`
    `openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libdl.so.2", O_RDONLY|O_CLOEXEC) = 3`
    `openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libutil.so.1", O_RDONLY|O_CLOEXEC) = 3`
> 6. <b>Part of the utsname information is also accessible via /proc/sys/kernel/{ostype, hostname, osrelease, version,
domainname}.</b>

> 7. &nbsp    ; - используется для разделения команд, выполнятся будут все команды из списка.<br>
        && - следующая команда будет выполнятся только если предыдущая выполнилась успешно.<br>
    `set -e` так же будет выполнять следующюю команду если предыдущая выполнилась успешно, так же как и в `&&`

> 8.   Завершит сценарий при наличии ошибок, на любом этапе выполнения сценария, кроме последней завершающей команды. Позволяет вносить в лог ошибок отсутствующие переменные.<br>
        `-e завершает работу если команда выдает не нулевой код завершения.`<br>
        `-u интерпретирует не установленные переменные как ошибки`<br>
        `-x пошагово отображает все команды вместе с их аргументами`<br>
        `-o pipefail устанавливает режим оболочки который будет менять код завершения конвеера на код последней не удачно завершившейся команды или на 0 в случае успешного завершения команд`<br>

> 9.    S* -процессы в ожидании завершения (waiting for an event to complete) 
        I* -фоновые процессы  (Idle kernel thread)
    