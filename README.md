 # Базовая настройка коммутатора
Топология


<img width="498" height="150" alt="image" src="https://github.com/user-attachments/assets/4ce04aed-07c1-4450-96f4-518c303e471e" />

| Устройство   |Интерфейс     |Ip-адрес/ префикс
| ------------- |:------------------:| -----:|
|S1            |VLAN1         |192.168.1.10/24
| PC-A         | NIC          |192.168.1.10/24

## Задачи
Часть 1. Проверка конфигурации коммутатора по умолчанию

Часть 2. Создание сети и настройка основных параметров устройства  
•	Настройте базовые параметры коммутатора.  
•	Настройте IP-адрес для ПК.  

Часть 3. Проверка сетевых подключений  
•	Отобразите конфигурацию устройства.  
•	Протестируйте сквозное соединение, отправив эхо-запрос.  
•	Протестируйте возможности удаленного управления с помощью Telnet.  

### Ход работы:  
Коммутатор Cisco 2960 соединяем через консоль с PC-1. Открываем терминал на ПК:  

**enable** заходим в привелигированный режим.  
**show running-configuration** выводим информацию о конфигурационном файле:  

Switch>enable  
Switch#show running-config   
Building configuration...  

Current configuration : 1080 bytes  
!  
version 15.0  
no service timestamps log datetime msec  
no service timestamps debug datetime msec  
no service password-encryption  
!  
hostname Switch  
!  
!  
!  
!  
!  
!  
spanning-tree mode pvst  
spanning-tree extend system-id  
!  
interface FastEthernet0/1  
!   
interface FastEthernet0/2  
!  
interface FastEthernet0/3  
!  
interface FastEthernet0/4  
!  
interface FastEthernet0/5  
!  
interface FastEthernet0/6  
!  
interface FastEthernet0/7  
!  
interface FastEthernet0/8  
!  
interface FastEthernet0/9  
!  
interface FastEthernet0/10  
!  
interface FastEthernet0/11  
!  
interface FastEthernet0/12  
!  
interface FastEthernet0/13  
!  
interface FastEthernet0/14  
!  
interface FastEthernet0/15  
!  
interface FastEthernet0/16  
!  
interface FastEthernet0/17  
!  
interface FastEthernet0/18  
!  
interface FastEthernet0/19  
!  
interface FastEthernet0/20  
!  
interface FastEthernet0/21  
!  
interface FastEthernet0/22  
!  
interface FastEthernet0/23  
!  
interface FastEthernet0/24  
!  
interface GigabitEthernet0/1  
!  
interface GigabitEthernet0/2  
!  
interface Vlan1  
 no ip address  
 shutdown  
!  
!  
!  
!  
line con 0  
!  
line vty 0 4  
 login  
line vty 5 15  
 login  
!  
!  
!  
!  
end  

**Из конфигурации видим, что у нас 24 интерфейса FastEthernet и 2 Gigabit Ethernet, Line VTY с 0 по 15 = 16 линий телетайпа. VLAN1 не задан ip, сам интерфейс выключен. Версия IOS 15, временная метка в логгировании системных и отладочных сообщениях отключена, отключено шифрование паролей**  

Убеждаемся, что загрузочный конфиг отсутствует на устройстве:  
Switch#show startup-config   
startup-config is not present  

### Подключаем PC-A витой парой к 6 порту Cisco  
В терминале появляется сообщение:  
Switch#
%LINK-5-CHANGED: Interface FastEthernet0/6, changed state to up  

%LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/6, changed state to up  

Смотрим свойства интерфейса:  
Switch#show interfaces f 0/6  
FastEthernet0/6 is up, line protocol is up (connected)  
  Hardware is Lance, address is 0006.2ae6.7306 (bia 0006.2ae6.7306)  
 BW 100000 Kbit, DLY 1000 usec,  
     reliability 255/255, txload 1/255, rxload 1/255  
  Encapsulation ARPA, loopback not set  
  Keepalive set (10 sec)  
  Full-duplex, 100Mb/s  
  input flow-control is off, output flow-control is off  
  ARP type: ARPA, ARP Timeout 04:00:00  
  Last input 00:00:08, output 00:00:05, output hang never  
  Last clearing of "show interface" counters never  
  Input queue: 0/75/0/0 (size/max/drops/flushes); Total output drops: 0  
  Queueing strategy: fifo  
  Output queue :0/40 (size/max)  
  5 minute input rate 0 bits/sec, 0 packets/sec  
  5 minute output rate 0 bits/sec, 0 packets/sec  
     956 packets input, 193351 bytes, 0 no buffer  
     Received 956 broadcasts, 0 runts, 0 giants, 0 throttles  
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored, 0 abort  
     0 watchdog, 0 multicast, 0 pause input  
     0 input packets with dribble condition detected  
     2357 packets output, 263570 bytes, 0 underruns  
     0 output errors, 0 collisions, 10 interface resets  
     0 babbles, 0 late collision, 0 deferred  
     0 lost carrier, 0 no carrier  
     0 output buffer failures, 0 output buffers swapped out  

**Интерфейс включен (no shutdown), виден хост, интерфейс работает на максимальной для себя скорости 100Mb/s**

Выводим информацию о флеш-памяти:  

Switch# show flash:   
Directory of flash:/  
  
    1  -rw-     4670455          <no date>  2960-lanbasek9-mz.150-2.SE4.bin  
  
64016384 bytes total (59345929 bytes free)   

##  Настройка базовых параметров сетевых устройств 

Switch#configure terminal   
Enter configuration commands, one per line.  End with CNTL/Z.  
Switch(config)#no ip domain-lookup  
Switch(config)#hostname S1  
S1(config)#service password-encryption   
S1(config)#banner motd #  
Enter TEXT message.  End with the character '#'.  
!!!!!!!!GO AWAY NOW!!!!!!#  
  
S1(config)#interface vlan1  
S1(config-if)#ip address 192.168.1.2 255.255.255.0  
S1(config-if)#no shutdown   
  
S1(config-if)#  
%LINK-5-CHANGED: Interface Vlan1, changed state to up  
  
%LINEPROTO-5-UPDOWN: Line protocol on Interface Vlan1, changed state to up  
  
S1(config-if)#exit  

**Задал ip для SVI, чтобы подключиться через Telnet. Усановил пароль, установил требование пароля при подючении.**

Задал для PC-1 адрес 192.168.1.10/24. 

Смотрю текущую конфигурацию:  
  
S1#show run  
Building configuration...  
  
Current configuration : 1249 bytes  
!  
version 15.0  
no service timestamps log datetime msec  
no service timestamps debug datetime msec  
service password-encryption  
!  
hostname S1  
!  
enable secret 5 $1$mERr$9cTjUIEqNGurQiFU.ZeCi1  
!  
!  
!  
no ip domain-lookup  
!  
!  
!    
spanning-tree mode pvst  
spanning-tree extend system-id  
!  
interface FastEthernet0/1  
!  
interface FastEthernet0/2  
!  
interface FastEthernet0/3  
!  
interface FastEthernet0/4  
!  
interface FastEthernet0/5  
!  
interface FastEthernet0/6  
!  
interface FastEthernet0/7  
!  
interface FastEthernet0/8  
!  
interface FastEthernet0/9  
!  
interface FastEthernet0/10  
!  
interface FastEthernet0/11  
!  
interface FastEthernet0/12  
!  
interface FastEthernet0/13  
!  
interface FastEthernet0/14  
!  
interface FastEthernet0/15  
!  
interface FastEthernet0/16  
!  
interface FastEthernet0/17  
!  
interface FastEthernet0/18  
!  
interface FastEthernet0/19  
!  
interface FastEthernet0/20  
!  
interface FastEthernet0/21  
!  
interface FastEthernet0/22  
!  
interface FastEthernet0/23  
!  
interface FastEthernet0/24  
!  
interface GigabitEthernet0/1  
!  
interface GigabitEthernet0/2  
!  
interface Vlan1  
 ip address 192.168.1.2 255.255.255.0  
!  
banner motd ^C  
!!!!!!!!GO AWAY NOW!!!!!!^C  
!  
!  
!  
line con 0   
 password 7 0822455D0A16  
 logging synchronous  
 login  
!  
line vty 0 4  
 password 7 0822455D0A16  
 login  
line vty 5 15  
 password 7 0822455D0A16  
 login   
!    
!  
!  
!  
end    

##  Тест сквозного соединения

Через командную строку PC-A пингую коммутатор и SVI:  
C:\>ping 192.168.1.10  
  
Pinging 192.168.1.10 with 32 bytes of data:  
  
Reply from 192.168.1.10: bytes=32 time=28ms TTL=128  
Reply from 192.168.1.10: bytes=32 time=2ms TTL=128  
Reply from 192.168.1.10: bytes=32 time=2ms TTL=128  
Reply from 192.168.1.10: bytes=32 time<1ms TTL=128  
  
Ping statistics for 192.168.1.10:  
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),  
Approximate round trip times in milli-seconds:  
    Minimum = 0ms, Maximum = 28ms, Average = 8ms  
  
C:\> ping 192.168.1.2  
  
Pinging 192.168.1.2 with 32 bytes of data:  
  
Reply from 192.168.1.2: bytes=32 time<1ms TTL=255  
Reply from 192.168.1.2: bytes=32 time<1ms TTL=255  
Reply from 192.168.1.2: bytes=32 time<1ms TTL=255  
Reply from 192.168.1.2: bytes=32 time<1ms TTL=255  
  
Ping statistics for 192.168.1.2:  
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),  
Approximate round trip times in milli-seconds:  
    Minimum = 0ms, Maximum = 0ms, Average = 0ms  

 **Успех**      

  ##  Проверка удаленного управления коммутатором S1

  Подлючаемся с помощью Telnet:  
Trying 192.168.1.2 ...Open  
!!!!!!!!GO AWAY NOW!!!!!!  
  
  
User Access Verification  
    
Password:   
S1>enable  
Password:   
S1#wr  
Building configuration...  
[OK]  
S1# exit  
