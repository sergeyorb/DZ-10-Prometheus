# Домашнее задание - Prometheus

<ol>
<li>Установка Prometheus</li>
<li>Установка Alertmanager</li>
<li>Установка node_exporter</li>  
</ol>  

# Установка Prometheus

<ul>
- Загрузил пакет 
wget https://github.com/prometheus/prometheus/releases/download/v2.20.1/prometheus-2.20.1.linux-amd64.tar.gz
  - Создал каталоги для Prometheus
mkdir /etc/prometheus
mkdir /var/lib/prometheus![image](https://user-images.githubusercontent.com/98658046/170860945-38b4451a-0dc7-4bb7-9ddc-596b8b5fe3be.png)
  - Распаковал архив
tar zxvf prometheus-*.linux-amd64.tar.gz
  - Распределил файлы по каталогам
cp prometheus promtool /usr/local/bin/
cp -r console_libraries consoles prometheus.yml /etc/prometheus
  - Создал пользователя от которого бедет запускаться система мониторинга
useradd --no-create-home --shell /bin/false prometheus
  - Задал владельца для каталогов которые создал ранее
chown -R prometheus:prometheus /etc/prometheus /var/lib/prometheus
  - Задал владельца для скопированных файлов
chown prometheus:prometheus /usr/local/bin/{prometheus,promtool}
  - Запустил м проверил работу Prometheus   
/usr/local/bin/prometheus --config.file /etc/prometheus/prometheus.yml --storage.tsdb.path /var/lib/prometheus/ --
web.console.templates=/etc/prometheus/consoles --web.console.libraries=/etc/prometheus/console_libraries

systemctl status prometheus  
</ul>  

# Установка Alertmanager

<ul>
- Загрузил пакет
Wget https://github.com/prometheus/alertmanager/releases/download/v0.21.0/alertmanager-0.21.0.linux-amd64.tar.gz
- Создал каталоги для Alertmanager
mkdir /etc/alertmanager /var/lib/prometheus/alertmanager![image](https://user-images.githubusercontent.com/98658046/170861381-f79f31d7-d0ab-4576-b279-7617cb68bfcb.png)
- Распаковал архив
tar zxvf alertmanager-*.linux-amd64.tar.gz
- Распределил файлы по каталогам
cp alertmanager amtool /usr/local/bin/
cp alertmanager.yml /etc/alertmanager
- Создал пользователя от которого бедет запускаться Alertmanager
useradd --no-create-home --shell /bin/false alertmanager
- Задал владельца для каталогов которые создал ранее
chown -R alertmanager:alertmanager /etc/alertmanager /var/lib/prometheus/alertmanager  
- Задал владельца для скопированных файлов 
chown alertmanager:alertmanager /usr/local/bin/{alertmanager,amtool}
- Запустил Alertmanager
systemctl start alertmanager
- Проверил работу Alertmanager
systemctl start alertmanager
</ul>

# Установка node_exporter

<ul>
- Загрузил пакет
wget https://github.com/prometheus/node_exporter/releases/download/v1.3.1/node_exporter-1.3.1.linux-amd64.tar.gz
- Распаковал архив
tar zxvf  node_exporter-1.3.1.linux-amd64.tar.gz
- Скопировал исполняемый файл в bin
cp node_exporter /usr/local/bin/
- Создал пользователя от которого бедет запускаться node_exporter
useradd --no-create-home --shell /bin/false nodeusr
- Задал владельца для исполняевого файла
chown -R nodeusr:nodeusr /usr/local/bin/node_exporter
- Запустил node_exporter
systemctl start node_exporter
- Проверил работу node_exporter
systemctl status node_exporter  
</ul>
