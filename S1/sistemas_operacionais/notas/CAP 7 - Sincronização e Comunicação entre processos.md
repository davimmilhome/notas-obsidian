Uma aplicação concorrente se caracteriza por possibilitar uma execução cooperativa de múltiplos processos ou threads, que trabalham em uma mesma tarefa na busca de um resultado comum.

A programação concorrente pode se beneficiar mesmo em sistemas que não possibilitem o paralelismo. Em sistemas de mútiplos processadores, a possibilidade do paralelismo estende as vantagens da programação concorrente.

Esse tipo de programação pode levar a situação indesejadas, uma vez que a compartilhamento de recusos (arquivos, registradores, etc.). Então, é necessário utilizar alguns mecanismos com o objetivo de garantir o processamento correto dos códigos.

Alguns  conceitos relacionados a isso serão apresentados a frente, como por exemplo, semáforos, monitores, deadlock, etc. 

# Aplicações concorrentes

Em uma aplicação concorrente, é necessário que os processos se comuniquem. Essa comunicação pode ser feita através de mecanismos, como variáveis compartilhadas na memória principal ou troca de menssagens. Nesse caso, é necessário que os processos concorrentes tenham a sua execução sincronizada através do S.O (pelos mecanismos de sincronização).

Nota: processo, subprocesso e thread são tratados com o mesmo significado nesse assunto.

## Especificação de Concorrência em programas

Uma das primeiras notações utilizadas para especificar a concorrência em um programa foi a FORK e JOIN (Conway, 1963).

Veja o exemplo:

![[Pasted image 20230507173125.png]]

O que acontece nesse caso é que o comando FORK inicia um novo processo para a execução do programa B. Então, o comando JOIN garante que A sincronize com o B, ou seja, A só continuará a processar após o termino da execução de B.

Outra notação utilizada  para expressar a concorrência é o PARBEGIN e o PAREND (posteriormente chamados de COBEGIN e COEND).

O PARBEGIN especifica que a sequência de comandos seja executada concorrentemente atrave´s da criação de um novo processo. 

O PAREND defini um ponto de sincronização, nesse caso, o processamento somente irá continuar quando todos os novos processos tiverem terminado sua execução. Observe o exemplo:

![[Pasted image 20230507173715.png]]

![[Pasted image 20230507173701.png]]

## Problemas de compartilhamento de recursos

Para exemplificar problemas que podem acontecer na programação concorrente, devido a utilização de compartilhamento de recursos, vamos especificar duas situações. Primeiramente o compartilhamento de um arquivo em disco, em seguida, o compartilhamento de uma variável na memória principal.

Imagine a seguinte situação, tem-se um programa o Processo A soma 1 a variável X e o Processo B diminui 1 da mesma variável que está sendo compartilhada. Inicialmente a variável X possui o valor 2.

![[Pasted image 20230507174242.png]]

Nem sempre após a exeuçãos dos processo A e B em sequência, o valor de X continuaria 2. Observe a decomposição dos comandas de atribuição:

![[Pasted image 20230507174346.png]]

Considere que o processo A carrega o valor de X no registrador Ra, some 1 e, antes de guardar o valor de X seja interrompido. Nesse instante, o processo B inicia sua execução, carrega X em Rb e subrtrai 1, após isso, o processo B também é interrompido dando lugar ao processo A. O processo A atribui o valor 3 a X, em seguida, B atribui o valor 1 a X e sobrepõe aquilo que havia sido gravado pelo processo A. 

No exemplo citado acima, o valor final da variável X é incosistente em função da forma concorrente com que os dois processos executaram.

## Exclusão Mútua

Uma solução simples para evitar que isso aconteça é impedir que dois ou mais processos acessem um mesmo recurso simultâneamente. Essa ideia de exclusividade é chamada de *exclusão mútua*.

A exclusão mútua deve afetar  os processos concorrentes somente um deles estiver utilizando um recurso compartilhado, a parte do código onde é feito o acesso ao recurso compartilhado é chamada de *região crítica*. Se for possível evitar que dois processos entrem em suas regiões críticas simultâneamente, ou seja, garantiada a execução mutualmente exclusiva das regiões, os problemas de comaprtilhamento serão evitados.

