# Домашнее задание - Prometheus

<ol>
<li>Установка Prometheus</li>
<li>Установка Alertmanager</li>
<li>Установка node_exporter</li>  
</ol>  

# Установка Prometheus

<ul>
<p>-загрузка пакета
<p>wget https://github.com/prometheus/prometheus/releases/download/v2.20.1/prometheus-2.20.1.linux-amd64.tar.gz

<p>-Установка (копирование файлов)
<p>После того, как мы скачали архив prometheus, необходимо его распаковать и скопировать содержимое по разным каталогам.

<p>Для начала создаем каталоги, в которые скопируем файлы для prometheus:
<p>mkdir /etc/prometheus
<p>mkdir /var/lib/prometheus

<p>-Распакуем наш архив:
<p>tar zxvf prometheus-*.linux-amd64.tar.gz

<p>... и перейдем в каталог с распакованными файлами:
<p>cd prometheus-*.linux-amd64

<p>Распределяем файлы по каталогам:
<p>cp prometheus promtool /usr/local/bin/
<p>cp -r console_libraries consoles prometheus.yml /etc/prometheus

<p>-Назначение прав
<p>Создаем пользователя, от которого будем запускать систему мониторинга:
<p>useradd --no-create-home --shell /bin/false prometheus
<p>* мы создали пользователя prometheus без домашней директории и без возможности входа в консоль сервера.

<p>-Задаем владельца для каталогов, которые мы создали на предыдущем шаге:
<p>chown -R prometheus:prometheus /etc/prometheus /var/lib/prometheus

<p>-Задаем владельца для скопированных файлов:
<p>chown prometheus:prometheus /usr/local/bin/{prometheus,promtool}

<p>-Запуск и проверка
<p>Запускаем prometheus командой:
<p>/usr/local/bin/prometheus --config.file /etc/prometheus/prometheus.yml --storage.tsdb.path /var/lib/prometheus/ --

<p>web.console.templates=/etc/prometheus/consoles --web.console.libraries=/etc/prometheus/console_libraries
<p>... мы увидим лог запуска — в конце «Server is ready to receive web requests»:

<p>level=info ts=2019-08-07T07:39:06.849Z caller=main.go:621 msg="Server is ready to receive web requests."
<p>Открываем веб-браузер и переходим по адресу http://<IP-адрес сервера>:9090 — загрузится консоль Prometheus:

<p>-Автозапуск
<p>Для настройки автоматического старта Prometheus мы создадим новый юнит в systemd.
<p>Создаем файл prometheus.service:
<p>vi /etc/systemd/system/prometheus.service
<p>Или
<p>sudo systemctl edit --full --force prometheus.service


<p>[Unit]
<p>Description=Prometheus Service
<p>After=network.target

<p>[Service]
<p>User=prometheus
<p>Group=prometheus
<p>Type=simple
<p>ExecStart=/usr/local/bin/prometheus \
<p>--config.file /etc/prometheus/prometheus.yml \
<p>--storage.tsdb.path /var/lib/prometheus/ \
<p>--web.console.templates=/etc/prometheus/consoles \
<p>--web.console.libraries=/etc/prometheus/console_libraries
<p>ExecReload=/bin/kill -HUP $MAINPID
<p>Restart=on-failure

<p>[Install]
<p>WantedBy=multi-user.target

<p>-Перечитываем конфигурацию systemd:
<p>systemctl daemon-reload

<p>-1Разрешаем автозапуск:
<p>systemctl enable prometheus

<p>После ручного запуска мониторинга, который мы делали для проверки, могли сбиться права на папку библиотек — снова зададим ей владельца:
<p>chown -R prometheus:prometheus /var/lib/prometheus

<p>Запускаем службу:
<p>systemctl start prometheus

