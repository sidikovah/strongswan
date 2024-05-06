ansible-playbook -u admin -l ipsec-01.pslab.cmp.pkz.icdc.io pcloud-strongswan.yaml 

ansible-playbook -u ubuntu --private-key=/home/user/.ssh/id_rsa  -i /home/user/strongswan-main/hosts.yml   pcloud-strongswan.yaml 

После поднятия тунеля нужно прописать статические маршруты в нужные удаленные сети через виртуалку со strongswan

Зачастую для того чтобы зарулить входящий трафик в тунель нужно настроить SNAT на сервер ipsec


# Пример для firewall-cmd
firewall-cmd --permanent --direct --add-rule ipv4 nat POSTROUTING 0 -s 0.0.0.0/0 -d 172.25.43.91/32 -p TCP -j SNAT --to-source 172.31.253.7
# Пример для ubuntu
iptables -t nat -I POSTROUTING -s 0.0.0.0/0 -d 172.25.43.91/32 -p TCP -j SNAT --to-source 172.31.253.7

172.25.43.91/32 - адрес удаленной стороны rightsubnet

172.31.253.7 - наш адрес прописанный в тунеле leftsubnet

Ссылка на документацию: https://wiki.strongswan.org/projects/strongswan/wiki

На виртуалке должны быть интерфейсы сетей куда нужен будет доступ, включен Ip_forwarding и настроены соответсвующие правила файрвола.

Должны быть открыты порты: 500/udp и 4500/udp

В некоторых случаях требуется натить адреса.
