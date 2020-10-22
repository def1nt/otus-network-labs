# DHCPv4
**Задачи**
1. Build the Network and Configure Basic Device Settings
2. Configure and verify two DHCPv4 Servers on R1
3. Configure and verify a DHCP Relay on R2


**Ход решения**

![Здесь должна быть картина со схемой сети](Lab3-DHCP-Topology.png)
*Используемая при выполнении работы топология.*

**Адресация в сети**

Устройство | Интерфейс | IP-адрес | Маска подсети | Шлюз
-|-|-|-|-
R1 | E0/0	    | 10.0.0.1|	255.255.255.252 |	N/A
- | E0/1	    | N/A	    |    N/A |	N/A
- | E0/1.100	|     |	  |	N/A
- | E0/1.200	|     |	  |	N/A
- | E0/1.1000| N/A |	N/A	| N/A
R2 | E0/0	    | 10.0.0.2|	255.255.255.252 |	N/A
- | E0/1	    |     |	  |	N/A
S1 | VLAN 200	|  	  |    |	 
S2 | VLAN 1	    |     |	  |	 
PC-A |	NIC	    | DHCP    |	DHCP |	DHCP
PC-B |	NIC	    | DHCP    |	DHCP |	DHCP

VLAN Table
VLAN |	Name |	Interface Assigned
-|-|-
1 |	N/A	| S2: E0/0
100 |	Clients |	S1: E0/0
200 |	Management| 	S1: VLAN 200 
999 |	Parking_Lot	| S1: F0/1-4, F0/7-24, G0/1-2
1000 |	Native	| N/A


## Часть 1. Построить сеть и настроить сетевые устройства

Разделим сеть 192.168.1.0/24 на три подсети согласно заданию.

**Подсеть А на 58 хостов для клиентов на R1:**

192.168.1.0/26

Первый адрес для E0/1.100 на R1: 192.168.1.1

Второй адрес для VLAN 200 на S1: 192.168.1.2

**Подсеть B на 28 хостов для менеджмента на R1:**

192.168.1.64/27

Первый адрес для E0/1.200 на R1: 192.168.1.65

Второй адрес для VLAN 1 на S2: 192.168.1.66

**Подсеть C на 12 хостов для клиентов на R2:**

192.168.1.96/28

Первый адрес для E0/1 на R2: 192.168.1.97

### Настроим базовые параметры роутеров

Assign a device name to the router.
```
Switch(config)#hostname R1
```
Disable DNS lookup to prevent the router from attempting to translate incorrectly entered commands as though they were host names.
```
R1(config)#no ip domain lookup
```
Encrypt the plaintext passwords.
```
R1(config)#service password-encryption
```
Create a banner that warns anyone accessing the device that unauthorized access is prohibited.
```
R1(config)#banner motd % Go away %
```
Save the running configuration to the startup configuration file.
```
R1#copy run start
```
Set the clock on the router to today’s time and date.
```
R1#clock set 12:17:00 15 oct 2020
```

### Настроим межвлановый роутинг на R1
```
R1(config)#int e0/1.100
R1(config-subif)#encapsulation dot1q 100
R1(config-subif)#ip address 192.168.1.1 255.255.255.192
R1(config-subif)#description Clients 100 network
```
### Настроим E0/1 на R2, затем E0/0 и статические маршруты для обоих роутеров
```
R2(config)#int e0/1
R2(config-if)#ip addr 192.168.1.97 255.255.255.240
```
```
R1(config)#int e0/0
R1(config-if)#
R1(config-if)#ip addr 10.0.0.1 255.255.255.252
R1(config-if)#no shut

R1(config)#ip route 0.0.0.0 0.0.0.0 10.0.0.2
```

```
Gateway of last resort is 10.0.0.2 to network 0.0.0.0

S*    0.0.0.0/0 [1/0] via 10.0.0.2
```

```
R1(config)#do ping 10.0.0.2
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 10.0.0.2, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 1/1/1 ms
```

### Настроим базовые параметры коммутаторов

Assign a device name to the switch.
```
Switch(config)#host S1
```
Disable DNS lookup to prevent the router from attempting to translate incorrectly entered commands as though they were host names.
```
S1(config)#no ip dom look
```
Encrypt the plaintext passwords.
```
S1(config)#service pass
```
Create a banner that warns anyone accessing the device that unauthorized access is prohibited.
```
S1(config)#banner motd % Go away %
```
Save the running configuration to the startup configuration file.
```
S1#copy run start
```
Set the clock on the switch to today’s time and date.
```
S1#clock set 12:30:00 15 oct 2020
```

```
S1(config)#vlan 100
S1(config-vlan)#name Clients
S1(config-vlan)#vlan 200
S1(config-vlan)#name Management
S1(config-vlan)#vlan 999
S1(config-vlan)#name ParkingLot
```
```
S1(config)#int vlan 200
S1(config-if)#ip addr 192.168.1.2 255.255.255.192
S1(config)#ip default-gateway 10.0.0.1
```


```
S1(config)#int e0/0
S1(config-if)#sw acc vlan 100
S1(config-if)#sw mode access
S1(config-if)#int range e0/2-3
S1(config-if-range)#sw mode acc
S1(config-if-range)#sw acc vlan 999
```

```
S1(config)#int e0/1
S1(config-if)#sw trunk enc dot1q
S1(config-if)#sw mode trunk
S1(config-if)#sw trunk all vlan 100,200,1000
```




