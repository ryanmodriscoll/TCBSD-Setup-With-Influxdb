# InfluxDB + (Optional) TF6420 Setup on TwinCAT/BSD / FreeBSD

This repo documents the exact commands I use to:
1) Install and start **InfluxDB (influxd)** on FreeBSD / TwinCAT/BSD  
2) Enable it at boot  
3) Create a database from the Influx shell  
4) Install **Beckhoff TF6420 Database Server** (TwinCAT function)

---

All of these packages installed require internet access.  The easiest way to do this is to connect an ethernet cable that has access to a network on the X001 slot.  You can check the internet connection by doing a "ping -c 4 google.com"

### Install influxdb package
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

### Influx uses the port 8086 to facilitate data exchange.  If firewall is going to be enabled we need to allow traffic through port 8086
```sh
doas ee /etc/pf.conf
```
<img width="617" height="782" alt="image" src="https://github.com/user-attachments/assets/48ee499e-3b09-4230-b223-44d11568d7e8" />


### Install the TwinCAT Function Database server
```sh
Doas pkg install TF6420-Database-Server
```

### After install completes restart
```sh
Shutdown -r now
```

### Now inside TwinCAT we need to make sure the influx db settings are what we setup on the BSD PC and Activate the Configuration to the Server
<img width="1116" height="385" alt="image" src="https://github.com/user-attachments/assets/082e20db-6120-4385-ab66-8c156037342e" />

<img width="452" height="156" alt="image" src="https://github.com/user-attachments/assets/2aef2848-046c-4a19-b15a-b23228b279ab" />



You can now activate configuration and execute the TwinCAT program

Here are some extra commands:

### Convert the Database measurement to an exportable CSV file:
```sh
influx -database mydata \
  -precision rfc3339 \
  -format csv \
  -execute 'SELECT * FROM "Axis1"' \
  > /var/tmp/Axis1.csv
```

### Transfer this CSV file to your laptop.  This command is run on your windows laptop and the IP address is the IP address of the Beckhoff PC
```sh
scp Administrator@192.168.1.44:/var/tmp/Axis1.csv C:\temp\
```



















