
# Introdução

Em sistemas mutiprogramáveis, vários processos podem estar em estado de pronto, dessa forma, é necessário estabelecer critérios para seleção do processo que será processado. Esses critérios são chamados de política de escalonamento, a base da gerência do processador.

O objetivo das políticas de escalonamento, no geral, é manter o processador ocupado o máximo de tempo possível. Entretanto, cada S.O possui sua política de escalonamento adequada ao seu propósito e às suas caracterísitcas.

Sistemas de tempo compartilhado, por exemplo, possuem requisitos de escalonamento distintos de sistemas de tempo real.

# Funções básicas

Nesse tópico, existem rotinas principais do sistemas operacionais, cruciais para a implementação e funcionamento das políticas de escalonamento, duas rotinas principais são:

 * Scheduler

O Scheduler é a rotina que de fato implementa a rotina de escalonamento, ou seja, ele é o responsável por selecionar qual processo será executado

 * Dispatcher

Outra rotina importante na gerência do processador é o dispatcher, responsável pela troca de contexto dos processos após o scheduler determinar quem será processado.
 O período de tempo gasto na substituição de um processo em execução para outro é denominado latência do dispatcher.

--- 

Nota: em ambientes que apenas processos são implementados, o escalonador funciona com base nos processos. Por sua vez, em sistemas que implementam threads, o escalonamento é realizado levando em consideração as threads no estado de pronto, independente do processo.

Nesse assunto, o termo processo é utilizado de forma genérica, como sendo o elemento selecionado pelo escalonador.

# Critérios de Escalonamento

Sistemas operacionais diferentes terão objetivos diferentes, por conta disso, os critérios levados em consideração na hora de escalonar um processo terão diferentes pesos em cada sistema. Os principais critérios levados em consideração são:

 * Utilização do processador

Na maioria dos sistemas, é desejável que o processador permaneça ocupado a maior parte do tempo, uma vez que é um recurso caro. Em geral, uma utilização na faixa de 30% a 90% seria adequado.

 * Throughput

O throughput representa o número de processos executados em um determinado intervalo de tempo. A maximização do throughput é desejada na maioria dos sistemas.

 * Tempo de processador

O tempo de processador é o tempo que um processo leva no estado de execução durante seu processamento. As políticas de escalonamento não influenciam no tempo de processador, sendo este função apenas do código da aplicação e da entrada de dados.

 * Tempo de espera

O tempo de espera é o tempo total que um processo permanece na fila de pronto durante o processamento, aguardando para ser executado. A redução do tempo de espera dos processos é desejada pela maioria das políticas de escalonamento.

 * Tempo de turnaround

O tempo de turnaround é o tempo total que um processo leva desde a sua criação até seu término, levando em consideração todos os fatores envolvidos nesses eventos. As políticas de escalonamento buscam minimizar o tempo de turnaround.

 * Tempo de resposta

O tempo de resposta é o tempo decorrido entre uma requisição ao sistema e sua resposta. Em sistemas iterativos, podemos entender o tempo de resposta como o tempo decorrido entre a última tecla digitada pelo usuário e o início da exibição do resultado no monitor.


# Escalonamentos Não-Preemptivos e Preemptivos

As políticas de escalonamento podem ser classificadas em preemptivas e não preemptivas. No caso, a preempção é a capacidade do S.O interromper um processo em execução e substituí-lo por outro.

## Escalonamento First-In-First-Out (FIFO)


Esse é um algoritmo bastante simples, sendo necessário apenas a implementação de uma fila, no caso, o como o nome sugere os processos são escalonados de acordo com a ordem de chegada a fila de pronto.

O escalonamento FIFO é do tipo não-preemptivo e foi inicialmente implementado em sistemas monoprogramáveis com processamento batch, sendo ineficiente se aplicado em sistemas de time-sharing.

Esse algoritmo é simples, seus principais problemas são, primeiramente a impossibilidade de se prever quando um processo terá sua execução iniciada, já que isso varia em função do tempo de execução dos demais processos e, além disso, é que os processos CPU-bound são favorecidos no uso do processador.

## Escalonamento Shortest-Job-First (SJF)

Esse algoritmo seleciona o processo que tiver o menor tempo de processador ainda por executar, dessa forma, o processo em estado de pronto que necessitar do menor tempo de CPU para terminar seu processamento é selecionado.

Como não é possível prever previamente quanto tempo um determinado processo irá necessitar de processador, uma estimativa era realizada com base em análises estatísticas de execuções passadas. Caso o tempo estimado fosse muito inferior ao tempo real, o sistema operacional poderia interromper a execução do processo.

O principal problema nesta implementação é a impossibilidade de estimar o tempo de processador para processos interativos, devido a entrada de dados ser uma ação imprevisível.

