# Introdução

O conceito de processo é a base para a implementação de sistemas mutiprogramáveis. Então, se tornou cargo do sistema operacional se responsabilizar por alocar recursos, compartilhar dados, trocar informações e sincronizar a execução entre processos.


Nota: Nos sistemas com múltiplos processadores, além de existir a concorrência entre processos pelo processador, também existe a possibilidade de execução simultânea de diferentes processos nos processadores distintos.

Como apresentado anteriormente, o processador busca a próxima instrução a ser executada na memória e a armazena no registrador PC (program counter). Do ponto de vista do hardware apenas são executadas instruções, sem distinguir qual programa encontra-se em processamento. Então, é de responsabilidade do S.O implementar a concorrência entre programas gerenciando a alternância da execução de instruções.


# Estrutura do processo

Para que a concorrência entre os programas ocorra sem problemas, é necessário que todas as informações do programa interrompido sejam guardadas para que, quando este volte a ser executado, não lhe falte nenhuma informação. Dessa forma, surge o conceito de processo, que é definido como o conjunto de informações necessárias para que o S.O implemente a concorrência.

Nota: A troca de um processo por outro no processador, dirigida pelo S.O, é denominada mudança de contexto.

Outra definição de processo é o ambiente onde um programa é executado, ou seja, além das informações da execução, também possui a quantidade de recursos do sistema que cada programa pode utilizar, como espaço de endereçamento, M.P, área em disco, etc. Por conta disso, o resultado da execução de um mesmo programa pode variar, dependendo do processo que é executado, ou seja, em função dos recursos que são disponibilizados para o programa.

Seguindo, um processo é formado por três partes, contexto de hardware, contexto de software e espaço de endereçamento.

![[Pasted image 20230618195226.png]]


## Contexto de hardware

O contexto de hardware armazena o conteúdo dos registradores gerais da CPU, além  dos registradores de uso específico, como o PC, SP, e registrador de status. Quando um processo está em execução, o seu contexto de hardware está armazenado nos registradores do processador e, ao sofre preempção, essas informações são salvas no contexto de hardware.

![[Pasted image 20230618195523.png]]

## Contexto de software

No contexto de software de um processo são especificados limites e características dos recursos que podem ser alocados pelo processo, como um número máximo de arquivos abertos simultaneamente, prioridade de execução e tamanho do buffer para as operações de I/O. A maioria das características são determinadas no momento da criação do processo.

A maior parte das informações do contexto de software do processo provém de um arquivo do sistema operacional, conhecido como arquivo de usuários. Neste arquivo são especificados os limites dos recursos que cada processo pode alocar, sendo gerenciado pelo ADM do sistema.

O contexto de software é composto por três grupos de informações sobre o processo:

* Identificação

Cada processo criado pelo S.O recebe um número de identificação única (PID). Essa identificação é utilizada para fazer referência a um determinado processo.

O processo também possui a identificação do usuário ou processo que criou (owner). Cada usuário possui uma identificação única no sistema. UID. A UID permite implementar um modelo de segurança onde somente processos com a mesma UID podem ser acessados por um determinado usuário.

* Quotas

As quotas são o limite de cada recurso do sistema que um processo pode alocar. Caso uma quota seja insuficiente, o processo poderá ser executado lentamente, interrompido ou nem mesmo ser executado. São exemplo de quotas:

- Número máximo de arquivos abertos simultâneamente
- N° max de M.P e secundária que o processo pode alocar
- N° máximo de operações de E/S pendentes
- Tamanho max do buffer para operações de E/S
- N° máximo de processos, suprocessos e threads que podem ser criados
---

* Privilégios

Os privilégios definem quais ações um processo pode fazer em relação a si mesmo, aos demais processos e ao sistema operacional.

Os privilégios que afetam o S.O são mais amplos e poderosos. A maioria dos S.Os implementa uma conta de acesso com todos estes privilégios, com o propósito de o administrador gerenciar o S.O. No Unix existe a conta 'root', por exemplo.

## Espaço de endereçamento

O espaço de endereçamento é a área de memória pertencente ao processo onde instruções e dados do programa são armazenados para a execução. Cada processo possui seu próprio espaço de endereçamento, que deve ser devidamente protegido de acesso dos demais processos.