Para garantir a exclusão mútua, existem protocolos de entrada e saída dos procesos, ou seja, apra um processo executar instruções de sua região crítica primeiramente ele deve executar o protocolo de entrada. Ao terminar, o mesmo deve executar um protocolo de saída.

![[Pasted image 20230507184351.png]]

Dessa forma, o protocolo de entrada garante que o recurso somente possa ser acessado caso esteja livre. O protocolo de saída informa aos outros processos que o recurso já está livre.

Além de garantir a exclusão mútua, duas sistuações indesejadas devem ser evitadas.

* Starvation

Também chamado de espera indefinida, o Stavation é uma situação em que o processo nunca consegue executar sua região crítica. Para esse caso, pode acontercer de no momento em que o recurso é liberado, o S.O pode utilizar um critério que nunca seleciona esse determinado processo para utilizar o recurso da região critica. Os critérios de escolha aleatória ou com base em prioridade possibilitam com que isso aconteça.

Uma solução simples para o problema é a criação de filas de pedido de alocação para cada recurso utilizando um esquema FIFO.

* Região crítica ocupada indevidamente

A outra situação indesejada na implemtação da exclusão mútua é aquela em que um determinado processo fora de sua região critica impeça que outros processos entrem nas suas prórpias regiões criticas.

---

Existem diversas soluções para garantir a exclusão mútua, sendo estas podendo ser implementadas em hardware e em software.

### Soluções de Hardware

* Desabilitação de interrupções

É uma solução simples para o problema da exclusão mútua porém com diversos desafios. Sua proposta é desabilitar as interrupções antes de um processo entrar na sua região crítica. Como não haveria mudança de contexto nesse caso, o processo que desabilitou as interrupções teria acesso exclusivo garantido.

Essa solução apresenta limitações. Primeiramente, esse tipo de abordagem compromete os princípios da multiprogramação. Além disso, poderia existir um risco de um programa desabilitar as interrupções e não voltar a habilitá-las. 

Além disso, com múltiplos processadores poderia ser uma solução ineficiente devido ao tempo de propagação quando um processador sinaliza aos demais que as interrupções devem ser habilitadas ou desabilitadas. Outra consideração é que o mecanismo de clock do sistema é implementado atrave´s de interrupções, ou seja, essa solução deve ser utilizada com bastante critério.

Apesar do apresentado, essa solução pode ser útil para situações como execução de parte do núcleo do sistema operacional, isso pode garantir que não ocorram problemas de inconsistência em suas estruturas de dados durante a execução de algumas rotinas.

* Instrução test-and-set

Muitos processadores possuem uma instrução de máquina especial que permite ler uma variável, armazenar seu conteúdo em outra área e atribuir um novo valor a mesma variável. Essa instrução especial é chamada de *instrução test-and-set* e tem como característica ser executada sem interrupção, ou seja, trata-se de uma instrução indivisível.

A instrução Test and Set consiste em duas etapas: Teste e Definição. Durante a etapa de teste, a instrução lê o valor atual de uma variável compartilhada e verifica se ela está definida como "verdadeira" ou "falsa". Se a variável for "falsa", o processo pode acessar o recurso compartilhado, caso contrário, o processo deve esperar até que a variável se torne "falsa". Na etapa de Definição, a instrução define o valor da variável como "verdadeiro" para indicar que um processo está acessando o recurso compartilhado e impede que outros processos possam acessá-lo. (GPT)

O uso dessa abordagem oferece vantagens, como por exemplo, a simplicidade na implementação e o uso da solução em arquiteturas de múltiplos processadores. A principal desvantagem é a possibilidade de starvation, advinda da  arbitrariedade na seleção do processo para acesso ao recurso.


### Soluções de software

* Primeiro algorítimo

O primeiro algorítimo foi implementado para solucionar a situação de dois processos. No caso, o mecanismo utiliza uma variável de bloqueio, indicando qual processo pode ter acesso a região crítica.

