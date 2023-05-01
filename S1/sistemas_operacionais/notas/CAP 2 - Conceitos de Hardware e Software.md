
# Introdução / Hardware

Um sistema computacional é um conjunto de circuitos eletrônicos interligados, formado por processadores, memórias, registradores, barramentos e dispositivos de entrada e saida (E/S). Todos esses dispositivos manipulam dados na forma digital, o que proporciona uma maneira confiável de representação e transmissão de dados.

Em 1940 foi desenvolvida uma arquitetura de sistema computacional que é aplamente utilizada até hoje, a arquitetura de Von Neumann. Essa arquitetura é norteada pelo princípio de armazenamento de dados e programas na memória, esse fator implica que as instruções dos programas e os dados são manipulados da mesma forma, ou seja, pelos mesmos componentes de hardware. A arquitetura de Von Neumann é composta de cinco elementos principais:

* Unidade Central de Processamento (CPU)

Responsável por executar as instruções do programa e manipular os dados. Ele consiste em um conjunto de registradores, uma unidade de controle e uma unidade Aritmética e Lógica (ALU).

* Memória 

É o local onde o programa e os dados são armazenados. A memória é geralmente dividida em duas áreas: a memória principal, que é usada para armazenar programas e dados durante a execução do programa, e a memória secundária, que é usada para armazenar programas e dados permanentemente.

* Unidades de Entrada e Saída (E/S)

As unidades de E/S são responsáveis por gerenciar a comunicação entre o computador e o mundo externo. Ele inclui dispositivos de entrada, como teclado e mouse, e dispositivos de saída, como monitor e impressora.

* Barramento

O barramento é o caminho de comunicação entre a CPU, a memória e a unidade de E/S. Ele é responsável por transportar os dados e as instruções entre esses componentes.

* Sistema de interrupções

O sistema de interrupções é responsável por permitir que o sistema operacional e os programas interajam com a CPU de maneira eficiente. Ele permite que a CPU execute instruções de maneira intercalada, permitindo que várias tarefas sejam executadas simultaneamente.

---

A arquitetura de von Neumann é conhecida por sua simplicidade e flexibilidade, e é amplamente utilizada em uma variedade de aplicações. No entanto, uma das principais desvantagens da arquitetura de Von Neumann é que ela é limitada pela velocidade da memória. Por conta disso, a arquitetura de von Neumann não é ideal para lidar com problemas complexos, como aprendizado de máquina e inteligência artificial, que exigem grande quantidade de dados e capacidade de processamento em paralelo.

![[Pasted image 20230305204434.png]]

## Processador

O processador é o cérebro do computador, sua principal função é controlar e executar as instruções presentes na memória principal, através de operações básicas como somar, subtrair, comparar e  movimentar dados.

Um processador é composto por três unidades:

* unidade de controle

É responsável por gerenciar as atividades de todos os componentes do computador, como gravação de dados em disco ou a busca de instruções em memória.

* unidade lógica aritimética

Como o nome diz, realiza operações lógicas como soma, comparações, etc.

* registradores

Os registradores são um tipo de memória de alta velocidade, que armazenam dados temporariamente. Alguns registradores podem ser manipulados diretamente por instruções (registradores de uso geral), enquanto outros são responsáveis por armazenar informações de controle do processador e do sistema operacional (registradores de uso específico).  Entre os registradores de uso específico, tem maior  destaque:

 ** Program Counter (PC)
Contém o endereço da próxima instrução que o processador deve buscar e executar. Toda vez que o processador buscar uma nova instrução, esse registrador é atualizado com o endereço da memória da  instrução seguinte a ser executada.

 **  Stack Pointer (SP)
Contém o endereço de memória do topo da pilha, que é a estrutura de dados onde o sistema que mantém informações sobre programas que estão sendo executados e tiveram que ser interrompidos.

 **  Program Status Word
Responsável por armazenar informações sobre a execução de instruções, como a ocorrência de overflow. A maioria das instruções quando executadas, alteram o registrador de status conforme o resultado.

## Memória Principal

É o local onde são armazenados instruções e dados. A memória é composta por unidades de acesso chamadas células, sendo cada célula composta por um determinado número de bits. Atualmente a grande maioria dos computadores utiliza um byte como tamanho de célula.

