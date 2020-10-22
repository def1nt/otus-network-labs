# DHCPv6
**Задачи**
1. Build the Network and Configure Basic Device Settings
2. Verify SLAAC address assignment from R1
3. Configure and verify a Stateless DHCPv6 Server on R1
4. Configure and verify a Stateful DHCPv6 Server on R1
5. Configure and verify a DHCPv6 Relay on R2

**Ход решения**

![Здесь должна быть картина со схемой сети](Lab3-DHCP-Topology.png)
*Используемая при выполнении работы топология.*

**Адресация в сети**

Устройство | Интерфейс | IPv6-адрес
-|-|-
R1   | G0/0/0 | 2001:db8:acad:2::1 /64
-   | G0/0/0 | fe80::1
-   | G0/0/1 | 2001:db8:acad:1::1/64
-   | G0/0/1 | fe80::1
R2   | G0/0/0 | 2001:db8:acad:2::2/64
-   | G0/0/0 | fe80::2
-   | G0/0/1 | 2001:db8:acad:3::1 /64
-   | G0/0/1 | fe80::1
PC-A | NIC    | DHCP
PC-B | NIC    | DHCP



## Часть 1. Построить сеть и настроить сетевые устройства

### Настроим базовые параметры коммутаторов


Assign a device name to the switch.
Disable DNS lookup to prevent the router from attempting to translate incorrectly entered commands as though they were host names.
Assign class as the privileged EXEC encrypted password.
Assign cisco as the console password and enable login.
Assign cisco as the VTY password and enable login.
Encrypt the plaintext passwords.
Create a banner that warns anyone accessing the device that unauthorized access is prohibited.
Shutdown all unused ports
Save the running configuration to the startup configuration file.

### Настроим базовые параметры маршрутизаторов
Step 3: Configure basic settings for each router.
Assign a device name to the router.
Disable DNS lookup to prevent the router from attempting to translate incorrectly entered commands as though they were host names.
Assign class as the privileged EXEC encrypted password.
Assign cisco as the console password and enable login.
Assign cisco as the VTY password and enable login.
Encrypt the plaintext passwords.
Create a banner that warns anyone accessing the device that unauthorized access is prohibited.
Enable IPv6 Routing
Save the running configuration to the startup configuration file.
