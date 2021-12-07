> 1. 
Скачиваем node_exporter
`wget https://github.com/prometheus/node_exporter/releases/download/v1.3.1/node_exporter-1.3.1.linux-amd64.tar.gz -P /tmp`
`cd /tmp`
Распаковываем архив и копируем его в /usr/local/bin
`tar -zxpvf node_exporter-1.3.1.linux-amd64.tar.gz`
`cd node_exporter-1.3.1.linux-amd64`
`sudo cp node_exporter /usr/local/bin`
` sudo chown node_exporter:node_exporter /usr/local/bin/node_exporter`
Создаем Systemd Unit
`sudo nano /etc/systemd/system/node_exporter.service`
`[Unit]`
`Description=Prometheus Node Exporter`
`Wants=network-online.target`
`After=network-online.target`
` `
`[Service]`
`User=node_exporter`
`Group=node_exporter`
`Type=simple`
`ExecStart=/usr/local/bin/node_exporter`
` `
`[Install]`
`WantedBy=multi-user.target`
Добавляем сервис в автозагрузку, запускаем его и проверяем статус
`sudo systemctl daemon-reload`
`sudo systemctl enable --now node_exporter`
`sudo systemctl status node_exporter`

>>● node_exporter.service - Node Exporter
>>     Loaded: loaded (/etc/systemd/system/node_exporter.service; enabled; vendor preset: enabled)
>>     Active: active (running) since Tue 2021-12-07 09:15:01 UTC; 1h 12min ago
>>   Main PID: 12819 (node_exporter)
>>      Tasks: 4 (limit: 1071)
>>      Memory: 5.0M
>>     CGroup: /system.slice/node_exporter.service
>>             └─12819 /usr/local/bin/node_exporter

> 2. CPU
node_cpu_seconds_total{cpu="0",mode="idle"} 8400.49
node_cpu_seconds_total{cpu="0",mode="system"} 17.14
node_cpu_seconds_total{cpu="0",mode="user"} 7.82
Память
node_memory_MemFree_bytes 3.38436096e+08
node_memory_MemTotal_bytes 1.028694016e+09
Диск
node_disk_io_time_seconds_total{device="dm-0"} 22.704
node_disk_io_time_seconds_total{device="dm-1"} 0.04
node_disk_io_time_seconds_total{device="sda"} 23.084
node_disk_read_bytes_total{device="dm-0"} 3.74670336e+08
node_disk_read_bytes_total{device="dm-1"} 3.342336e+06
node_disk_read_bytes_total{device="sda"} 3.88436992e+08
node_disk_read_time_seconds_total{device="dm-0"} 8.088000000000001
node_disk_read_time_seconds_total{device="dm-1"} 0.024
node_disk_read_time_seconds_total{device="sda"} 4.9
node_disk_write_time_seconds_total{device="dm-0"} 5.112
node_disk_write_time_seconds_total{device="dm-1"} 0
node_disk_write_time_seconds_total{device="sda"} 6.0760000000000005
Сеть
node_network_receive_errs_total{device="eth0"}
node_network_receive_bytes_total{device="eth0"}
node_network_transmit_bytes_total{device="eth0"}
node_network_transmit_errs_total{device="eth0"}
3. Веб-интерфес доступен с локальной машины

4. 