---
![[Pasted image 20230618201054.png]]


## Bloco de Controle do Processo (PCB)

O PCB é a estrutura de dados que é utilizada pelo S.O para implementação das informações. Dessa forma, o PCB é onde o S.O mantém todas as informações sobre contexto de hardware, contexto de software e espaço de endereçamento de cada processo.

Os PCBs de todos os processo ativos residem na M.P em uma área exclusiva do sistema operacional.

![[Pasted image 20230618201529.png]]

# Estados do processo

Os processos passam por diferentes estados ao longo do processamento, em função de eventos gerados pelo sistema operacional ou pelo próprio processo. Um processo ativo pode encontrar-se em três diferentes estados:

* Execução (running)

Um processo é dito no estado de execução quando está sendo processado pela CPU. Em sistemas com apenas uma CPU, somente um processo pode estar sendo executado em dado um instante de tempo.

Em sistemas com múltiplos processadores, existe a possibilidade de mais de um processo ser executado ao mesmo tempo. Nesse sistema, também é possível um mesmo processo ser executado simultaneamente em mais de uma CPU.

* Pronto (ready)

Um processo está no estado de pronto quando aguarda apenas para ser executado. O S.O é o responsável por estabelecer a ordem de execução entre os processos.

* Espera (wait)

Um processo está no estado de espera quando aguarda por algum evento externo ou por algum recurso para prosseguir o seu processamento. Por exemplo, o termino de uma operação de entrada e saída.

## Mudanças de Estado do Processo

Existem quatro mudanças de estado que podem ocorrer a um processo:

![[Pasted image 20230619095445.png]]


Observe que, um processo no estado de espera sempre terá de passar pelo estado de pronto antes de poder ser novamente selecionado para a execução.

Além disso, um processo em estado de espera pode ou não se encontrar na memória principal. Isso pode acontecer caso não exista espaço suficiente para todos os processos.

![[Pasted image 20230619095636.png]]

# Criação e eliminação de processos

A criação de um processo ocorre a partir do momento em que o S.O adiciona um novo PCB à sua estrutura e aloca um espaço de endereçamento na memória para uso.

No caso da eliminação de um processo, todos os recursos associados ao processo são desalocados e o PCB é eliminado pelo S.O.


Isso resulta em dois novos estados possíveis para um processo, além dos três vistos antes, são eles: estado de término e estado de criação.

![[Pasted image 20230619100423.png]]

* Criação (new)

Um processo é dito no estado de criação quando o sistema operacional já criou um novo PCB, porém, ainda não pode colocá-lo na lista de processos do estado de pronto. Alguns sistemas operacionais limitam o número de processos ativos em função dos recursos disponíveis e essa limitação pode ocasionar que processos criados permaneçam por um determinado tempo no estado de criação até passarem para ativos.

* Terminado (exit)

Um processo no estado de terminado não poderá ter mais nenhum programa executado no seu contexto, porém o sistema operacional ainda mantém suas informações de controle na memória. Nesse estado, um processo não é mais considerado como ativo, porém, como o PCB ainda existe o sistema operacional pode recuperar informações como a contabilização do uso de recursos de um determinado processo.

O término de um processo pode ocorrer por razões como:

Término normal de execução;

Eliminação por um outro processo;

Eliminação forçada por ausência de recursos no sistema.

# Processos CPU-bound e I/O-bound

Um processo é definido como CPU-bound (ligado a CPU) quando passa a maior parte do tempo no estado de execução, utilizando o processador ou no estado de pronto. Esse tipo de processo realiza poucas operações de leitura e gravação.

Por outro lado, um processo é definido como I/O-bound (ligado à E/S) quando passa a maior parte do tempo no estado de espera, pois realiza um elevado número de operações de E/S.

# Processos Foreground e Background

Um processo possui necessariamente pelo menos dois canais de comunicação associados a sua estrutura (Input e Output), pelos quais são realizadas todas as estradas e saídas de dados ao longo do processamento. Esses canais podem estar associados a terminais, arquivos, impressoras, ou até mesmo outros processos.

Um processo foreground é aquele que permite a comunicação direta do usuário (permite interação) com o processo durante o processamento. No caso, tanto o canal de entrada quanto o de saída estão associados ao teclado, mouse e  monitor.

