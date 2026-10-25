*This project has been created as part of the 42 curriculum by osousa-d.*

# NetPractice

## Description
This project focuses on understanding computer networks and IP addresses, connecting devices through routers, and understanding the role of gateways in a network. It also covers subnetting concepts, including IP masks, CIDR notation, block sizes, and network address ranges.


--------------

### 1. Equipamentos de rede
- PC (Host)
-- Um PC, também chamado de host, é um dispositivo conectado à rede. Ele possui uma interface de rede configurada com um endereço IP e uma máscara de sub-rede.
-- O gateway padrão é utilizado quando o destino está fora da rede local e o host precisa encaminhar o pacote para um roteador.

Exemplo de configuração:

Campo

Valor

IP

159.192.109.2

Máscara

255.255.255.192 (/26)

Gateway

159.192.109.1

Switch

O switch conecta vários dispositivos dentro de uma rede local (LAN). Ele encaminha quadros de rede entre suas interfaces, utilizando endereços MAC para identificar os dispositivos.

Importante: conectar dois PCs ao mesmo switch não garante que eles consigam se comunicar diretamente.

Para a comunicação direta entre dois hosts IPv4 funcionar, eles normalmente precisam estar na mesma sub-rede, além de possuir configurações válidas. Se estiverem em sub-redes diferentes, precisam de um roteador para encaminhar os pacotes entre elas.

Exemplo:
PC A: 192.168.1.10/24
PC B: 192.168.1.20/24
Os dois pertencem à rede 192.168.1.0/24 e podem se comunicar diretamente pelo switch.
Agora, se o PC B utilizar 192.168.2.20/24, ele estará em outra sub-rede. Mesmo conectado ao mesmo switch, a comunicação entre as sub-redes precisará de roteamento.
Roteador (Router)
O roteador conecta redes IP diferentes e encaminha pacotes entre elas.
Cada interface do roteador que participa de uma rede possui seu próprio endereço IP e sua própria máscara.
Quando um pacote chega ao roteador, ele consulta sua tabela de rotas para decidir por qual interface deve encaminhá-lo e qual será o próximo salto, quando necessário.
Exemplo:
Rede local: 192.168.1.0/24
Interface do roteador nessa rede: 192.168.1.1
Gateway configurado nos PCs: 192.168.1.1
Quando um PC precisa acessar um destino fora da rede local, ele envia o pacote ao gateway.
Internet
A Internet é uma rede global formada pela interconexão de inúmeras redes.
Em um exercício do NetPractice, o elemento Internet representa um destino externo. Para alcançar esse destino, os roteadores precisam ter rotas adequadas.
Também é necessário existir um caminho de retorno para que as respostas cheguem ao host que iniciou a comunicação.

-------------------

Explicar conceitos simples
Explicar pc, switchs, roteadores, internet
em switchs explicar que os pcs conectados em diferentes interfaces precisam estar na mesma rede para conversarem entre si mesmo conectados ao mesmo switch


As 3 Faixas Reservadas (RFC 1918):
| Classe | Faixa de IPs | Bloco / Máscara | Uso Comum |
|------|---|---|---|
| Classe A | 10.0.0.0 até 10.255.255.255 | 10.0.0.0/8 | Grandes empresas e data centers |
| Classe B | 172.16.0.0 até 172.31.255.255 | 172.16.0.0/12 | Redes corporativas de médio/grande porte |
| Classe C | 192.168.0.0 até 192.168.255.255 | 192.168.0.0/16 | Roteadores domésticos e pequenas redes |

explicar também separações
Rede do Host Three: 159.192.109.4/30
Endereço	Função
159.192.109.4	Endereço da rede
159.192.109.5	Host utilizável
159.192.109.6	Host Three
159.192.109.7	Broadcast


ANOTAÇÂO: Converter IP de Rede

### Converter NÚMERO para "/X" (CIDR):
Pegar o número "mágico" **(256)** e **subtrair** pelo número da máscara agora dobrar o número 1 até virar o resultado da subtração pegar o limite máximo de bits (32) e subtrair por quantas vezes o 1 foi multiplicado

### Como descobrir o número total de endereços de um bloco através da notação CIDR (ou prefixo):
Um **IPV4** possui **32 bits** no total
255.255.255.255 = (8 bits + 8 bits + 8 bits + 8 bits)

### Conversao de um CIDR "/X" para mascara:
CIDR e quantos bits tem aquela mascara:
CIDR /30 significa que temos 30 bits ligados
Contamos da esquerda para a direita 
MONTAR TABELA com 255.255.255.




vamos usar de exemplo a **CIDR /18**:
Passo 1: /18
Passo 2: 18 - 32 = 14
Passo 3: 2¹⁴ = 16384 endereços

### Como saber o tamanho do bloco em um octeto da máscara:
ex: /18 é 16 bits + 2 bits ou seja 16 = "255.255." e esses 2 bits no terceiro octeto fica 11000000, cada casa dos bits equivalem a um valor, no exemplo fica 128 + 64 que dá 192, logo /18 é 255.255.192.0

### Como descobrir o tamanho do bloco do terceiro octeto (255.255."192".0)?
Cada octeto de um IPV4 equivalem a 256 valores diferentes, pois cada casa de bits possuem valores:

| Bit  | 1 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | <- 8 bits totais |
|------|---|---|---|---|---|---|---|---|---|
| Valor | 128 | 64 | 32 | 16 | 8 | 4 | 2 | 1 | <- 256 valores totais |

(nesse exemplo acima somente a primeira casa ta ligada, ou seja o valor decimal desse bit e 128)

Tabelinha pra usar na calculadora: 128 + 64 + 32 + 16 + 8 + 4 + 2 + 1 

Se "192" é "11000000" em bits, o calculo para sabermos o espaço nesse octeto é:
192 - 256 = 64 cada bloco

### CIDR "/X" para NÚMERO:
Quantos bits faltam para chegar no número "32" (subtração) pegar o número 1 e dobra X vezes pelo resultado da subtração = Tamanho do bloco fazer 256 menos o tamanho do bloco = o número da máscara
**Vamos usar de exemplo o CIDR /18**
Passo 1: /18
Passo 2: 18 - 32 = 14
Passo 3: 
Descobrir tamanho do bloco: Cenário A: Você tem o número da máscara (ex: 224) Descubra o tamanho do bloco: 256 - 224 = 32 Divida 256 pelo tamanho deo bloco: 256 / 32 = 8

Cenário B: Você tem o /X (ex: /30) Veja quantos bits faltam para 32: 32 - 30 = 2 Dobre o número 1 aquela quantidade de vezes (2 vezes): 1 -> 2 -> 4 (tamanho do bloco). Divida 256 pelo tamanho do bloco: 256 / 4 = 64

QUERO DIVIDIR EM X VEZES ex: 4 256 / 4 = 64 256 - 64 = (192)
255.255.
11111110


Nesse projeto, mais um dos conceitos que eu entendi e a definicao das rotas de destinos e o proximo salto, o que significa isso? 

- Rede de Destino (Destination): Qual endereço/rede o pacote está buscando. Aqui defino o caminho final
- Próximo Salto (Next Hop / Gateway): Qual o IP do próximo roteador que vai receber esse pacote.

Foi preciso entender a rede de dest

---------------

expicar 127.0.0.0/8 loopback

python3 -m http.server 8080

site auxiliar https://www-freecodecamp-org.translate.goog/news/subnet-cheat-sheet-24-subnet-mask-30-26-27-29-and-other-ip-address-cidr-network-references/?_x_tr_sl=en&_x_tr_tl=pt&_x_tr_hl=pt&_x_tr_pto=tc
