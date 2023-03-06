
# Introdução / Hardware

Um sistema computacional é um conjunto de circuitos eletrônicos interligados, formado por processadores,memórias, registradores, barremantos e dispositivos de entrada e saida (E/S). Todos esses dispositivos manipulam dados na forma digital, o que proporciona uma maneira confiável de representação e transmissão de dados.

Em 1940 foi desenvolvida uma arquitetura de sistema computacional que é aplamente utilizada até hoje, a arquitetura de Von Neumann. Essa arquitetura é norteado pelo princípio do armazenamento de dados e programas na memória, esse fator implica que as instruções dos programas e os dados sejam manipulados da mesma forma, ou seja, pelos mesmos componentes de hardware. A arquitetura de Von Neumann é composta de cinco elementos principais:

* Unidade Central de Processamento (CPU)

Responsável por executar as instruções do programa e manipular os dados. Ele consiste em um conjunto de registradores, uma unidade de controle e uma unidade aritmética e lógica (ALU).

* Memória 

É o local onde o programa e os dados são armazenados. A memória é geralmente dividida em duas áreas principais: a memória principal, que é usada para armazenar programas e dados durante a execução do programa, e a memória secundária, que é usada para armazenar programas e dados permanentemente.

* Unidades de Entrada e Saída (E/S)

As unidades de E/S são responsáveis por gerenciar a comunicação entre o computador e o mundo externo. Ele inclui dispositivos de entrada, como teclado e mouse, e dispositivos de saída, como monitor e impressora.

* Barramento

O barramento é o caminho de comunicação entre a CPU, a memória e a unidade de E/S. Ele é responsável por transportar os dados e as instruções entre esses componentes.

* Sistema de interrupções

O sistema de interrupções é responsável por permitir que o sistema operacional e os programas interajam com a CPU de maneira eficiente. Ele permite que a CPU execute instruções de maneira intercalada, permitindo que várias tarefas sejam executadas simultaneamente.

A arquitetura de von Neumann é conhecida por sua simplicidade e flexibilidade, e é amplamente utilizada em uma variedade de aplicações. No entanto, uma das principais desvantagens da arquitetura de Von Neumann é que ela é limitada pela velocidade da memória. Por conta disso, a arquitetura de von Neumann não é ideal para lidar com problemas complexos, como aprendizado de máquina e inteligência artificial, que exigem grande quantidade de dados e capacidade de processamento paralelo.

![[Pasted image 20230305204434.png]]

## Processador

O processador é o cérebro do computador, sua principal função é controlar e executar as instruções presentes na memória principal, através de operações básicas como somar, subtrair, comparar e  movimentar dados.

Um processador é composto por três unidades

* registradores

Os registradores são um tipo de memória de alta velocidade, que armazenam dados temporariamente.

* unidade de controle

É reposável por gerenciar as atividades de todos os componentes do computador, como gravação de dados em disco ou a bvusca de instruções em memória.


* unidade lógica aritimética

Como o nome diz, realiza operações lógicas como soma, comparações,. etc.

Alguns registradores podem ser manipulados diretamente por instruções (registradores de uso geral), enquanto outros são responsáveis por armazenar informações de controle do processador e do sistema operacional (registradores de uso específico). Entre os registradores de uso específico, tem maior  destaque:

* Program Counter (PC)
Contém o endereço da próxima instrução que o processador deve buscar e executar. Toda vez que o processador buscar uma nova instrução, esse registrador é atualizado com o endereço da memória da  instrução seguinte a ser executada.

* Stack Pointer (SP)
Contém o endereço de memória do topo da pilha, que é a estrutura de dados onde o sistema que mantém informações sobre progrmas que estão sendo executados e tiveram que ser interrompidos.

* Program Satus Word
Responsável por armazenar informações sobre a execução de instruções, como a ocorrência de overflow. A maioria das instruções quando executadas, alteram o registrador de status conforme o resultado.

## Memória Principal

É o local onde são armazenados instruções e dados. A memória é composta por unidades de acesso chamadas células, sendo cada célula composta por um determinado número de bits. Atualmente a grande maioria dos computadores utiliza um byte como tamanho de célula.

O acesso ao conteúdo de uma celula é realizado através da especificação de um número chamado de endereço, uma referência única a cada célula. Quando um programa deseja ler ou escrever um dado em uma célula, deve primeiro específicar qual endereço deve ser utilizado.

A específicação do endereço é realizada através de um registreador denominado registrador de endereço de memória (MAR). Outro registrador usado em operações com a memória é o registrador de dados da memória (MBR). Esse registrador é utilizado para guardar o conteúdo de uma ou mais células de memória, após uma operação de leitura, ou para guardar o dado que será transferido para a memória em uma gravação.

![[Pasted image 20230305210028.png]]

A memória principal pode ser classificada em função da sua volatilidade, que é a capacidade de a memória preservar seu conteúdo mesmo sem uma fonte de alimentação ativa. As mesmórias do tipo RAM são voláteis, quantos as ROM (ready only memory) e EPROM (erasable programmable ROM) são do tipo não voláteis.

### Memória Cache

A memória cache é uma memória volátil de alta velocidade, porém, com uma pequena capacidade de armazenamento. O tempo de acesso a ela é uito menor que o da memória principal. Seu tamanho é limitado em função de seu alto custo.


![[Pasted image 20230305210551.png]]

![[Pasted image 20230305210457.png]]


A memória cache armazena uma pequena parte do conteúdo da memória principal. Toda vez que o processador faz referência a um dado armazenado na memória, verifica-se se este dado não está na cache. Caso o dado esteja na cache, a situação é chamada de cache hit, caso não, é chamada de cache miss.

Os dados são alocados na cache segundo o princípio da localidade, que é a tendência do processador, ao longo da execução de um programa, referenciar instruções da memória principal localizados em endereços próximos. Essa tedência é justificada pela alta incidência de sub-rotinas e acesso a estruturas de dados como vetores e tabelas. O princípio da localidade garante então que após a transferência de um novo bloco para a cache, haverá uma alta probabilidade de cache hits em futuras referências.

Em algumas construções, há a hierarquização da construção de cache em múltiplos níveis.  O nível mais alto é chamado de L1 
(Level 1), com baixa capacidade de armazenamento e altíssima velocidade, e assim por diante.

[[EXERCÍCIOS]]