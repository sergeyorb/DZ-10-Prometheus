# Домашнее задание - Prometheus

<ol>
<li>Установка Prometheus</li>
<li>Установка Alertmanager</li>
<li>Установка node_exporter</li>  
</ol>  

# Установка Prometheus

<ul>
<p>- Загрузил пакет 
<p>wget https://github.com/prometheus/prometheus/releases/download/v2.20.1/prometheus-2.20.1.linux-amd64.tar.gz
<p>  - Создал каталоги для Prometheus
<p>mkdir /etc/prometheus
<p>mkdir /var/lib/prometheus![image](https://user-images.githubusercontent.com/98658046/170860945-38b4451a-0dc7-4bb7-9ddc-596b8b5fe3be.png)
<p>  - Распаковал архив
<p>tar zxvf prometheus-*.linux-amd64.tar.gz
<p>  - Распределил файлы по каталогам
<p>cp prometheus promtool /usr/local/bin/
<p>cp -r console_libraries consoles prometheus.yml /etc/prometheus
<p>  - Создал пользователя от которого бедет запускаться система мониторинга
<p>useradd --no-create-home --shell /bin/false prometheus
<p>  - Задал владельца для каталогов которые создал ранее
<p>chown -R prometheus:prometheus /etc/prometheus /var/lib/prometheus
<p>  - Задал владельца для скопированных файлов
<p>chown prometheus:prometheus /usr/local/bin/{prometheus,promtool}
<p>  - Запустил м проверил работу Prometheus   
<p>/usr/local/bin/prometheus --config.file /etc/prometheus/prometheus.yml --storage.tsdb.path /var/lib/prometheus/ --
<p>web.console.templates=/etc/prometheus/consoles --web.console.libraries=/etc/prometheus/console_libraries

<p>systemctl status prometheus  
</ul>  

# Установка Alertmanager

<ul>
<p>- Загрузил пакет
<p>Wget https://github.com/prometheus/alertmanager/releases/download/v0.21.0/alertmanager-0.21.0.linux-amd64.tar.gz
<p>- Создал каталоги для Alertmanager
<p>mkdir /etc/alertmanager /var/lib/prometheus/alertmanager![image](https://user-images.githubusercontent.com/98658046/170861381-f79f31d7-d0ab-4576-b279-7617cb68bfcb.png)
<p>- Распаковал архив
<p>tar zxvf alertmanager-*.linux-amd64.tar.gz
<p>- Распределил файлы по каталогам
<p>cp alertmanager amtool /usr/local/bin/
<p>cp alertmanager.yml /etc/alertmanager
<p>- Создал пользователя от которого бедет запускаться Alertmanager
<p>useradd --no-create-home --shell /bin/false alertmanager
<p>- Задал владельца для каталогов которые создал ранее
<p>chown -R alertmanager:alertmanager /etc/alertmanager /var/lib/prometheus/alertmanager  
<p>- Задал владельца для скопированных файлов 
<p>chown alertmanager:alertmanager /usr/local/bin/{alertmanager,amtool}
<p>- Запустил Alertmanager
<p>systemctl start alertmanager
<p>- Проверил работу Alertmanager
<p>systemctl start alertmanager
</ul>

# Установка node_exporter

<ul>
<p>- Загрузил пакет
<p>wget https://github.com/prometheus/node_exporter/releases/download/v1.3.1/node_exporter-1.3.1.linux-amd64.tar.gz
<p>- Распаковал архив
<p>tar zxvf  node_exporter-1.3.1.linux-amd64.tar.gz
<p>- Скопировал исполняемый файл в bin
<p>cp node_exporter /usr/local/bin/
<p>- Создал пользователя от которого бедет запускаться node_exporter
<p>useradd --no-create-home --shell /bin/false nodeusr
<p>- Задал владельца для исполняевого файла
<p>chown -R nodeusr:nodeusr /usr/local/bin/node_exporter
<p>- Запустил node_exporter
<p>systemctl start node_exporter
<p>- Проверил работу node_exporter
<p>systemctl status node_exporter  
</ul>