Exemplificando, sejam dois processos A e B, inicialmente, a variável global Vez é igual a 'A', indicando que o processo A pode executar sua região crítica. O processo B somente executará a sua região crítica quando o processo A atribuir o valor 'B' a variável de bloquei Vez. Dessa forma, fica garantida a exclusão mútua.

![[Pasted image 20230507195541.png]]

Apesar de funcionar, esse algorítimo é limitado. No caso, o acesso ao recurso compartilhado somente pode ser realizado por dois processos e sempre de maneira alternada. Com isso, um processo que necessite utilizar o recurso mais vezes do que outro, permanecerá grande parte do tempo bloqueado. No algorítimo apresentado, caso o Processamento_A demore muito tempo, é possível que o processo B queira executar a sua região crítica e não consiga. Como o Processo B pode executar seu loop mais rapidamente  que o Processo_A, a possibilitade de executar sua região crítica fica limita a velocidade do Processo A.

Outro problema é que, caso ocorra algum problema com um dos processos de forma que a variável de bloqueio não seja alterada, o outro processo permanecerá indefinidamente bloqueado.

* Segundo algorítimo

O segundo algorítimo evita a utilização de uma única variável global de bloqueio e institui uma variável para cada processo (CA e CB). que indica se o processo está ou não em sua região crítica. Nesse caso, toda vez que um processo deseja entrar em sua região crítica, a variável do outro processo é testada para verificar se o recurso está livre para uso.

![[Pasted image 20230507200227.png]]

Nesse caso, o uso do recurso não necessariamente é realizado alternado. Caso ocorra algum problema com um dos processos fora da região crítica, o outro processo não fica bloqueado. Caso um processo tenha um problema dentro de sua região crítica ou antes de alternar a variável, o outro permanecerá indefinidamente bloqueado. Na prática, essa solução é pior que a primeira pois nem sempre garante a exclusão mútua.

* Terceiro algorítimo

O terceiro algorítimo tenta solucinar o problema do segundo colocandop a instrução de atribuição das variáveis CA e CB antes do loop de teste.

![[Pasted image 20230507200556.png]]

Essa aleração garante a exclusão mútua, entretanto, introduz um novo problema, que é a possibilidade de bloqueio indefinido de ambos os processos. Caso os dois processos alterem as variáveis CA e CB ao mesmo tempo antes da execução do while, ambos não poderão entrar em suas regiões críticas.

* Quarto algorítimo

Como no terceiro algorítimo, o quarto algorítimo apresenta uma implementação onde o processo, da mesma forma, altera a variável antes de entrar em sua região crítica, porém, existe a possibilidad de essa alteração ser revertida.

![[Pasted image 20230507200914.png]]

Essa solução garante a exclusão mútua e não gera um bloqueio simultâneo, entretanto, uma nova situação indesejada pode ocorrer. No caso, se os tempos aleatórios forem muito próximos e a concorrência gerar uma situação onde os dois processos alterem a as variável CA e CB para falso antes do termino do loop, nenhum dos processos irá conseguir executar sua região crítica. Mesmo com essa situação não sendo permanente, pode ocasionar alguns problemas

* Algorítimo de Dekker

Foi a primeira solução de software que garantiu a exclusão mútua sem a incorrência de outros problemas. Esse é um algorítimo de fato complexo e pode ser encontrado em Ben-Ari(1990). Posteriormente, Peterson propôs uma solução mais simples.

* Algorítimo de Peterson

Similar ao terceiro algorítimo, além das variáveis de condição CA e CB, introduz a variável de Vez para resolver os conflitos gerados pela concorrência.

Antes de acessar a região crítica, o processo sinaliza esse desejo através da variável de condição, porém, o processo cede o uso do recurso a outro processo indicado pela variável Vez. Em seguida, é executado um WHILE como protocolo de entrada na região crítica. Nesse caso, além da garantia de exclusão mútua, o bloqueio indefinido de um dos processos não ocorrerá, já que a variável Vez sempre permitirá a continuidade de execução de um dos processos.

![[Pasted image 20230510151144.png]]

* Algorítimo para execlusão mútua de N processos

