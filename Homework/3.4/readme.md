> 1. 
Скачиваем node_exporter<br>
`wget https://github.com/prometheus/node_exporter/releases/download/v1.3.1/node_exporter-1.3.1.linux-amd64.tar.gz -P /tmp`<br>
`cd /tmp`<br>
Распаковываем архив и копируем его в /usr/local/bin<br>
`tar -zxpvf node_exporter-1.3.1.linux-amd64.tar.gz`<br>
`cd node_exporter-1.3.1.linux-amd64`<br>
`sudo cp node_exporter /usr/local/bin`<br>
` sudo chown node_exporter:node_exporter /usr/local/bin/node_exporter`<br>
Создаем Systemd Unit<br>
`sudo nano /etc/systemd/system/node_exporter.service`<br>
`[Unit]`<br>
`Description=Prometheus Node Exporter`<br>
`Wants=network-online.target`<br>
`After=network-online.target`<br>
` `<br>
`[Service]`<br>
`User=node_exporter`<br>
`Group=node_exporter`<br>
`Type=simple`<br>
`ExecStart=/usr/local/bin/node_exporter`<br>
` `<br>
`[Install]`<br>
`WantedBy=multi-user.target`<br>
Добавляем сервис в автозагрузку, запускаем его и проверяем статус<br>
`sudo systemctl daemon-reload`<br>
`sudo systemctl enable --now node_exporter`<br>
`sudo systemctl status node_exporter`<br>

>>● node_exporter.service - Node Exporter
>>     Loaded: loaded (/etc/systemd/system/node_exporter.service; enabled; vendor preset: enabled)
>>     Active: active (running) since Tue 2021-12-07 09:15:01 UTC; 1h 12min ago
>>   Main PID: 12819 (node_exporter)
>>      Tasks: 4 (limit: 1071)
>>      Memory: 5.0M
>>     CGroup: /system.slice/node_exporter.service
>>             └─12819 /usr/local/bin/node_exporter

> 2. CPU<br>
node_cpu_seconds_total{cpu="0",mode="idle"} 8400.49<br>
node_cpu_seconds_total{cpu="0",mode="system"} 17.14<br>
node_cpu_seconds_total{cpu="0",mode="user"} 7.82<br>
Память<br>
node_memory_MemFree_bytes 3.38436096e+08<br>
node_memory_MemTotal_bytes 1.028694016e+09<br>
Диск<br>
node_disk_io_time_seconds_total{device="dm-0"} 22.704<br>
node_disk_io_time_seconds_total{device="dm-1"} 0.04<br>
node_disk_io_time_seconds_total{device="sda"} 23.084<br>
node_disk_read_bytes_total{device="dm-0"} 3.74670336e+08<br>
node_disk_read_bytes_total{device="dm-1"} 3.342336e+06<br>
node_disk_read_bytes_total{device="sda"} 3.88436992e+08<br>
node_disk_read_time_seconds_total{device="dm-0"} 8.088000000000001<br>
node_disk_read_time_seconds_total{device="dm-1"} 0.024<br>
node_disk_read_time_seconds_total{device="sda"} 4.9<br>
node_disk_write_time_seconds_total{device="dm-0"} 5.112<br>
node_disk_write_time_seconds_total{device="dm-1"} 0<br>
node_disk_write_time_seconds_total{device="sda"} 6.0760000000000005<br>
Сеть<br>
node_network_receive_errs_total{device="eth0"}<br>
node_network_receive_bytes_total{device="eth0"}<br>
node_network_transmit_bytes_total{device="eth0"}<br>
node_network_transmit_errs_total{device="eth0"}<br>
> 3. Веб-интерфес доступен с локальной машины<br>

> 4.  
[    0.000000] Hypervisor detected: KVM<br>
[    0.004681] CPU MTRRs all blank - virtualized system.<br>
[    0.053040] Booting paravirtualized kernel on KVM<br>
[   43.671057] systemd[1]: Detected virtualization oracle.<br>

> 5. Команда `sysctl fs.nr_open` показывает максимальное число открытых дескрипторов (1024х1024)<br>
     ulimit -n 1048576<br>
> 6.  В первом терминале вводим `unshare -f --pid --mount-proc sleep 1h`<br>
Во втором ищем процес и переносим его в другой namespace<br>
root@vagrant:~# ps aux | grep sleep<br>
root        1189  0.0  0.0   8076   528 pts/2    S+   21:20   0:00 sleep 1h<br>
root        1202  0.0  0.0   8900   736 pts/1    S+   21:22   0:00 grep --color=auto sleep<br>
root@vagrant:~# nsenter --target 1189 -p --mount<br>
<br>
Как итог получаем процесс с <b>PID 1</b><br>
root@vagrant:/# ps aux<br>
USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND<br>
root           1  0.0  0.0   8076   528 pts/2    S+   21:20   0:00 sleep 1h<br>
root           2  0.0  0.3   9836  3952 pts/1    S    21:23   0:00 -bash<br>
root          11  0.0  0.3  11492  3324 pts/1    R+   21:23   0:00 ps aux<br>

> 7. Количество процессов на пользователя можно задать с помощью `ulimit -u`
`:(){ :|:& };:` - это рекурсивная функция. Вызывает 2 копии себя самой