O acesso ao conteúdo de uma celula é realizado através da especificação de um número chamado de endereço, uma referência única a cada célula. Quando um programa deseja ler ou escrever um dado em uma célula, deve primeiro específicar qual endereço deve ser utilizado.

A específicação do endereço é realizada através de um registrador denominado registrador de endereço de memória (MAR). Outro registrador usado em operações com a memória é o registrador de dados da memória (MBR). O MBR  é utilizado para guardar o conteúdo de uma ou mais células de memória, após uma operação de leitura, ou para guardar os dados que serão transferido para a memória em uma gravação.

![[Pasted image 20230305210028.png]]

A memória principal pode ser classificada em função da sua volatilidade, que é a capacidade de a memória preservar seu conteúdo mesmo sem uma fonte de alimentação ativa. As memórias do tipo RAM são voláteis, em contraste,  as ROM (ready only memory) e EPROM (erasable programmable ROM) são do tipo não voláteis.

## Memória Cache

A memória cache é uma memória volátil de alta velocidade, porém, com uma pequena capacidade de armazenamento. O tempo de acesso a ela é muito menor que o da memória principal. Seu tamanho é limitado em função de seu alto custo.


![[Pasted image 20230305210551.png]]

![[Pasted image 20230305210457.png]]


A memória cache armazena uma pequena parte do conteúdo da memória principal. Toda vez que o processador faz referência a um dado armazenado na memória, verifica-se se este dado não está na cache. Caso o dado esteja na cache, a situação é chamada de cache hit, caso não, é chamada de cache miss.

Os dados são alocados na cache segundo o princípio da localidade, que é a tendência do processador, ao longo da execução de um programa, referenciar instruções da memória principal localizados em endereços próximos. Essa tedência é justificada pela alta incidência de sub-rotinas e acesso a estruturas de dados como vetores e tabelas. O princípio da localidade garante então que após a transferência de um novo bloco para a cache, haverá uma alta probabilidade de cache hits em futuras referências.

Em algumas construções, há a hierarquização da construção de cache em múltiplos níveis.  O nível mais alto é chamado de L1 (Level 1), com baixa capacidade de armazenamento e altíssima velocidade, o segundo nível é chamado L2, com capacidade de armazenamento um pouco maior e um pouco menos de velocidade, e assim por diante.

## Memória secundária

É uma memória não volátil para armazenamento de dados e programas. Seu acesso é lento, porém, o custo é baixo.

---
![[Pasted image 20230305221049.png]]

## Dispositivos de entrada e saída (E/S)

Permitem a comunicação do sistema computacional e o ambiente externo. Podem ser subdivididos como dispositivos de memória secundária e dispositivos de interface usuário e máquina.

## Barramento

É o meio de compartilhamento de infomações entre as unidades funcionais de um sistema computacional. Em geral um barramento possui linhas de controle e linhas de dados.

Através das linhas de controle trafegam informações de sinalização como, por exemplo, o tipo de operação que está sendo realizada. Pelas linhas de dados, informações como instruções, operandos e endereços são transferidos entre unidades funcionais.

Os barramentos ainda podem ser de três tipos: barramentos de processador/memória, barramentos de E/S e barramentos de blackpane. Os barramentos de processador/memória são de alta velocidade e curta extensão, diferentemente do barramento de E/S que possui uma maior extensão e são mais lentos.

![[Pasted image 20230305221740.png]]

Observe a imagem acima, nesse caso, os adaptadores atuam na conversão de velocidade entre os diferentes barramentos.

Algumas arquiteturas de alto desempenho utilizam um terceiro barramento, blackplane. Nessa organização, o barramento de E/S não se conecta diferetamente ao barramento do processador/memória.

![[Pasted image 20230305221920.png]]

A principal vantagem nesse caso é reduzir o número de adaptadores existentes no barramento processador/memória e, dessa forma, otimizar seu desempenho.

# Pipelining

Da mesma forma em que uma linha de montagem, a execução de uma instrução pode ser dividida em subtarefas, como as fases de busca da instrução e dos operandos, execução e armazenamento dos resultados. O processador, através de suas várias unidades funcionais pipeline, funciona de forma a permitir que, enquanto uma instrução esteja em fase de execução, uma outra instrução possa estar na fase de busca simultaneamente.

