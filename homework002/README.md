# Базовая настройка сетевых устройств и концепция коммутации

Цель: Настройка DTP Добавление сетей VLAN и назначение портов
В этой лабораторной работе вы настроите магистральные каналы между этими коммутаторами

![](4.2.8%20scheme.png)



###### Addressing Table
| Device | Interface | IP Address   | Subnet Mask   | Default  Gateway |
| ------ | --------- | ------------ | ------------- | ---------------- |
| R1     | G0/0/1.3  | 192.168.3.1  | 255.255.255.0 | N/A              |
| R1     | G0/0/1.4  | 192.168.4.1  | 255.255.255.0 | N/A              |
| R1     | G0/0/1.8  | N/A          | N/A           | N/A              |
| S1     | VLAN 3ex  | 192.168.3.11 | 255.255.255.0 | 192.168.3.1      |
| S2     | VLAN 3    | 192.168.3.12 | 255.255.255.0 | 192.168.3.1      |
| PC-A   | NIC       | 192.168.3.3  | 255.255.255.0 | 192.168.3.1      |
| PC-B   | NIC       | 192.168.4.3  | 255.255.255.0 | 192.168.4.1      |

###### VLAN Table

| VLAN | Name       | Interface  Assigned                                         |
| ---- | ---------- | ----------------------------------------------------------- |
| 3    | Management | S1: VLAN 3  S2: VLAN 3  S1: F0/6                            |
| 4    | Operations | S2: F0/18                                                   |
| 7    | ParkingLot | S1: F0/2-4, F0/7-24, G0/1-2   S2: F0/2-17, F0/19-24, G0/1-2 |
| 8    | Native     | N/A                                                         |

### Step 1: Configure basic settings for the router.
a. Консоль в маршрутизатор и включить привилегированный режим EXEC.

```
Router>enable
```



b. Войдите в режим конфигурации.

```
Router# configure terminal
```



c. Назначьте имя устройства для маршрутизатора.

```
Router(config)#hostname R1
```



d. Отключите поиск DNS, чтобы маршрутизатор не пытался переводить неправильно введенные команды, как если бы они были именами хостов.

```
R1(config)#no ip domain lookup
```



e. Назначьте class в качестве привилегированного зашифрованного пароля EXEC.

```
R1(config)#enable secret class
```



f. Назначьте cisco в качестве пароля консоли и включите вход в систему.

```
R1(config)#line console 0

R1(config-line)#password cisco

R1(config-line)#login
```



g. Назначьте cisco в качестве пароля VTY и включите вход.

```
R1(config)#line vty 0 4

R1(config-line)#password cisco

R1(config-line)#login
```



h. Зашифруйте незашифрованные пароли.

```
R1(config)#service password-encryption


```

i. Создайте баннер, который предупреждает любого, кто получает доступ к устройству, о том, что несанкционированный доступ запрещен.

```
R1(config)#banner motd #Unauthorized access is prohibited#
```



j. Сохраните текущую конфигурацию в файл конфигурации запуска.

```
R1#copy running-config startup-config
```



k. Установите часы на роутере.

```
R1#clock set 11:20:00 09 jul 2020
```




### Step 2: Configure basic settings for each switch.

Открыть окно конфигурации

a. Консоль в коммутатор и включить привилегированный режим EXEC.

```
Switch>enable
```

b. Войдите в режим конфигурации.

```
Switch#configure terminal
```

c. Назначьте имя устройства для коммутатора.

```
Switch(config)#hostname S1

Switch(config)#hostname S2
```

d. Отключите поиск DNS, чтобы маршрутизатор не пытался переводить неправильно введенные команды, как если бы они были именами хостов.

```
S1(config)#no ip domain-lookup

S2(config)#no ip domain-lookup
```

e. Назначьте class в качестве привилегированного зашифрованного пароля EXEC.

```
S1(config)#enable secret class

S2(config)#enable secret class
```

f. Назначьте cisco в качестве пароля консоли и включите вход в систему.

```
S1(config)#line console 0

S1(config-line)#password cisco

S1(config-line)#login

S2(config)#line console 0

S2(config-line)#password cisco

S2(config-line)#login
```



g. Назначьте cisco в качестве пароля vty и включите вход в систему.

```
S1(config)#line vty 0 4

S1(config-line)#password cisco

S1(config-line)#login

S2(config)#line vty 0 4

S2(config-line)#password cisco

S2(config-line)#login
```



h. Зашифруйте незашифрованные пароли.

```
S1(config)#service password-encryption

S2(config)#service password-encryption
```



i. Создайте баннер, который предупреждает любого, кто получает доступ к устройству, о том, что несанкционированный доступ запрещен.

```
S1(config)#banner motd #Unauthorized access is prohibited#

S2(config)#banner motd #Unauthorized access is prohibited#
```



j. Установите часы на выключателе.

```
S1#clock set 11:38:20 09 jul 2020

S2#clock set 11:39:40 09 jul 2020
```



к. Скопируйте рабочую конфигурацию в конфигурацию запуска.

```
S1#copy running-config startup-config

S2#copy running-config startup-config
```



### Step 3: Create VLANs and Assign Switch Ports

a.	Create and name the required VLANs on each switch from the table above.

