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
influxd_enable="YES"
```

### Start Service
```sh
doas service influxd start
```

### Create a username and password for the influx shell.  This will be the username and password of the database you use inside TwinCAT
```sh
influx -username admin -password 123
```

### Create a database.  This must match the name of the Database in the Database project setup in TwinCAT
```sh
CREATE DATABASE mydb
```

### The measurements will go inside this Database.  For example Axis1 Axis2 Axis3 etc will all be a separate measurement with its own variable data.  For now exit
```sh
exit
```

### Install the TwinCAT Function Database server
```sh
Doas pkg install TF6420-Database-Server
```

### After install completes restart
```sh
Shutdown -r now
```

### Now inside TwinCAT we need to make sure the influx db settings are what we setup on the CX5140 and Activate the Configuration to the Server
<img width="1531" height="517" alt="image" src="https://github.com/user-attachments/assets/5552de26-9827-4903-b05f-6c3989c078b3" />
<img width="452" height="151" alt="image" src="https://github.com/user-attachments/assets/fc7cf3e2-2b13-4248-b46d-2d25ce42413d" />

