O pipelinig pode ser empregado em sistemas de um ou mais processadores, em diversos níveis, além disso, tem sido a técnica de paralelismo mais utilizada para aumentar o desempenho dos sistemas computacionais.

![[Pasted image 20230305222434.png]]

# Arquiteturas RISC e CISC

Cada processador possui um conjunto definido de instruções de máquina, definidos por seu fabricante. As instruções de máquina fazem referências a detalhes, como registradores, modos de endereçamento e tipos de dados que caracterizam um processador e suas funcionalidades.

Quando um programa é escrito em linguagem de máquina de um determinado processador, ele não pode ser executado em uma máquina de arquitetura difrente, haja vista que o conjunto de instruções de um processador é específico de para cada arquitetura.

## Reduced Instruction Set Computer (RISC)

Essa arquitetura se caracteriza por possuir poucas instruções de máquina, em geral bastante simples, que são executadas diretamente pelo hardware. Na sua maioria, essas instruções não acessam a memória principal, trabalhando principalmente com registradores (que se apresentam em grande número). Essa arquitetura permite mais velocidade e facilita o pipelining.

## Complex Instruction Set Computers (CISC)

Essa arquitetura possui instruções complexas, que são interpretadas por microprogramas. O número de registradores é pequeno, e qualquer instrução pode referenciar a memória principal.

---
![[Pasted image 20230305223201.png]]

![[Pasted image 20230305223250.png]]


Os microprogramas definem a linguagem de máquina de um computador CISC. Além disso, o código executável de um processador CISC é interpretado por microprogramas durante a sua execução, gerando outras microinstruções que, de fato, são executadas pelo hardware.

Dessa forma, os processadores chamados microprogramáveis são aqueles que permitem a criação de novas instruções de máquina atráves de microprogramas.

# Análise de Desempenho

Uma das variáveis utilizadas na avaliação de desempenho de processadores é o intervalo de tempo entre os pulsos de sinal de um clock.  A frequência do clock é o inverso do ciclo de clock e indica o número de pulsos elétricos gerados em um segundo.  A medida de frequência é dada em Hz e seus múltiplos.

Continuando, uma das formas de avaliar o desempenho de diferentes processadores é  pela comparação de tempo de execução de um mesmo programa. Esse tempo é denominado tempo de CPU, que leva em consideração apenas o tempo para execução de instruções pelo processador, não incluindo tempo de espera em I/O.

O tempo de espera para um determinado programa é o seguinte:

![[Pasted image 20230323061255.png]]

Dessa forma, o desempenho do processador pode ser otimizado reduzindo o tempo de ciclo de clock ou o n° de ciclos utilizados na execução do programa.

A técnica conhecida como benchmark é utilizada para comparação entre processadores. Uma das técnicas de benchmark entre processadores é a SPEC (System Performance Evaluation Cooperative). Aspectos como operações de E/S e componentes do sistema operacional também vem sendo introduzidos nos benchmarks para simular condições de uso real.

![[Pasted image 20230324052652.png]]

# Software

A utilização de softwares adequados as diversas tarefas e aplicações torna o trabalho dos usuários muito mais simples e eficiente.

São chamados utilitários os serviços complementares do sistema operacional, como compiladores, linkers e depuradores. Os softwares desenvolvidos pelo usuário são denominados aplicativos ou aplicações.

## Tradutor

Apesar das inúmeras vantagens referentes a praticidade e simplicidade proporcionadas pelas linguagens de montagem de alto nível, os programas escritos nessas linguagens não estão prontos para ser executados pelo processador. Então, eles tem que ser convertidos, ou seja, ocorre uma etapa de conversão das representações simbólicas em linguagem de máquina. Essa conversão é realizada por um utilitário denominado tradutor.

O tradutor, ao fazer seu trabalho gera um output denominado módulo-objeto, que se apresenta em código de máquina, entretanto, na maioria das vezes esse código ainda não pode ser executado. Isso ocorre em função de um programa poder chamar [[sub-rotinas]] externas e, nesse caso, o tradutor não tem como associar o programa principal às sub-rotinas. Essa função é feita por um utilitário chamado linker.

Dependendo do tipo de programa-fonte, existem dois tipos distintos de tradutores: montador e compilador.

![[Pasted image 20230324054201.png]]

### Linker

É o resposável por gerar, a partir de um ou mais módulos-objeto um único programa executável. Ou seja, ele resolve todas as referências entre diferentes módulos referenciados no código fonte  e reseva espaço na memória para a execução do programa.