Apesar de as soluções apresentadas garatirem a exclusão mútua, todas possuem uma deficiência (busy wait). Essa deficiência consome tempo do processador desnecessariamente.

A solução para esse problema foi a introdução de mecanismos de sincronização que permitiram que um processo, quando não pudesse entrar em sua região crítica, fosse colocado em estado de espera. Esses mecanismo são conhecidos como semáforos e monitores.

## Sincronização condicional

Sincronização condicional é uma situação onde o acesso ao recurso compartilhado exige a sincronização de processos vinculada a uma condição de acesso. Determinado recurso pode ainda não se encontrar na condição para uso e, nesse caso, o processo que seja acessá-lo fica bloqueado até que o recurso fique disponível.

Como exemplo desse tipo de processo, imagine o seguinte problema de sincronizão condicional (problema produtor/consumidor). O recurso compartilhado é um buffer, sempre que a variável Cont for igual a zero, significa que o bugger está vazio e o processo conssumidor deve permanecer aguardando até que se grave um dado. Da mesma forma, quando a variável Cont foir igual a TamBuf(variável que indica que o buffer está cheio), significa que o processo Produtor deve aguardar a leitura de um novo dado. Nessa solução, a tarefa de colocar e retirar dados do buffer é realizada por dois procedimentos (Grava_buffer e Le_buffer), executados de forma mutualmente exclusiva (execto para a variável Cont que é compartilhada).

![[Pasted image 20230510151959.png]]

Esse modelo resolve a questão da sincronização, porém, a questão da esperá ocupada ainda está presente.

## Semáforos

O conceito de semáforo é um mecanismo de sincronização que permite implementar, de forma simples, a exclusão mútua e a sincronização condicional em processos.

Um semáforo é uma variável inteira, não negativa, que só pode ser manipulada por duas instruções (UP e DOWN. A execução dessas instruções não pode ser interrompida.  A  instrução UP  incrementa uma unidade ao valor do semáforo, enquanto a instrução DOWN decrevemnta a variável. Como, por definição, valores negativos não podem ser atribuídos a um semáforo, a instrução DOWN executa em um semáforo com o valor 0, faz com que o processo entre em estado de espera.

Os semáforos podem ser do tipo binários ou contadores. Os semáforos binários, também chamados de mutexes, só podem assumir o valor zero ou um enqunaot os semáforos contadores podem assumir qualquer inteiro positivo.

### Exclusão mútua utilizando semáforos

A implementação da exclusão mútua de semáforos faz com que o busy await não aconteça, sendo uma alternativa de melhor befícios as demais.

As instruções UP e DOWn funcionam como protocolos de entrada e saída de um processo a sua região crítica. Se o valor do semáforo é igual a 1, indica que nenhum processo está utilizando o recurso, enquanto o valor 0 indica que o recurso está ocupado.

Quando um processo deseja entrar em sua região crítica ele executa uma instrução DOWN. Caso a instrução seja executada em um semáforo 0, o processo fica impedido do acesso, permanecendo em estado de espera (não gerando overhead do processador).

Ao terminar a utilização da região crítica, um determinado processo executa uma instrução UP no semáforo, liberando o acesso ao recurso. O sistema selecionará um processo de fila de espera alternando o estado do próximo processo da fila para pronto.

![[Pasted image 20230510152929.png]]



![[Pasted image 20230510152949.png]]

![[Pasted image 20230510153005.png]]


### Sincronização condicional utilizando semáforos

Além de permitir a exclusão mútua, os semáforos permitem a sincronização condicional.

No caso do problema do produtor/consumidor são utilizados dois mutexes para essa operação, quando o semáforo Vazio for 0, significa que o buffer está cheio e o processo produtor deverá aguardar,  analogamente, quando o semáforo Cheio for 0,significa que o buffer está vazio e consumidor deverá aguardar.


___ 
Semáforos do tipo contadores também são bastante úteis, aplicáveis em problemas de sincronização condicional onde existem processos concorrentes alocando recursos do mesmo tipo (pool de recursos). Nesse caso, o semáforo é iniciado com o número total de recursos, executa um down subtraindo um de recurso disponíveis, e assim por diante.


