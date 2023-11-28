# demo2024
| Имя устройства | Интерфейс   | IPv4/IPv6        |    Маска/Префикс    | Шлюз         |
| -----------    | ----------- |------------      |   ---------------   |------        |
| ISP            | ens192      | 10.12.14.10      |  255.255.255.0/24   | 10.12.14.254 |
|                | ens256      | 192.168.0.162    |  255.255.255.252/30 |              |
|                | ens224      | 192.168.0.166    |  255.255.255.252/30 |              |
| HQ-R           | ens192      | 192.168.0.161    |  255.255.255.252/30 | 192.168.0.162|
|                | ens224      | 192.168.0.1      |  255.255.255.128/25 |              |
| BR-R           | ens192      | 192.168.0.165    |  255.255.255.252/30 | 192.168.0.166|
|                | ens224      | 192.168.0.129    |  255.255.255.224/27 |              |
| HQ-SRV         | ens192      | 192.168.0.126    |  255.255.255.128/25 | 192.168.0.1  |
| BR-SRV         | ens192      | 192.168.0.158    |  255.255.255.224/27 | 192.168.0.129|

![image](https://github.com/angelinaaaan999/demo2024/assets/148867770/4b546c52-dfdc-41d0-a312-1b1462759528)

Захожу в файл конфигурации:
```
nano /etc/network/interfaces
```
Ввожу IP-адреса, маску подсети и шлюз по умолчанию по следующему образцу:
```
auto ens192
iface ens192 inet static
        address 10.12.14.10
        netmask 255.255.255.0.
        gateway 10.12.34.254

auto ens256
iface ens256 inet static
        address 192.168.0.162
        netmask 255.255.255.252
        
auto ens224
iface ens224 inet static
        address 192.168.0.166
        netmask 255.255.255.252
```
Далее сохраняю:
```
ctrl + s
```
Далее выхожу из файла конфигурации:
```
ctrl + x
```
И перезагрузил виртуальную машину:
```
systemctl reboot
```
Точно также настроил остальные виртуальные машины.

# №1.2
Настройте внутреннюю динамическую маршрутизацию по средствам FRR. Выберите и обоснуйте выбор протокола динамической маршрутизации из расчёта, что в дальнейшем сеть будет масштабироваться.
# Ход выполнения работы:
Установил пакет FRR:
```
apt-get install frr
```
Проверил состояние:
```
systemctl status frr
```
Вошёл в файл:
```
nano /etc/frr/daemons
```
Изменил значение:
```
ospfd=yes
```
Перезагрузил службу FRR:
```
systemctl restart frr
```
Вошёл в настройку маршрутизации на ISP:
```
vtysh
```
Вхожу в конфигурацию терминала, затем запускаю процесс и добавляю интерфейсы:
```
conf t
router ospf
 network 192.168.1.162/30 area 0
 network 192.168.2.166/30 area 0
```
Просматриваю соседей:
```
show ip ospf neighbor
```
Такие же действия повторяю с HQ-R и BR-R.

# №1.3
Настройте автоматическое распределение IP-адресов на роутере HQ-R.
# Ход выполнения работы:
