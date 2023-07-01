# Konfigurasi-Prometheus-dan-grafana

	Node Promethues
	------------------------
	1. Install prometheus
#wget https://github.com/prometheus/prometheus/releases/download/v2.44.0/prometheus-2.44.0.linux-amd64.tar.gz

	2. Ekstrak prometheus 
	#tar xvf prometheus-2.44.0.linux-amd64.tar.gz
	
	3. Tambahkan user
	#groupadd --system prometheus
	#useradd --system -s /sbin/nologin -g prometheus prometheus
	
	4. Memindahkan file biner prometheus dan promtool 
	#mv prometheus promtool /usr/local/bin/
	
	5. Buat folder 
	#mkdir /etc/prometheus
	
	6. Buat folder data untuk prometheus
	#mkdir /var/lib/prometheus
	
	7. Ganti permission folder prometheus dari root ke pr0metheus 
	#chown -R prometheus:prometheus /var/lib/prometheus/
	
	8. Pindahkan beberapa file ke /etc/prometheus/
	mv consoles/ console_libraries/ prometheus.yml /etc/prometheus/
	
	9. Masuk ke /etc/prometheus
	cd /etc/prometheus
	Nano prometheus.yml
	Isikan user dan group yang telah dibuat pada langkah 4. pastikan execstart, config.file, storage memiliki path yang sudah tepat
	
	10. Membuat service daemon 
	nano /etc/systemd/system/prometheus.service
	systemctl daemon-reload
	systemctl status prometheus
	systemctl enable --now prometheus
	
	11. Akses ip_prometheus : port_prometheus
	Contoh -> 10.2.15.87:9090
	Lalu klik status->targets untuk melihat node yang telah terhubung ke prometheus
	
Node exporter
-------------------
	1. Install node exporter
#wget  https://github.com/prometheus/node_exporter/releases/download/v1.6.0/node_exporter-1.6.0.linux-amd64.tar.gz

	2. Ekstrak node exporter
	#tar xvf node_exporter-1.6.0.linux-amd64.tar.gz
	
	3. Memindahkan file biner node_exporter 
	#mv node_exporter /usr/local/bin/
	
	4. Membuat service daemon
	#nano /etc/systemd/system/node-exporter.service
	Edit :
	Isikan user dan group yang telah dibuat pada langkah 4. pastikan execstart, config.file, storage memiliki path yang sudah tepat
	
	5. Aktifkan  service daemon dan cek status
	#systemctl enable --now node-exporter.service
	#systemctl status node-exporter.service
	
	Node Prometheus
	------------------------
	1. Tambahkan perintah berikut pada prometheus.yml bagian scrape_configs agar prometheus dapat melakukan scrapping pada node exporter
	-job_name:"node_exporter"
	  static_configs:
	      - targets: ["ip_node_exporter:9100"]
	
	2. Restart dan cek status
	#systemctl restart prometheus
	#systemctl status prometheus
	
	3. Buka prometheus di browser dan cek pada status-> targets apakah node-exporter telah terhubung
	
	4. Jika telaj terhubung. Pada kolom pencarian, cari query yang dibutuhkan. Contohnya intuk mengetahui mengenai CPU ketikkan node_cpu_seconds_total 
	
	Node Grafana
	------------------
	1. Install grafana
	#apt-get update
	#apt install grafana
	#systemctl enable --now grafana-server.service
	#systemctl status

	1. Akses grafana di browser. Masukkan ip addres dari node tempat grafana diinstal
	Contohnya : 10.2.15.82:3000
	Masukkan username = admin dan pass=admin
	
	2. Setting grafana
	Setting -> datasources ->add datasource -> prometheus -> isi url prometheus
	Save & test
	
	3. Buatkan dashboard
	Dashboard ->  import -> add new panel 
	Pada kolom prometheus. Pastikan untuk memilih prometheus
	Lalu import
