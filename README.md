# VLANHW
Домашнее задание на тему "Работа с VLAN"
Задание:
Что нужно сделать?
в Office1 в тестовой подсети появляется сервера с доп интерфейсами и адресами
в internal сети testLAN:
testClient1 - 10.10.10.254
testClient2 - 10.10.10.254
testServer1- 10.10.10.1
testServer2- 10.10.10.1
Равести вланами:
testClient1 <-> testServer1
testClient2 <-> testServer2
Между centralRouter и inetRouter "пробросить" 2 линка (общая inernal сеть) и объединить их в бонд, проверить работу c отключением интерфейсовДля начала устанавливаем сервисы для выполнения нашего ДЗ: Vagrant, Ansible, Virtualbox
Установка Vagrant:
wget -O - https://apt.releases.hashicorp.com/gpg | sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(grep -oP '(?<=UBUNTU_CODENAME=).*' /etc/os-release || lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list
sudo apt update && sudo apt install vagrant
Установка Ansible:
apt install software-properties-common
add-apt-repository --yes --update ppa:ansible/ansible
apt install ansible
Установка Virtualbox:
apt install virtualbox
После установки пишем наши скрипты и Vagrantfile.
Делаем запуск и проверяем работу
VLAN
Зайдите на testClient1 и проверьте связь с testServer1:
vagrant ssh testClient1
ping 10.10.10.1
Аналогично для testClient2:
vagrant ssh testClient2
ping 10.10.10.1
Между клиентами разных VLAN (testClient1 → testClient2) пинга не будет, так как они в разных широковещательных доменах.
LACP bond
На inetRouter запустите непрерывный пинг centralRouter:
vagrant ssh inetRouter
ping 192.168.255.2
Откройте второй терминал, зайдите на centralRouter и отключите один из интерфейсов бонда:
vagrant ssh centralRouter
sudo ip link set down bond-slave-1
Интерфейс office1-central
На centralRouter проверьте наличие IP‑адреса:
ip addr show office1
Должен быть 192.168.255.9/30
Всё работает – задание выполнено.