```
S1(config)#vlan 3

S1(config-vlan)#name Management

S1(config)#vlan 4

S1(config-vlan)#name Operations

S1(config)#vlan 7

S1(config-vlan)#name ParkingLot

S1(config)#vlan 8

S1(config-vlan)#name Native
```



```
S2(config)#vlan 3

S2(config-vlan)#name Management

S2(config)#vlan 4

S2(config-vlan)#name Operations

S2(config)#vlan 7

S2(config-vlan)#name ParkingLot

S2(config)#vlan 8

S2(config-vlan)#name Native
```



b.	Configure the management interface and default gateway on each switch using the IP address information in the Addressing Table.

```
S1(config)#ip default-gateway 192.168.3.1

S1(config)#interface vlan 3

S1(config-if)#ip address 192.168.3.11 255.255.255.0

S1(config-if)#no shutdown



S2(config)#ip default-gateway 192.168.3.1

S2(config)#interface vlan 3

S2(config-if)#ip address 192.168.3.11 255.255.255.0

S2(config-if)#no shutdown
```



### Step 4 : Configure an 802.1Q Trunk Between the Switches

Конфигурируем порты F0/1и F0/5 коммутатора S1 и порт F0/1 коммутатора S2 в TRUNK порты

```
S1(config)#interface fa0/1

S1(config-if)#switchport mode trunk

S1(config-if)#switchport trunk native vlan 8
```

  (Устанавливаем Native VLAN 8 на обоих коммутаторах)

```
S1(config-if)#switchport trunk allowed vlan 3,4,8
```

 (Разрешаем передачу данных по trunk порту vlan 3,4,8)



### Step 5: Configure Inter-VLAN Routing on the Router

a.   Активируем интерфейс  G0/0/1 на маршрутизаторе R1.

```
R1(config)#interface g0/0/1

R1(config-if)#no shutdown
```

b. Создаем субинтерфейсы G0/0/1.3, G0/0/1.4, G0/0/1.8

```
R1(config)#interface GigabitEthernet0/0/1.3

R1(config-subif)#description Default Gateway for Vlan 3

R1(config-subif)#encapsulation dot1Q 3

R1(config-subif)#ip address 192.168.3.1 255.255.255.0

R1(config-subif)#no shutdown
```

**!**

```
R1(config)#interface GigabitEthernet0/0/1.4

R1(config-subif)#description Default Gateway for Vlan 4

R1(config-subif)#encapsulation dot1Q 4

R1(config-subif)#ip address 192.168.4.1 255.255.255.0

R1(config-subif)#no shutdown
```

**!**

```
R1(config)#interface GigabitEthernet0/0/1.8

R1(config-subif)#encapsulation dot1Q 8 native

R1(config-subif)#no shutdown
```



Complete the following tests from PC-A. All should be successful

##### a.   Ping from PC-A to its default gateway.

```
C:\>ping 192.168.3.1



Pinging 192.168.3.1 with 32 bytes of data:



Reply from 192.168.3.1: bytes=32 time=31ms TTL=255

Reply from 192.168.3.1: bytes=32 time<1ms TTL=255

Reply from 192.168.3.1: bytes=32 time<1ms TTL=255

Reply from 192.168.3.1: bytes=32 time<1ms TTL=255



Ping statistics for 192.168.3.1:

​    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),

Approximate round trip times in milli-seconds:

​    Minimum = 0ms, Maximum = 31ms, Average = 7ms
```



##### b.   Ping from PC-A to PC-B

```
C:\>ping 192.168.4.3



Pinging 192.168.4.3 with 32 bytes of data:



Request timed out.

Reply from 192.168.4.3: bytes=32 time<1ms TTL=127

Reply from 192.168.4.3: bytes=32 time<1ms TTL=127

Reply from 192.168.4.3: bytes=32 time<1ms TTL=127



Ping statistics for 192.168.4.3:

​    Packets: Sent = 4, Received = 3, Lost = 1 (25% loss),

Approximate round trip times in milli-seconds:

​    Minimum = 0ms, Maximum = 0ms, Average = 0ms
```



##### c.   Ping from PC-A to S2

```
C:\>ping 192.168.3.12



Pinging 192.168.3.12 with 32 bytes of data:



Request timed out.

Reply from 192.168.3.12: bytes=32 time=1ms TTL=255

Reply from 192.168.3.12: bytes=32 time=1ms TTL=255

Reply from 192.168.3.12: bytes=32 time<1ms TTL=255



Ping statistics for 192.168.3.12:

​    Packets: Sent = 4, Received = 3, Lost = 1 (25% loss),

Approximate round trip times in milli-seconds:

​    Minimum = 0ms, Maximum = 1ms, Average = 0ms
```



##### From the command prompt on PC-B, issue the tracert command to the address of PC-A.

```
C:\>tracert 192.168.3.3



Tracing route to 192.168.3.3 over a maximum of 30 hops: 



  1   0 ms      0 ms      0 ms      192.168.4.1

  2   0 ms      0 ms      0 ms      192.168.3.3



Trace complete.
```


[Конфигурации](https://github.com/Krestok//otus-networks//tree/master/homework/lab4.2.8/)

[Схема в Packet Tracer](https://github.com/Krestok//otus-networks//tree/master/homework/lab4.2.8/)