<p>... и проверяем, что она запустилась корректно:
<p>systemctl status prometheus
<p>![image](https://user-images.githubusercontent.com/98658046/174864651-80e75c64-ce0a-46ae-8d5c-926468819b56.png)

</ul>  

# Установка Alertmanager

<ul>
<p>-используем ссылку для загрузки alertmanager:
<p>Wget https://github.com/prometheus/alertmanager/releases/download/v0.21.0/alertmanager-0.21.0.linux-amd64.tar.gz

<p>-Установка
<p>Создаем каталоги для alertmanager:
<p>mkdir /etc/alertmanager /var/lib/prometheus/alertmanager

<p>-Распакуем наш архив:
<p>tar zxvf alertmanager-*.linux-amd64.tar.gz
<p>... и перейдем в каталог с распакованными файлами:
<p>cd alertmanager-*.linux-amd64

<p>-Распределяем файлы по каталогам:
<p>cp alertmanager amtool /usr/local/bin/
<p>cp alertmanager.yml /etc/alertmanager

<p>-Назначение прав
<p>Создаем пользователя, от которого будем запускать alertmanager:
<p>useradd --no-create-home --shell /bin/false alertmanager
<p>* мы создали пользователя alertmanager без домашней директории и без возможности входа в консоль сервера.

<p>-Задаем владельца для каталогов, которые мы создали на предыдущем шаге:
<p>chown -R alertmanager:alertmanager /etc/alertmanager /var/lib/prometheus/alertmanager

<p>-Задаем владельца для скопированных файлов:
<p>chown alertmanager:alertmanager /usr/local/bin/{alertmanager,amtool}

<p>-Автозапуск

<p>-Создаем файл alertmanager.service в systemd:
<p>vi /etc/systemd/system/alertmanager.service
<p>Или
<p>sudo systemctl edit --full --force alertmanager.service

<p>[Unit]
<p>Description=Alertmanager Service
<p>After=network.target

<p>[Service]
<p>EnvironmentFile=-/etc/default/alertmanager
<p>User=alertmanager
<p>Group=alertmanager
<p>Type=simple
<p>ExecStart=/usr/local/bin/alertmanager \
<p>          --config.file=/etc/alertmanager/alertmanager.yml \
<p>          --storage.path=/var/lib/prometheus/alertmanager \
<p>          $ALERTMANAGER_OPTS
<p>ExecReload=/bin/kill -HUP $MAINPID
<p>Restart=on-failure

<p>[Install]
<p>WantedBy=multi-user.target

<p>-Перечитываем конфигурацию systemd:
<p>systemctl daemon-reload

<p>-Разрешаем автозапуск:
<p>systemctl enable alertmanager

<p>-Запускаем службу:
<p>systemctl start alertmanager

<p>Открываем веб-браузер и переходим по адресу http://<IP-адрес сервера>:9093 — загрузится консоль alertmanager:
<p>![image](https://user-images.githubusercontent.com/98658046/174865856-e55c630d-d10d-4208-a810-06839109f510.png)

</ul>

# Установка node_exporter

<ul>
 <p>-Устанавливаем на сервер и клиент

<p>wget https://github.com/prometheus/node_exporter/releases/download/v1.3.1/node_exporter-1.3.1.linux-amd64.tar.gz

<p>tar zxvf  node_exporter-1.3.1.linux-amd64.tar.gz

<p>-Копируем исполняемый файл в bin:

<p>cp node_exporter /usr/local/bin/

<p>-Назначение прав
<p>Создаем пользователя nodeusr:
<p>useradd --no-create-home --shell /bin/false nodeusr

<p>-Задаем владельца для исполняемого файла:
<p>chown -R nodeusr:nodeusr /usr/local/bin/node_exporter

<p>-Автозапуск
<p>vi /etc/systemd/system/node_exporter.service
<p>Или
<p>sudo systemctl edit --full --force node_exporter.service


<p>[Unit]
<p>Description=Node Exporter Service
<p>After=network.target

<p>[Service]
<p>User=nodeusr
<p>Group=nodeusr
<p>Type=simple
<p>ExecStart=/usr/local/bin/node_exporter
<p>ExecReload=/bin/kill -HUP $MAINPID
<p>Restart=on-failure

<p>[Install]
<p>WantedBy=multi-user.target

<p>-Перечитываем конфигурацию systemd:
<p>systemctl daemon-reload

<p>-Разрешаем автозапуск:
<p>systemctl enable node_exporter

<p>-Запускаем службу:
<p>systemctl start node_exporter

<p>Открываем веб-браузер и переходим по адресу http://<IP-адрес сервера или клиента>:9100/metrics — мы увидим метрики, собранные node_exporter:
<p>![image](https://user-images.githubusercontent.com/98658046/174866752-a76f974a-1f7f-4528-9726-7e9a23921461.png)
 
</ul>
