*This project has been created as part of the 42 curriculum by osousa-d.*

# NetPractice

## Description
This project focuses on understanding computer networks and IP addresses, connecting devices through routers, and understanding the role of gateways in a network. It also covers subnetting concepts, including IP masks, CIDR notation, block sizes, and network address ranges.


--------------
Explicar conceitos simples
Explicar pc, switchs, roteadores, internet
em switchs explicar que os pcs conectados em diferentes interfaces precisam estar na mesma rede para conversarem entre si mesmo conectados ao mesmo switch


FAZER UMA TABELA
As 3 Faixas Reservadas (RFC 1918):ClasseFaixa de IPsBloco / MáscaraUso ComumClasse A10.0.0.0 até 10.255.255.25510.0.0.0/8Grandes empresas e data centersClasse B172.16.0.0 até 172.31.255.255172.16.0.0/12Redes corporativas de médio/grande porteClasse C192.168.0.0 até 192.168.255.255192.168.0.0/16Roteadores domésticos e pequenas redes


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

Tabelinha pra usar na calculadora: c + 2 + 1 

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
