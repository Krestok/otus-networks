# Базовая настройка сетевых устройств и концепция коммутации

Цель: Настройка DTP Добавление сетей VLAN и назначение портов
В этой лабораторной работе вы настроите магистральные каналы между этими коммутаторами

![](C:\Users\Админ\Documents\GitHub\otus-networks\homework\4.2.8 scheme.JPG)



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
Router>enable

b. Войдите в режим конфигурации.
Router# configure terminal

c. Назначьте имя устройства для маршрутизатора.
Router(config)#hostname R1

d. Отключите поиск DNS, чтобы маршрутизатор не пытался переводить неправильно введенные команды, как если бы они были именами хостов.
R1(config)#no ip domain lookup

e. Назначьте class в качестве привилегированного зашифрованного пароля EXEC.
R1(config)#enable secret class

f. Назначьте cisco в качестве пароля консоли и включите вход в систему.
R1(config)#line console 0
R1(config-line)#password cisco
R1(config-line)#login

g. Назначьте cisco в качестве пароля VTY и включите вход.
R1(config)#line vty 0 4
R1(config-line)#password cisco
R1(config-line)#login

h. Зашифруйте незашифрованные пароли.
R1(config)#service password-encryption

i. Создайте баннер, который предупреждает любого, кто получает доступ к устройству, о том, что несанкционированный доступ запрещен.
R1(config)#banner motd #Unauthorized access is prohibited#

j. Сохраните текущую конфигурацию в файл конфигурации запуска.
R1#copy running-config startup-config

k. Установите часы на роутере.
R1#clock set 11:20:00 09 jul 2020


### Step 2: Configure basic settings for each switch.

Открыть окно конфигурации
a. Консоль в коммутатор и включить привилегированный режим EXEC.
Switch>enable

b. Войдите в режим конфигурации.
Switch#configure terminal

c. Назначьте имя устройства для коммутатора.
Switch(config)#hostname S1
Switch(config)#hostname S2

d. Отключите поиск DNS, чтобы маршрутизатор не пытался переводить неправильно введенные команды, как если бы они были именами хостов.
S1(config)#no ip domain-lookup
S2(config)#no ip domain-lookup

e. Назначьте class в качестве привилегированного зашифрованного пароля EXEC.
S1(config)#enable secret class
S2(config)#enable secret class

f. Назначьте cisco в качестве пароля консоли и включите вход в систему.
S1(config)#line console 0
S1(config-line)#password cisco
S1(config-line)#login
S2(config)#line console 0
S2(config-line)#password cisco
S2(config-line)#login


g. Назначьте cisco в качестве пароля vty и включите вход в систему.
S1(config)#line vty 0 4
S1(config-line)#password cisco
S1(config-line)#login
S2(config)#line vty 0 4
S2(config-line)#password cisco
S2(config-line)#login

h. Зашифруйте незашифрованные пароли.
S1(config)#service password-encryption
S2(config)#service password-encryption

i. Создайте баннер, который предупреждает любого, кто получает доступ к устройству, о том, что несанкционированный доступ запрещен.
S1(config)#banner motd #Unauthorized access is prohibited#
S2(config)#banner motd #Unauthorized access is prohibited#

j. Установите часы на выключателе.
S1#clock set 11:38:20 09 jul 2020
S2#clock set 11:39:40 09 jul 2020

к. Скопируйте рабочую конфигурацию в конфигурацию запуска.
S1#copy running-config startup-config
S2#copy running-config startup-config

### Step 3: Create VLANs and Assign Switch Ports
a.	Create and name the required VLANs on each switch from the table above.
S1(config)#vlan 3
S1(config-vlan)#name Management
S1(config)#vlan 4
S1(config-vlan)#name Operations
S1(config)#vlan 7
S1(config-vlan)#name ParkingLot
S1(config)#vlan 8
S1(config-vlan)#name Native

S2(config)#vlan 3
S2(config-vlan)#name Management
S2(config)#vlan 4
S2(config-vlan)#name Operations
S2(config)#vlan 7
S2(config-vlan)#name ParkingLot
S2(config)#vlan 8
S2(config-vlan)#name Native

b.	Configure the management interface and default gateway on each switch using the IP address information in the Addressing Table.
S1(config)#ip default-gateway 192.168.3.1
S1(config)#interface vlan 3
S1(config-if)#ip address 192.168.3.11 255.255.255.0
S1(config-if)#no shutdown

S2(config)#ip default-gateway 192.168.3.1
S2(config)#interface vlan 3
S2(config-if)#ip address 192.168.3.11 255.255.255.0
S2(config-if)#no shutdown
