*This project has been created as part of the 42 curriculum by osousa-d.*

# NetPractice

## Description
This project focuses on understanding computer networks and IP addresses, connecting devices through routers, and understanding the role of gateways in a network. It also covers subnetting concepts, including IP masks, CIDR notation, block sizes, and network address ranges.

---

## Instructions

### Como abrir o projeto

Por motivos autorais da 42 não posso colocar o arquivo HTML aqui, mas posso mostrar o fluxo.

Inicialmente vem uma pasta, com os arquivos do projeto, basicamente é um html com js. Baixo a pasta, abro o terminal e entro na pasta pelo terminal, rodo o comando "python3 -m http.server 8080" e acesso o projeto via "http://localhost:8080/" na pagina do navegador, mas você pode tambem só abrir o index com o comando python rodando no terminal.

Comandos:

	python3 -m http.server 8080 

	http://localhost:8080/end.html


### 1. Equipamentos de rede
- **PC (Host)** - Um PC, também chamado de host, é um dispositivo conectado à rede. Ele possui uma interface de rede configurada com um endereço IP e uma máscara de sub-rede.
	
	O gateway padrão é utilizado quando o destino está fora da rede local e o host precisa encaminhar o pacote para um roteador.
	
	Exemplo de configuração:

	| Campo | Valor |
	|---|---|
	| IP | 159.192.109.2 |
	| Máscara | 255.255.255.192 (/26) |
	| Gateway | 159.192.109.1 |
---
- **Switch** - O switch conecta vários dispositivos dentro de uma rede local (LAN). Ele encaminha quadros de rede entre suas interfaces, utilizando endereços MAC para identificar os dispositivos. 

	Importante: conectar dois PCs ao mesmo switch não garante que eles consigam se comunicar diretamente.

	Para a comunicação direta entre dois hosts IPv4 funcionar, eles normalmente precisam estar na mesma sub-rede, além de possuir configurações válidas. Se estiverem em sub-redes diferentes, precisam de um roteador para encaminhar os pacotes entre elas.

	Exemplo:
	- PC A: **192.168.1.10/24**
	- PC B: **192.168.1.20/24**

	Os dois pertencem à rede **192.168.1.0/24** e podem se comunicar diretamente pelo switch.

	Agora, se o PC B utilizar **192.168.2.20/24**, ele estará em outra sub-rede. Mesmo conectado ao mesmo switch, a comunicação entre as sub-redes precisará de roteamento.
---
- **Roteador (Router)** - O roteador conecta redes IP diferentes e encaminha pacotes entre elas.
Cada interface do roteador que participa de uma rede possui seu próprio endereço IP e sua própria máscara.
Quando um pacote chega ao roteador, ele consulta sua tabela de rotas para decidir por qual interface deve encaminhá-lo e qual será o próximo salto, quando necessário.

	Exemplo:
	- Rede local: **192.168.1.0/24**
	- Interface do roteador nessa rede: **192.168.1.1**
	- Gateway configurado nos PCs: **192.168.1.1**

	Quando um PC precisa acessar um destino fora da rede local, ele envia o pacote ao gateway.
---
- **Internet** - A Internet é uma rede global formada pela interconexão de inúmeras redes. Em um exercício do NetPractice, o elemento Internet representa um destino externo. Para alcançar esse destino, os roteadores precisam ter rotas adequadas.
Também é necessário existir um caminho de retorno para que as respostas cheguem ao host que iniciou a comunicação.

-------------------
### 2. Endereço IPv4, máscara e sub-rede
- **Endereço IPv4**

	Um endereço IPv4 possui 32 bits, divididos em quatro octetos de 8 bits.
	
	Exemplo:

	| Máscara: | 255. | 255. | 255. | 255 | |
	|---|---|---|---|---|---|
	| Bits: | 11111111 | 11111111 | 11111111 | 11111111 | <- total 32 bits |

	Outro exemplo:

	- **159.192.109.6**
	- Cada octeto representa um número de **0 a 255**.

---
- **Máscara de sub-rede**

	A máscara indica qual parte do endereço IP identifica a rede e qual parte identifica os dispositivos dentro dela.

	Exemplo:
	
		Um pc com essas configurações:
		- IP: 192.168.1.10
		- Máscara: 255.255.255.0 (CIDR: /24)
	
		Significa que ele está dentro desse bloco:
		- Rede: 192.168.1.0
		- Broadcast: 192.168.1.255

	Na máscara **/24**, os primeiros **24 bits** identificam a rede, enquanto os **8 bits** restantes identificam os endereços dentro dessa rede.

	| Mscara: | 255. | 255. | 255. | 0 | |
	|---|---|---|---|---|---|
	| Bits: | 11111111 | 11111111 | 11111111 | 00000000 | <- total 24 bits |