Na sua concepção inicial, o escalonamento SJF é  não-preemptivo. Sua vantagem sobre o FIFO está na redução do tempo médio de Turnaround dos processos, advinda na redução do tempo médio de espera dos processos, porém, no SJF é possível haver starvation para processo com tempo de processador muito longo ou do tipo CPU-bound.

Uma implementação do escalonamento SJF com preempção é conhecida como escalonamento Shortest Reamaining Time.

## Escalonamento Cooperativo

O escalonamento cooperativo é uma implementação que busca aumentar o grau de multiprogramação em políticas de escalonamento não preemptivos. Neste caso, um processo em execução pode voluntariamente liberar o processador retornando à fila de pronto, possibilitando que um novo processo seja escalonado.

Neste mecanismo, o processo em execução verifica periodicamente uma fila de mensagens para determinar se existem outros processos na fila de pronto.

Como a interrupção do processo em execução é de responsabilidade exclusiva da aplicação, algumas situações indesejadas podem ocorrer. No caso, se um processo não verificar a fila de mensagens, os demais não terão chance de executar, por exemplo.

## Escalonamento Circular

O escalonamento circular é do tipo preemptivo, projetado especialmente para sistemas de tempo compartilhado. Esse escalonamento implementa uma fila do tipo FIFO, porém, quando um processo passa para o estado de execução, existe um tempo limite para o uso contínuo do processador denominado time-slice.

Caso a fatia de tempo expire, o S.O interrompe o processo em execução, salva seu contexto e direciona-o para o final da fila de pronto, mecanismo conhecido como preempção por tempo. O escalonamento segue alocando a CPU no primeiro processo da fila de pronto.

O valor da fatia de tempo depende da arquitetura de cada sistema operacional. Este valor afeta diretamente o desempenho dessa política de escalonamento. Caso a fatia de tempo tenha um valor muito alto, o sistema tenderá a ter o mesmo comportamento de um FIFO. 

Por sua vez, caso o valor do time slice seja pequeno, a tendência é que haja um grande número de preempções, o que ocasionará excessivas mudanças de contexto e acumulo de tempo de latência do dispatcher, prejudicando o turnaround dos processos.

A principal vantagem do escalonamento circular é não permitir que um processo monopolize a CPU, sendo o tempo máximo alocado contiguamente igual ao time-slice.

Um problema presente nessa política é que os processos CPU-bound são beneficiados no uso do processador em relação aos processos I/O-bound. Devido às suas características, os processos CPU-bound tendem a utilizar por completo a fatia de tempo, enquanto os processos I/O-bound têm mais chances de passar para o estado de espera antes de sofrerem a preempção por tempo. Estas características ocasionam um balanceamento desigual no uso do processador.

### Escalonamento circular virtual

Um refinamento do escalonamento circular tradicional, busca reduzir o problema de favorecimento de processos CPU-bound. 

Nessa política, os processos saem do estado de espera vão para uma fila de pronto auxiliar, e os processos da fila de pronto auxiliar possuem uma preferência no escalonamento em relação a fila de pronto, o escalonador somente seleciona processos na fila de pronto quando a fila auxiliar estiver vazia. 

Além disso, quando um processo é escalonado a partir da fila auxiliar, sua fatia de tempo é calculada subtraindo o tempo de processador que o processo utilizou da última vez em que foi escalonado.

## Escalonamento por Prioridades

O escalonamento prioridades é um escalonamento do tipo preemptivo realizado com base em um valor associado a cada processo denominado prioridade de execução. O processo com maior prioridade no estado de pronto é sempre escolhido para execução, e processos com valores iguais são escalonados seguindo um critério FIFO. Neste escalonamento, o conceito de fatia de tempo não existe; consequentemente, um processo em execução não pode sofrer preempção por tempo.

No escalonamento por prioridades, a perda do uso do processador somente ocorrerá no caso de uma mudança voluntária para o estado de espera ou quando um processo de prioridade maior passa para o estado de pronto. Esse mecanismo é conhecido como preempção por prioridade.

A preempção por prioridade é implementada através de uma interrupção de clock, gerada em determinados intervalos de tempo, para que o scheduler reavalie as prioridades dos processos em estado de pronto.

O escalonamento por prioridades também pode ser implementado de uma maneira não-preemptiva. Neste caso, os processo que passarem para o estado de pronto com prioridade maior que a do processo de execução não ocasionam uma preempção por prioridade, apenas vão para o início da fila.

Nota: cada sistema operacional implementa sua faixa de valores para as prioridades de execução.

A prioridade de execução é uma característica do contexto de software de um processo e pode ser classificada como estática ou dinâmica.

Um dos principais problemas no escalonamento por prioridades é o starvation. Processos de baixa prioridade podem não ser escalonados, permanecendo indefinidamente na fila de pronto. Uma solução para este problema, possível em sistemas que implementam a prioridade dinâmica é a técnica de aging. Esse mecanismo incrementa gradualmente a prioridade de processos que permanecem por muito tempo na fila de pronto.

