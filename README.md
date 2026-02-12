# InfluxDB + (Optional) TF6420 Setup on TwinCAT/BSD / FreeBSD

This repo documents the exact commands I use to:
1) Install and start **InfluxDB (influxd)** on FreeBSD / TwinCAT/BSD  
2) Enable it at boot  
3) Create a database from the Influx shell  
4) (Optional) Install **Beckhoff TF6420 Database Server** (TwinCAT function)

---

## 1) Install InfluxDB

### Install package
```sh
doas pkg install influxdb
```

### Start Influxdb
```sh
doas service influxd onestart
```

### Edit the configuration file
```sh
doas ee /usr/local/etc/influxd.conf
```
### Scroll down to the [http] section and uncomment the bind-address and auth-enabled
<img width="583" height="256" alt="image" src="https://github.com/user-attachments/assets/20fbe2c9-75cc-4e74-8250-aa99f6a14ba3" />

### Set Influxdb to autostart
```sh
doas ee /etc/rc.conf
```

### Add this line at the bottom of the file
```sh
influxd_enable="YES"<img width="238" height="23" alt="image" src="https://github.com/user-attachments/assets/c21f1d40-1042-4bc2-a97c-dc2cdd28a31f" />
```