![[Pasted image 20230325160230.png]]

Para resolver essas referências, o linker também pode pesquisar em bibliotecas do sistema ou do próprio usuário.

Outra função importante do linker é a relocação, que determina a região na memória na qual o programa será carregado para a execução.  Antes se utilizavam posições prefixadas absolutas para posições na memória, porém,com o advento dos sistemas multiprogramáveis esse tipo de relocação de difícil implementação já que é pouco provável que um determinado endereço prefixado esteja disponível. A solução para esse problema é permitir que o programa seja carregado em regiões diferentes toda vez que for trazido para a memória (código relocável). Esse tipo de relocação éfeita por outro utilitário, o loader.

Em alguns sistemas, o compilador pode realizar mais do que suas funções básicas, como resolver referências de endereço e, dessa forma, o programa não precisaria passar pela linkedição. Nesse caso, um outro utilitário linking-loader fica responsável pela carga do programa na memória e por sua execução. Nesse tipo de sistema, o programa passa por um processo de linking dinâmico, onde as referências globais são resolvidas no momento da execução. A grande desvantagem dessa implementação é a sobrecarga de processamento que existe em cada execução do programa, problema que não ocorre em sistemas que possuem linkers independentes.

### Montador

O montador (assembler) é responsável por traduzir um programa fonte em um módulo-objeto (geralmente ainda não executável, pois pode precisar da atuação do linker para buscar suas dependências). A linguagem de montagem é particular de cada processador, assim como a linguagem de máquina, isso faz com que os programas assembly não possam ser portado entre máquinas de arquitetura diferente.

O montador traduz o código em linguagem de montagem diretamente para código de máquina, sem realizar transformações significativas no código. Além disso, o montador geralmente é usado para produzir programas de sistema e drivers de dispositivos, enquanto o compilador é usado para produzir programas de aplicativos.

### Compilador

Diferentemente do montador, o compilador trabalha com uma linguagem de alto nível, que não tem nenhuma relação direta com a máquina e, por conta disso, não têm que se envolver em detalhes sobre a arquitetura do processador. Assim programas-fonte podem ser portados entre computadores de diversos fabricantes.

De maneira geral, podemos visualizar o compilador como uma interface entre o S.O e o usuário, de maneira que é possível acessar diversos serviços do sistema sem a necessidade de uma linguagem de controle.

Além disso, diferentemente do montador o compilador, ao fazer a tradução para linguagem de máquina, realiza análises mais profundas no código, podendo implementar otimizações na hora da execução.

### Loader

O loader é responsável por carregar na memória principal um programa a ser executado. O processo de carga varia com o código gerado a partir do linker, a partir disso, o loader pode ser classificado como absoluto ou relocável.

* Loader absoluto

Se o código executável for do tipo absoluto o loader só necessita conhecer o endereço de memória inicial e o tamanho do módulo para realizar o carregamento. Então, o loader transfere o programa da memória secundária para a memória principal e inicia a execução.

* Loader relocável

No caso do código relocável, o programa pode ser carregado em qualquer posição de memória, e o loader é responsável pela relocação no momento do carregamento.

### Interpretador

O interpretador é um tipo de tradutor que não gera um módulo-objeto.  Durante a execução de um programa, o interpretador traduz cada instrução e a executa imediatamente.

A desvantagem de se utilizar um interpretador é o tempo gasto na tradução das instruções de um programa toda vez que ele for executado, uma vez que não existe a criação de um executável. A sua vantagem é a possibilidade de implementação de tipos de dados dinâmicos, ou seja, que podem mudar durante a execução de um programa e aumentando sua flexibilidade.

### Depurador (debugger)

A depuração é um dos estágios de desenvolvimento, e necessita da utilização de ferramentas adequadas para acelerar o processo de correção de programas. Então, esse utilitário permite ao usuário acompanhar toda a execução de um programa a fim de detectar erros na lógica oferecendo recursos como:

* Acompanhamento instrução por instrução
* Possibilidade de  alterar e visualizar o conteúdo de variáveis
* Implementar pontos de parada no programa para melhorar a análise
* Especificar uma variável observada onde, toda vez que seu valor for modificado, o programa envia uma mensagem (watchpoint)
[[EXERCÍCIOS]]