Isto é bastante útil, tanto em sistemas de tempo real e nas aplicações de controle de processos, como em sistemas de tempo compartilhado, onde, às vezes, é necessário priorizar o escalonamento de determinados processos.

## Escalonamento Circular com Prioridades

O escalonamento circular com prioridades implementa tanto o conceito de time-slice quanto o conceito de prioridade de execução associada a cada processo. Então, nesse tipo de escalonamento, um processo pode sofrer tanto uma preempção por tempo quanto por prioridade.

A principal vantagem desse tipo de escalonamento é permitir melhor balanceamento no uso da CPU, com a possibilidade de diferenciar o grau de importância entre os processos.

Esse tipo de escalonamento é amplamente utilizado em sistemas de time-sharing.

## Escalonamento por Múltiplas Filas

Esses escalonamento parte do princípio que  os processos possuem características de processamento distintas e, por conta disso, é difícil que um único mecanismo de escalonamento seja adequado a todos.

Continuando, no escalonamento por múltiplas filas existem diversas filas de processos no estado de pronto, cada qual com uma prioridade específica. Os processos são associados às filas em função de características próprias, como importância para a aplica, tipo de processamento ou área de memória necessária.

A principal vantagem de múltiplas filas é a possibilidade da convivência de mecanismos de escalonamento distintos em um mesmo S.O. Cada fila possuem um mecanismo próprio, permitindo, por exemplo, que alguns processos sejam escalonados pelo mecanismo FIFO enquanto outro pelo circular.

Neste mecanismo, o processo não possui prioridade própria, sendo esta característica associada à fila em que o processo se encontra. O processo em execução sofre preempção caso um outro processo entre em uma fila de prioridade maior. Além disso, o S.O somente pode escalonar processos de uma determinada fila caso todas as filas de prioridade mais altas estejam vazias.

![[Pasted image 20230620124704.png]]


Uma desvantagem deste escalonamento é que, no caso do processo alterar seu comportamento no decorrer do tempo, o mesmo não poderá ser redirecionado para uma outra fila mais adequada. A associação de um processo à a fila é determinada na criação de um processo, permanecendo até o final de seu processamento.

## Escalonamento por múltiplas filas com Realimentação

O escalonamento por múltiplas  filas com realimentação é semelhante ao escalonamento por múltiplas filas padrão. Sua grande vantagem é permitir ao S.O identificar dinamicamente o comportamento de cada processo e direciona-lo para a fila com a prioridade de execução mais adequada.

Nesse caso, existe a necessidade de implantação de um mecanismo no S.O chamado de mecanismo adaptativo.

Um mecanismo FIFO adaptado com time-slice é implementado para escalonamento em todas as filas, com exceção da fila de menor prioridade que utiliza um escalonamento circular. O escalonamento de um processo em fila ocorre apenas quando todas as outras filas de prioridade mais altas estiverem vazias.

A fatia de tempo em cada fila varia em função de suas prioridade, ou seja, quanto maior a prioridade da fila, menor a sua fatia de tempo. Sendo assim, a fatia concedida aos processos varia em função da fila de fila de pronto em que ele se encontra. Um processo, quando criado, entra no final da fila de maior prioridade, porém, durante a sua execução, a cada preempção por tempo, o processo é redirecionado para uma fila de menor prioridade.

No caso de processos I/O-bound, um tempo de resposta adequado é obtido, já que esses processos têm prioridades mais altas por permanecerem a maior parte do tempo nas filas de maior prioridade, pois dificilmente sofrerão preempção por tempo. 

Por outro lado, em processos CPU-bound a tendência é de que, ao entrar na fila de prioridade mais alta, ele execute toda sua fatia de tempo e seja direcionado a uma fila de menor prioridade. Dessa forma, quanto mais tempo um processo se utiliza do processador, mais ele cai de prioridade.

Um dos problemas encontrados nessa política é que a mudança de comportamento de um processo CPU-bound para I/O-bound pode comprometer seu tempo de resposta. Outro aspecto a ser considerado é a sua complexidade de implementação, ocasionando um grande overhead no sistema.

---

Embora os sistemas com prioridade dinâmica sejam mais complexos de serem implementados, o tempo de resposta oferecido compensa. Atualmente, a maioria dos S.O de time-sharing utiliza o escalonamento circular com prioridades dinâmicas.

Em outras ocasiões, algumas aplicações exigem respostas imediatas para a execução de determinadas tarefas. Nesse caso, a aplicação deve ser executada em sistemas operacionais de tempo real, onde é garantida a execução dos processos dentro de limites de tempo rígidos, sem a possibilidade de comprometimento da aplicação.

Em função disso, o escalonamento por prioridades é o mais adequado nesses casos. No escalonamento para sistemas em real-time não se deve existir o conceito de time-slice e a prioridade de cada processo deve ser estática.

[[EXERCÍCIOS]]