---
- **Endereço de rede, hosts e broadcast**

	Uma sub-rede IPv4 tradicionalmente possui:
	- Endereço de rede: identifica a própria sub-rede.
	- Endereços de host: endereços normalmente disponíveis para dispositivos.
	- Broadcast: endereço usado para enviar um pacote a todos os dispositivos da sub-rede.
	
	Esses conceitos são importantes para identificar quais endereços pertencem à mesma rede.
---

### 3. As três faixas privadas da RFC 1918

A RFC 1918 define três blocos de endereços IPv4 reservados para uso em redes privadas. Esses endereços não são roteados diretamente pela Internet pública.
As 3 Faixas Reservadas (RFC 1918):

| Classe | Faixa de IPs | Bloco / Máscara | Uso Comum |
|------|---|---|---|
| Classe A | 10.0.0.0 até 10.255.255.255 | 10.0.0.0/8 | Grandes empresas e data centers |
| Classe B | 172.16.0.0 até 172.31.255.255 | 172.16.0.0/12 | Redes corporativas de médio/grande porte |
| Classe C | 192.168.0.0 até 192.168.255.255 | 192.168.0.0/16 | Roteadores domésticos e pequenas redes |

	Observação: as designações Classe A, B e C são históricas. Atualmente, o planejamento de redes utiliza principalmente CIDR, que permite definir blocos de diferentes tamanhos.
---

### 4. Entendendo o CIDR

CIDR é a notação utilizada para indicar quantos bits de um endereço IPv4 pertencem à parte da rede.

	Exemplos:

| CIDR | Máscara | Bits de rede |
|---|---|---|
| /8 | 255.0.0.0 | 8 | 24
| /16 | 255.255.0.0 | 16 | 
| /18 | 255.255.192.0 | 18 
| /24 | 255.255.255.0 | 24 | 8 |
| /26 | 255.255.255.192 | 26 | 6 |
| /30 | 255.255.255.252 | 30 | 2 | 

Quanto maior o prefixo, mais bits são utilizados para identificar a rede e menos bits ficam disponíveis para endereços dentro dela.

### 4.1. Converter CIDR para quantidade de endereços

Um IPv4 possui 32 bits. 

Para descobrir quantos endereços existem em um bloco:
	
	Quantidade de endereços = 2^(32 − prefixo)

	
- Exemplo com /18:
	
	- Passo 1: Prefixo: /18
	- Passo 2: Bits restantes: 32 − 18 = 14
	- Passo 3: Quantidade de endereços: 2¹⁴ = 16.384

	Portanto, uma rede /18 possui 16.384 endereços IPv4.

Em uma sub-rede convencional, normalmente dois endereços são reservados: o endereço da rede e o broadcast. Assim, o número usual de endereços utilizáveis para hosts é 16.382.

---
### 4.2. Converter CIDR para máscara

Cada octeto possui 8 bits.

Na máscara, os bits de rede ficam ligados (1) e os bits de host ficam desligados (0).

Exemplo: /18

- Passo 1: preencher os primeiros 18 bits com 1.

		11111111.11111111.11000000.00000000

- Passo 2: converter cada octeto para decimal. (vou ensinar no próximo tópico)

		11111111 = 255
		11111111 = 255
		11000000 = 128 + 64 = 192
		00000000 = 0

- Resultado:

	**/18** é **255.255.192.0**
---
### 4.3. Valores dos bits de um octeto

Cada posição binária representa uma potência de 2.

| Bit  | 1 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | <- 8 bits totais |
|------|---|---|---|---|---|---|---|---|---|
| Valor | 128 | 64 | 32 | 16 | 8 | 4 | 2 | 1 | <- 256 valores totais |


(Nesse exemplo como só o primeiro "1" está ligado o valor do octeto seria 128)

Para converter binário em decimal, somamos os valores das posições que possuem 1.
	
Exemplo:

	11000000

Cálculo:

128 + 64 = 192	

Logo, **11000000** equivale a **192**.

Tabelinha pra usar na calculadora: 128 + 64 + 32 + 16 + 8 + 4 + 2 + 1 

---
### 4.4. Descobrir o tamanho do bloco de uma máscara

Quando o octeto da máscara está entre 1 e 254, o tamanho do bloco pode ser calculado assim:

	Tamanho do bloco = 256 − valor do octeto da máscara

Exemplo: 

- Passo 1: Máscara **255.255.192.0**
- Passo 2: No terceiro octeto, temos **192**.
- Passo 3: 256 − 192 = **64**, portanto, o tamanho do bloco nesse octeto é 64.

As redes começam nos múltiplos de 64 nesse octeto: 0, 64, 128 e 192.

---
### 4.5. Converter máscara para CIDR

Para converter uma máscara para CIDR, contamos os bits ligados (1) nos quatro octetos.

Exemplo:

- Máscara: 255.255.192.0
- Primeiro octeto: 8 bits ligados.
- Segundo octeto: 8 bits ligados.
- Terceiro octeto: 2 bits ligados.
- Quarto octeto: 0 bits ligados.
- Total: 8 + 8 + 2 = 18
- Resultado: /18.

Dica: também é possível descobrir o prefixo observando a sequência de bits da máscara. Uma máscara válida apresenta todos os bits 1 antes dos bits 0.

---
### 5. Exemplo prático: separação de redes

Durante o NetPractice, foi necessário identificar a rede de um Host, vamos nomea-lo de "Host Three".

- Rede do Host Three: **159.192.109.4/30**

	| Endereço | Função |
	|---|---|
	| 159.192.109.4 | Endereço da rede |
	| 159.192.109.5 | Endereço do Router Two (R22) |
	| 159.192.109.6 | Endereço do Host Three |
	| 159.192.109.7 | Broadcast |


	A máscara **/30** equivale a **255.255.255.252**.

- Cálculo do tamanho do bloco: **256 − 252 = 4**

	Assim, as redes nesse último octeto começam em múltiplos de 4: **0, 4, 8, 12** e assim por diante.

	A rede **159.192.109.4/30** contém quatro endereços, dos quais dois são normalmente utilizáveis por interfaces de rede.

	Nesse exemplo, o Router Two utiliza **159.192.109.5**, enquanto o Host Three utiliza **159.192.109.6**. Como ambos pertencem à mesma sub-rede, conseguem se comunicar diretamente.

---
### 6. Rotas: rede de destino e próximo salto

Uma tabela de rotas informa ao roteador como encaminhar pacotes para diferentes redes.
Cada rota possui, entre outras informações, um **destino** e um **próximo salto**.

- **Rede de destino (Destination)**

	É a rede IP para a qual o pacote está tentando chegar.

	O roteador compara o endereço IP de destino do pacote com as rotas disponíveis e escolhe a rota mais específica que corresponda ao destino.

	Exemplo:

		159.192.109.128/26

	Essa rota representa a rede que vai de **159.192.109.128 até 159.192.109.191**.

- **Próximo salto (Next Hop / Gateway)**

	É o endereço IP do próximo roteador para o qual o pacote deve ser encaminhado.

	Exemplo:

		159.192.109.128/26 → 159.192.109.253

	Nesse caso:

	Destino: a rede **159.192.109.128/26**.

	Próximo salto: o **roteador** que utiliza o endereço **159.192.109.253**.

	O **destino** informa para onde o pacote precisa chegar. O **próximo salto** informa para qual roteador enviá-lo na próxima etapa do caminho.

- **Rota padrão**

	A rota padrão é representada por:

		0.0.0.0/0

	Ela corresponde a qualquer destino IPv4 e é utilizada quando não existe uma rota mais específica aplicável.

	Exemplo:

		0.0.0.0/0 → 159.192.109.254

	Isso significa que o roteador encaminhará para 159.192.109.254 os pacotes cujos destinos não correspondam a uma rota mais específica.

	O caminho de ida e o caminho de volta

	A comunicação entre dois dispositivos depende de um caminho de ida e de um caminho de retorno.

	Por exemplo:

		Host → Router One → Router Two → Host Three

	Para que a comunicação funcione, os equipamentos envolvidos precisam ter configurações e rotas compatíveis para encaminhar os pacotes em ambas as direções.

	Uma rota de ida correta não garante que a resposta conseguirá retornar ao dispositivo de origem.

---
### 7. Checklist para resolver exercícios do NetPractice

- Identificar os IPs e as máscaras de cada interface.

- Descobrir a rede à qual cada IP pertence.

- Verificar se os hosts conectados ao mesmo switch estão na mesma sub-rede quando precisam se comunicar diretamente.

- Confirmar se o gateway de cada host está em uma rede diretamente alcançável por sua interface.

- Examinar as rotas dos roteadores e identificar o próximo salto de cada destino.

- Verificar se existe uma rota de retorno para a rede de origem.

- Alterar uma configuração por vez e conferir se os objetivos que já estavam funcionando continuam funcionando.

Lembrete: o objetivo não é apenas fazer o pacote chegar ao destino. É garantir que ele chegue ao host correto e que o caminho de retorno também funcione.

---

## Resources



Inicialmente uma página que me auxiliou com máscaras: https://www-freecodecamp-org.translate.goog/news/subnet-cheat-sheet-24-subnet-mask-30-26-27-29-and-other-ip-address-cidr-network-references/?_x_tr_sl=en&_x_tr_tl=pt&_x_tr_hl=pt&_x_tr_pto=tc

Para tirar dúvidas utilizei também: https://chatgpt.com/ e https://gemini.google.com/ e também colegas da 42.
