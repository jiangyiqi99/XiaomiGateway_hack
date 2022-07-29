# Soft hack to open telnet
You need gateway 3(mgl03) connected to MiHome. And also ip and gateway token.

### 1 way (recommended)
Via [XiaomiGateway3](https://github.com/AlexxIT/XiaomiGateway3) component.

You must input in the 'Open Telnet command' field(**as it is without changing anything**):
```
{"method":"set_ip_info","params":{"ssid":"\"\"","pswd":"123123 ; passwd -d admin ; echo enable > /sys/class/tty/tty/enable; telnetd"}}
```

### 2 way (recommended if not using Home Assistant)
php-miio (https://github.com/skysilver-lab/php-miio)

You may need to change id.
```shell
php miio-cli.php --ip GW_IP --token GW_TOKEN --sendcmd '{"id":123,"method":"set_ip_info","params":{"ssid":"\"\"","pswd":"123123 ; passwd -d admin ; echo enable > /sys/class/tty/tty/enable; telnetd"}}'
```

### 3 way (maybe problem with sequence id)
python-miio (https://github.com/rytilahti/python-miio)
```shell
miiocli device --ip GW_IP --token GW_TOKEN raw_command set_ip_info '{"ssid":"\"\"","pswd":"123123 ; passwd -d admin ; echo enable > /sys/class/tty/tty/enable; telnetd"}'
```

This gateway has much more potential that you may think. I’ve been working with this device last few weeks and have something to share with community. Ok, first of all here is how to get root access without disassembling. Just send this command via MIIO protocol:


{"id":0,"method":"enable_telnet_service", "params":[]}
then you can telnet on it, use login “admin” or “app”. no password.

How to send message via MIIO: you need device token. There are a lot of methods you can find on internet how to do this, I used this one: download and install modified MiHome
https://ru.kapiba.ru/mihome/files/old/MiHome_5.6.93_63033_vevs.apk 452
Authorise, click on “Mi Smart Home Hub”, thee dots, Additional Settings, Network info. Here you will find token at the very bottom.
Then use this tool https://github.com/skysilver-lab/php-miio 542 to send miio message to device:

php miio-cli.php  --ip $ip --token $token --sendcmd '{"id":0,"method":"enable_telnet_service", "params":[]}'
now you have telnet enabled.

There is a mosquito broker running and all internal communication is running via mqtt messages. I was able to integrate all my devices with HA with help of NodeRed and simple workflow.
Login: admin

Password is empty

After opening telnet, it is better to install custom firmware **(only for Xiaomi Gateway 3 mgl03)**.

Read here:
https://github.com/zvldz/mgl03_fw/tree/main/firmware#the-easy-way

**Open telnet command** should also work with:
* lumi.gateway.mgl03 - Mi Smart Home Hub
* lumi.gateway.acn01 - Aqara Hub M1S CN
* lumi.gateway.aeu01 - Aqara Hub M1S EU
* lumi.aircondition.acn05 - Aqara Air Conditioning Controller P3
* lumi.gateway.sacn01 - Smart USB Wall Outlet Hub

# Aqara Hub E1 (ZHWG16LM usb stick)
You need gateway E1 connected to MiHome. And also ip and gateway token.

### 1 way (recommended)
Via [XiaomiGateway3](https://github.com/AlexxIT/XiaomiGateway3) component, version 2+.

You must input in the 'Open Telnet command' field(**as it is without changing anything**):
```
{"method":"set_ip_info","params":{"ssid":"\"\"","pswd":"123123 ; /bin/riu_w 101e 53 3012; telnetd"}}
```

### 2 way (recommended if not using Home Assistant)
php-miio (https://github.com/skysilver-lab/php-miio)

You may need to change id.
```shell
php miio-cli.php --ip GW_IP --token GW_TOKEN --sendcmd '{"id":123,"method":"set_ip_info","params":{"ssid":"\"\"","pswd":"123123 ; /bin/riu_w 101e 53 3012; telnetd"}}'
```

### 3 way (maybe problem with sequence id)
python-miio (https://github.com/rytilahti/python-miio)
```shell
miiocli device --ip GW_IP --token GW_TOKEN raw_command set_ip_info '{"ssid":"\"\"","pswd":"123123 ;  /bin/riu_w 101e 53 3012 ; telnetd"}'
```

Login: root

Password is empty



***I am not author, I just tested and improved and published.***



[Enable telnet on Aqara G3 hub](https://github.com/Wh1terat/aQRootG3)