Já um processo background é aquele que não existe a comunicação com o usuário durante o seu processamento. Neste caso, os canais de Input e Output não estão associados a dispositivos que permitam interação do usuário.

# Formas de criação de um processo

As três principais formas de criação de processos são:

* Logon interativo

No logon interativo, o usuário interage com o sistema por meio de um terminal. Após efetuar um login sucedido, um processo foreground é criado, possibilitando ao usuário interagir com o sistema utilizando uma linguagem de comandos.

* Via linguagem de comandos

Um usuário pode, a partir de seu processo criado no logon, criar novos processo por intermédio de comandos da linguagem de comandos. O principal objetivo para que um usuário crie diversos processos é a possibilidade de execução de programas concorrentemente.

* Via rotinas do S.O

Um processo pode ser criado a partir de qualquer programa executável, chamando rotinas do S.O. A criação deste processo possibilita a execução de outros programas concorrentes ao programa chamador.

# Processo Independentes, Subprocessos e Threads

Processos independentes é a maneira mais simples de implementar a concorrência em sistemas mutiprogramáveis. Nesse caso, não existe vínculo do processo criado com o seu processo criador.

A criação de um processo independente exige a alocação de uma PCB própria.


Por sua vez, subprocessos são processos criados dentro de uma estrutura hierárquica. Nessa estrutura, há um processo pai, enquanto os novos processos criados a partir dele são chamados de processos filhos. Nesse tipo de implementação, caso o processo-pai deixe de existir, os processos subordinados são automaticamente eliminados (apesar de cada subprocesso possuir sua própria PCB).

Outra característica importante dos suprocessos é que eles podem compartilhar quotas com o processo pai, ou seja, o processo-pai deve ceder parte de suas quotas de recursos.

Nesse caso, a sincronização e comunicação entre os subprocessos é considerada pouco eficiente, visto que os processos não compartilham espaço de endereçamento.

Nota:  Sempre que um novo processo é criado, o S.O deve alocar recursos, consumindo tempo da CPU nesse trabalho. No momento de término dos processos, o sistema também dispensa tempo para desalocar esses recursos, ou seja, é uma operação custosa.


As threads por sua vez buscam reduzir o tempo gasto na criação, eliminação e troca de contexto entre processos nas aplicações concorrentes. Em um ambiente multithread, cada thread possui uma parte do código da aplicação associada, assim, quando uma determinada thread espera por uma operação de I/O, outra pode estar executando.


Cada thread possui o seu próprio contexto de hardware, porém, compartilha o mesmo contexto de software e espaço de endereçamento com os demais processos. O compartilhamento do espaço de endereçamento permite que a comunicação entre threads  dentro de um mesmo processo seja realizada de forma simples e rápida.

# Processo do sistema operacional


O conceito de processos também pode ser implementado na arquitetura do S.O. No caso, a arquitetura de microkernel implementa o uso  intensivo de processos individuais que disponibilizam serviços para o processos das aplicações do usuário e do próprio S.O.

Utilizando essa técnica, estamos retirando do núcleo do sistema alguns serviços, tornando-o menor e mais estável. Além disso, permite economia de memória, não ativando processos desnecessários.

# Sinais

O uso de sinais é uma forma de possibilitar a comunicação e sincronização entre processos.

![[Pasted image 20230619155509.png]]


A maior parte dos eventos associados a sinais são gerados pelo sistema operacional ou pelo hardware, como a ocorrência de exceções e interrupções. A geração de um sinal ocorre quando o S.O, a partir da ocorrência de eventos síncronos ou assíncronos, notifica o processo através de bits de sinalização localizados em seu PCB.

Um processo não responde instantaneamente a um sinal, os sinais podem ficar pendentes até que o processo seja escalonado para e execução.

O tratamento de sinal é muito semelhante ao mecanismo de tratamento de interrupções. Quando um sinal é tratado, o contexto do processo é salvo e a sua execução é desviada para um código de tratamento (signal handler), geralmente no núcleo do sistema.  [

Em certas implementações, o próprio processo pode tratar o sinal através de uma rotina definida em seu código. É possível também que um processo bloquei temporariamente ou ignore completamente alguns sinais. 

Nota: o sinal está para o processo assim como as interrupções e exceções estão para o S.O.


[[EXERCÍCIOS]]

