Em sistemas multiprogramáveis, diferentemente dos monoprogramáveis, a sub-utilização do processador e da memória é reduzida. Isso também acontece para dispositivos de entrada e saída como impressora e disco.

## Interrupções e Excessões

Interrupções e excessões são eventos inesperados, que ocasionam  um desvio no fluxo da execução de determinado programa. Elas podem ser utilizadas na sinalização por algum dispositivo de hardware ou pelo próprio programa.

Dessa forma a interrupção é o mecanismo que tornou possível a implementação de concorrência nos computadores.

### Interrupção

Uma interrupção é sempre gerada por um evento externo ao programa e independe da instrução que está sendo executada.

Por exemplo:

![[Pasted image 20230504051858.png]]

Continuando, ao final de cada instrução, a unidade de controle verifica se ocorreu algum tipo de interrupção, se ocorrer, o programa em execução é interrompido desviando para a rotina de tratamento daquele evento (rotina de tratamento de interrupção).

O mecanismo de interrupção funciona utilizando o hardware e o software. Seu cíclo é o seguinte:

![[Pasted image 20230504052156.png]]


Para cada tipo de interrupção existe uma rotina de tratamento associada, dessa forma, existem dois métodos para o tratamento de interrupções:

 * Vetor de interrupções

Nesse caso, existe uma estrutura de dados (vetor) que contém o endereço inicial de todas as rotinas de tratamento existentes associadas a cada evento.

* Tratamento específico

No segundo método, utiliza-se um registrador de status que armazena o tipo de evento ocorrrido. Com essa informação, existe somente uma única rotina de tratamento a ser executada que, ao iniciar, testa o registrador para identificar qual é o tipo de interrupção. (É como se fosse um bloco único de código, repleto de if-elses).

As interrupções são resultados de eventos assíncronos, ou seja, não relacionados a instrução que está sendo executada. Por conta disso, podem ocorrer multiplas interrupções simultâneamente. Uma maneira de evitar essa situação é a rotina de tratamento ignorar as demais interrupções após a primeira. Interrupções com essa característica são chamadas de interrupções mascaráveis.

Em alguns tipos de processador, as interrupções seguintes não podem ser ignoradas, dessa forma, existe a necessidade de um dispositivo chamado controlador de pedidos de interrupção que avalia as interrupções geradas e suas prioridades de atendimento.

### Exceções

Uma exceção, diferentemente das interrupções, é resultado direto da execução de instrução do próprio programa como, por exemplo, uma divisão por zero ou ocorrência de overflow. A exceção é gerada então por um evento síncrono. Fazendo uma associação, se um programa que causa esse tipo de eventos for executado com a mesma entrada de dados, a excessão ocorrerá sempre na mesma instrução

Dessa forma, quando uma exceção acontece, a exceução é interrompida e o controle é desviado para uma rotina de tratamento de exceções. Normalmente, o mecanismo de tratamento é definido pelo próprio programador, visando evitar que o processo seja encerado abruptamente.

## Operações de I/O

Nos primórdios, a comunicação entre o processador e periféricos era controlada por um grupo de instruções (instruções de I/O), executadas pelo próprio processador. Essas instruções continham detalhes específicos de cada periférico. Ou seja, havia uma dependência entre I/O e processador.

Visando solucionar o problema, surgiu o *controloador de interface* que permitiu independência entre processador e dispositivos de I/O. Isso simplificou as instruções de I/O por não ser mais necessário especificar detalhes de operações específicos de cada periférico.

![[Pasted image 20230504054146.png]]

Com a utilização do controlador, passaram a existir duas maneiras básicas de gerenciamento de operaçoes I/0 pelo processador:

* I/O controlada por programa

Nesse caso, o processador sincronizava-se com o periférico para começar a transferência de dados e, após o início da operação, o sistema ficava permanentemente testando o estado do periférico para saber quando a operação chegaria ao seu final. Esse tipo de controle mantém o processador ocupado enquanto espera o fim da execução do I/O (busy wait), ou seja, havia um enorme disperdício de recursos.

* I/O controlada por programa com Polling

Evolução do modelo anteiror, permitia que, após o início da operação, o processador permanecesse livre. Assim, em determinados intervalos de tempo o S.O testaria cada dispositivo de I/O para verificar se houve término da operação (polling). Isso possibilitou o surgimento dos primeiros sistemas multiprogramáveis, o problema é que, nesse caso, a depender do número de periféricos o processador poderia ser interrompido frequentemente já que é difícil determinar o momento exato de término desse tipo de operação.


* I/O controlada por interrupção

Com a implementação das interrupções, as operações de I/O puderam ser realizadas de uma forma mais eficiente, pois passou a ser responsabilidade do controlador avisar o termino da operação. 

Nesse cenário, após a execução de um comando de leitura ou gravação, o processador permanece livre e o controlador, por sua vez, (exemplo, sinal de leitura) fica encarregado de ler os blocos do disco e armazená-los na memória ou registradores próprios. Após isso, a interrupção é sinalizada ao processador. Quando o processador atende à interrupção, a rotina responsável pelo tratamento transfere os dados dos registradores e do controlador para a memória principal. Após terminada a transferência, o processador pode voltar a executar o programa interrompido e o controlador fica disponível para uma nova operação.

