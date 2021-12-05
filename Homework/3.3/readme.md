> 1. `chdir("/tmp")`

> 2. `/usr/share/misc/magic.mgc`

> 3. В первом терминале запускаем процесс с записью в файл
    `ping 127.0.0.1 > ping.log`
    Во втором смотрим
    `ps -aux | grep ping`
    ~``vagrant     1328  0.0  0.0   9692   936 pts/0    S+   21:39   0:00 ping 127.0.0.1``
    `ls -lh
        ~-rw-rw-r-- 1 vagrant vagrant 10K Dec  5 21:42 ping.log`
    Удаляем процесс
    `sudo rm -f ping.log`
    Проверяем файл
    `tail ping.log
         ~tail: cannot open 'ping.log' for reading: No such file or directory`
    Проверяем процесс
    `sudo lsof -l -p 1328
        ~COMMAND  PID     USER   FD   TYPE DEVICE SIZE/OFF   NODE NAME
        ~ping    1328     1000    1w   REG  253,0    15163 131090 /home/vagrant/ping.log (deleted)`
    Для обнуления отправляем пустую строку в файл
    `echo " " | sudo tee /proc/1328/fd/1`
    Проверяем размер файла
    `sudo lsof -l -p 1328
        ~COMMAND  PID     USER   FD   TYPE DEVICE SIZE/OFF   NODE NAME
        ~ping    1328     1000    1w   REG  253,0    28261 131090 /home/vagrant/ping.log (deleted)`
> 4. Зомби процессы не используют ресурсы, однако оставляют запись в таблице процессов.

> 5. При команде  `strace opensnoop-bpfcc`
    `openat(AT_FDCWD, "/etc/ld.so.cache", O_RDONLY|O_CLOEXEC) =
    openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libc.so.6", O_RDONLY|O_CLOEXEC) = 3
    openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libpthread.so.0", O_RDONLY|O_CLOEXEC) = 3
    openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libdl.so.2", O_RDONLY|O_CLOEXEC) = 3
    openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libutil.so.1", O_RDONLY|O_CLOEXEC) = 3`

