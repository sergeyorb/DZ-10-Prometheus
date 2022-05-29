# Домашнее задание - Prometheus

<ol>
<li>Установка Prometheus</li>
<li></li>
<li></li>  
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