Observe que, as operações de I/O controladas por interrupção são muito mais efecientes uqe as controlados por gramas, entretanto, a transferência de grandes volumes de dados pode exigir muitas intervenções do processador, reduzindo sua eficiência. A solução para esse problema foi a implementação de uma ténica chamda DMA (Direct Acess Memory).

### DMA

Essa técnica permite que um bloco de dados seja transferido entre a memória principal e os dispotivos I/O com a intervação do processador somente no início e no final da transferência. Nesse caso, quando é acionada uma operação de leitura ou gravação, o processador informa ao controlador a sua localização, o dispositivos de I/O utilizado, a posição inicial de memória que será utilizada na operação e o tamanho do bloco.

Com as informações recebidas, o contralador inicia a transferência entre o periférico e a memória principal, o processador somente é interrompido ao final da operação.  A área de memória transitória utilizada pelo controlador no DMA é chamada de *buffer de entrada e saída*.

No momento em que essa técnica é associnada, o controlador assume momentaneamente o controle do barramento. Como a utilização do barramento é exlusiva para um único dispositivo, o processador deve suspender temporariariamente o acesso ao bus. Esse procedimento não gera uma interrupção, e o processador por realizar operações desde que sem a utilização do barramento, como um acesso a memória cache.

A extensão do conceito de DMA possibilitou o surgimento do *canal  de entrada e saída*
. O canal é um tipo de processador com capacidade de executar programas I/O, permitindo controle total sobre as operações de I/O. As instruções de I/O são armazenadas na memória principal pelo processador, entretanto, quem de fato executa é o canal.

Dessa forma, o processador realizada uma operação de I/O instruindo o canal para executar um programa executado na memória (programa de canal). Esse programa especifica os dispositivos para transferência, buffers e ações a serem tomadas em caso de erros. O canal de I/O realiza a transferência e, em seguida, gera uma interrupção, avisando o termino da operação. 

Um canal de I/O pode controlar múltiplos dispositivos através de vários controladores. Cada dispositivo (ou conjunto deles) é manipulador por um único controlador. O canal atua como elo entre processador principal e controlador.

A evolução do canal permitiu que este obtivesse sua própria memória, eliminando a necessidade de programas de I/O serem carregados na memória principal.

Nota: é possível encontrar também a nomenclatura de processador de entrada e saída.

![[Pasted image 20230506184945.png]]


### Buffering

A ténica de buffering consiste na utilização de uma área de memória principal (buffer) para transferência entre dados do I/O e memória. Nesse caso, a aplicação desse conceito permite que em uma operação (leitura, exemplo) o dado seja transferido primeiramente para o buffer, liberando imediatamente o dispositivo de I/O para uma nova leitura. Nesse caso, o processador manipula os dados localizados no buffer e, além disso, esse conceito também é aplicável  nas operações de gravação.

Essa técnica busca manter o processador e o dispositivo de I/O ocupados na maior parte do tempo, buscando minimizar o problema da disparidade de velocidade entre processador e I/O.

A unidade de transferência utilizada no buffering é o registro. O tamanho do registro pode ser especificado em função da natureza do dispositivo ou da aplicação.

O buffer permite que existam dados lidos, mais ainda não processados (operação de leitura), ou processados, mas ainda não lidos (operação de gravação). Dessa forma, o dispositivo de I/O poderá  ler diversos registros antes que o processador manipule os dados, ou o procesador poderá manipular diversos registros antes de o dispositivo de I/O terminar uma gravação.

## Spooling

O spooling (simultaneos peripheral operational on-line) é uma técnica oriunda de década de cinquenta para minimizar a espera pelos I/O. No caso, a solução era submeter os programas ao processamento e, ao invés de um job gravar suas saídas diretamente na impressora, ele gravaria em uma fita magnética (por exemplo) que, seria impressa integralmente ao final de todas as execuções.

No caso, a utilização de fitas magnéticas forçava com que o processamento de jobs fosse estritamente sequêncial. Com o surgimento de dispositivos de acesso direto, como discos, foi possível tornar o spooling mais eficiente e, principalmente, possibilitar o processamento não sequencial de jobs.

A técnica de spooling é semelhante ao buffering, utiliza uma área de disco como se fosse um grande buffer. Nesse caso, dados podem ser lidos ou gravados em disco enquanto programas são executados concorrentemtemente.

Essa tecnica ainda é utilizada, por exemplo, no momento que um comando de impressão é utilizado, as informações que serão impressas são gravadas antes um arquivo em disco (arquivo de spool), liberando imediatamente o programa para outras atividades. Posteriormente o S.O se encarrega de direcionar o arquivo de spool para impressora. 

O uso do spooling desvincula o programa do dispositivo de impressão, impedindo que um programa reserve a impressora para uso exclusivo. O S.O é o responsável por gerenciar a sequência de impressões solicitadas pelos progrmas, seguindo critérios que garantam a segurança e o uso eficiente desses dispositivos.

## Reentrância

É comum, em sistemas multiprogramáveis, vários usuários utilizarem o mesmo aplicativo simultâneamente. Se cada usuário que trouxesse o  código executável para a memória, haveria diversas cópias de um mesmo programa na memória principal, o que ocasionaria disperdício.

Reentrância é a capacidade de um código executável (código reentrante) ser compartilhado por diversos uisuários, exigindo apenas uma cópia do programa na memória. A reentrância permite que cada usuário possa estar em um ponto diferente do código reentrante, de maneira exclusiva.

![[Pasted image 20230506212723.png]]

	

[[EXERCÍCIOS]]