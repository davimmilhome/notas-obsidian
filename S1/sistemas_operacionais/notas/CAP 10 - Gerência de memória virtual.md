
Memória virtual é uma técnica sofisticada de gerência de memória, onde as memórias principal e secundária são combinadas, dando ao usuário a ilusão de existir uma memória muito maior do que a capacidade real da memória principal. Esse conceito fundamenta-se em não vincular o endereçamento feito pelo programa aos endereços físicos da M.P. Dessa forma, os programas podem possuir endereços associados também a memória secundária.

Outra vantagem desse tipo de  técnica é permitir que um número maior de processos esteja alocado na M.P, já que apenas partes de cada processo estarão residentes de fato. Isso leva a uma utilização mais eficiente do processador.

# Espaço de endereçamento virtual

O conceito de memória virtual se aproxima muito da ideia de vetor, existente nas linguagens de alto nível. Quando um programa faz referência a um elemento do vetor, não há preocupação em saber a posição de memória daquele dado. O compilador se encarrega dessa função, tornando esse mecanismo invisível ao programador.

Um programa no ambiente de memória virtual não faaz referencia a endereços físicos, mas apenas endereços virtuais. No mento da execução de uma instrução, o endereço virtual referenciado é traduzido para um endereço físico, pois o processador manipula apenas posições da memória principal. O mecanismo de tradução do endereço virtual para o físico é chamado de *mapeamento*.

Como o espaço de endereçamento virtual não tem relação direta com os endereços no espaço real, um programa pode fazer referência a endereços virtuais que estejam foram dos limites da memória principal, ou seja, os programas e estruturas de dados não estão limitados à memória física. Quando um programa é executado, somente parte de seu código fica residente na M.P, permanecendo o restante na memória secundária até o momento de ser referenciado.

Nota:  No desenvolvimento de aplicações, a existência de endereços virtuais é ignorada pelo programador. Compiladores e linkers se encarregam de gerar um código executável em função do espaço de endereçamento virtual e o S.O se encarrega de sua execução.

# Mapeamento

O mecanismo que traduz o endereço localizado no espaço virtual para o endereço associado no espaço real é chamado de mapeamento. Uma de suas consequências é que um determinado programa não precisa mais estar alocado em um espaço contíguo na M.P.

![[Pasted image 20230530113854.png]]

Em sistemas modernos, a tradução dos endereços virtuais é realizada por um hardware juntamente com o sistema operacional, de forma a não comprometer seu desempenho e torná-lo transparente a usuários e aplicações. O dispositivo de hardware responsável por essa tradução é conhecido como unidade de gerência de memória (MMU), sendo acionado sempre que se faz referência a um endereço virtual.

*Cada processo tem o seu espaço de endereçamento virtual, como se possuísse sua própria memória.* O mecanismo de tradução se encarrega de manter tabelas de mapeamento exclusivas para cada processo, relacionando os endereços virtuais do processo com seus endereços reais.

![[Pasted image 20230530114128.png]]

A troca de tabelas de mapeamento é realizada através de um registrador, que indica a posição inicial da tabela corrente, onde, toda vez que há mudança de contexto, o registrador é atualizado com o endereço da nova tabela.

De maneira a otimizar o espaço utilizado, essas tabelas mapeiam blocos de dados, cujo o tamanho determina o número de entradas existente na tabela de mapeamento.

Quanto maior o bloco menos entradas nas tabelas de mapeamento e, consequentemente, tabelas de mapeamento ocupam menos espaço na memória.

![[Pasted image 20230530114346.png]]

Existem S.Os que trabalham apenas com blocos de tamanho fixo (técnica de paginação). Enquanto outros utilizam blocos de tamanho variável (técnica de segmentação). Existe ainda um terceiro tipo de sistema que implementa as duas técnicas.

## Memória virtual por paginação

É a técnica de gerência onde o espaço de endereçamento virtual e o espaço de endereçamento real são devidos em blocos de mesmo tamanho, chamados páginas. Nesse caso, há distinção entre páginas reais (ou frames) e páginas virtuais.

Cada página virtual de um processo possui uma entrada na tabela de páginas (ETP), com informações de mapeamento que permitem ao sistema localizar a página real correspondente.

Quando um programa é executado, as páginas virtuais são transferidas da memória secundária para a memória principal e colocadas nos frames. Sempre que um programa fizer referência a um endereço virtual, o mecanismo de mapeamento localizará  na tabela o endereço físico do frame no qual se encontra o endereço real correspondente.

Nessa técnica, o endereçamento virtual é formado pelo número da página virtual (NVP) e por um deslocamento. O NVP identifica unicamente a página virtual que contém o endereço. O deslocamento indica a posição do endereço virtual em relação ao início da página na qual se encontra.

A ETP também possui outras informações além do endereço, como o bit de validade que indica se uma página está ou não na memória principal.

![[Pasted image 20230530133043.png]]

![[Pasted image 20230530133108.png]]

Caso um processo referencie um endereço virtual e na verificação do bit de validade indicar que a página não está na M.P (page fault) o sistema realiza uma operação de E/S conhecida como paginação.

O número de page faults gerados por cada processo em um determinado intervalo de tempo é conhecido como taxa de paginação do processo.

Nota: quanto mais alta a taxa de paginação, mais lento é um processo, passando para o estado de execução para o estado de espera.

### Políticas de busca de páginas

A política de busca de páginas determina quando uma página deve ser carregada para a M.P. Seguindo, existem duas estratégias: paginação por demanda e paginação antecipada.

 * Paginação por demanda (demand paging)

Na paginação por demanda, as páginas de um determinado programa somente são levadas para a M.P quando são referenciadas. Sendo possível que certas partes do programa nunca sejam carregadas.

 * Paginação antecipada
Nesse caso, o sistema carrega além da página referenciada, outras páginas que podem ou não ser necessárias ao longo do processamento. Nesse caso, há uma economia de tempo em levar páginas em conjunto, entretanto, é possível ocupar a memória principal desnecessariamente.

A técnica de paginação antecipada pode ser empregada no momento da criação de um processo ou na ocorrência de um page fault. Quando um processo é criado, diversas páginas do programa na memória principal devem ser carregadas, gerando um elevando número de pagefaults. Na medida em que as páginas são carregadas para a memória, a taxa de paginação tende a diminuir. 

Nesse cenário, se o sistema carregar um conjunto de páginas, a taxa de paginação do processo irá cair imediatamente e estabilizar-se durante um certo período de tempo.


### Políticas de alocação de páginas

Essa política determina quantos frames cada processo pode alocar na memória principal. Geralmente, se segue um dos dois caminhos: alocação fixa e alocação variável.

 * Alocação fixa
Na política de alocação fixa, cada processo tem um número máximo de frames que pode ser utilizado durante a execução do programa. Caso o número de páginas seja insuficiente, uma página do próprio processo deve ser descartada para alocar uma nova.

Essa geralmente não é uma boa ideia pois é raro que diferentes processos tenham a mesma necessidade de memória. 

O limite de páginas que um programa pode alocar deve ser definido na criação do processo, com base no tipo de aplicação que será executada. Essa informação faz parte do contexto de software de seu respectivo processo.

Nesse caso, podem ocorrer os seguintes cenários

-Número de páginas muito pequeno

O processo tenderá a ter uma alta taxa de paginação.

-Número de páginas muito grande

Cada processo irá ocupar na M.P um espaço maior do que o necessário, reduzindo o número de processos residentes na M.P e o grau de mutiprogramação. Nesse caso, o sistema pode implementar o swapping, retirando e carregando processos da/para memória principal.

 * Política de alocação variável
Nesse modelo, o número máximo de páginas alocadas a um processo pode variar em função da sua execução, taxa de paginação e ocupação da M.P. Nesse modelo, processos com elevadas taxas de paginação podem ampliar o limite máximo de frames, a fim de reduzir o n° de page faults. Da mesma fora, processos com baixas taxas de paginação podem ter páginas relocadas para outros processos. 

Esse mecanismo, apesar de mais flexível exige monitoramento  constante do S.O sobre o comportamento dos processos, gerando overhead.

### Políticas de substituição de páginas

Uma página quando é liberada por um processo, está livre para ser utilizada por qualquer outro processo. Entretanto, há um problema, qualquer substituição de página deve considerar se uma determinada página foi modificada ou não antes de liberá-la, caso contrário, os dados armazenados serão perdidos. Nesse caso, o sistema deverá gravar a página memória secundária antes do descarte.

Com esse propósito, o sistema mantém um arquivo de paginação (page file), onde todas as páginas modificadas e descartadas são armazenadas. Sempre que uma página modificada for novamente referenciada, ocorrerá um page in carregando-a para a M.P direntamente do arquivo de paginação.

O sistema operacional consegue identificar páginas modificadas através de um bit que existe em cada entrada da tabela de páginas, chamado bit de modificação.

A política de substituição de páginas pode ser classificada conforme seu escopo. Em função desse escopo, a política de substituição pode ser local ou global.

 * Política de substituição local
Nessa política, apenas páginas do processo que gerou o page fault são candidatas a relocação.

 * Política de substituição global
Nesse caso, todas as páginas alocadas na M.P são candidatas a substituição, independente do processo que gerou o page fault. Nesse  caso, há algumas páginas que são marcadas como bloqueadas e não podem ser relocadas como, por exemplo, as do núcleo do sistema.

Há uma relação entre o escopo da política de substituição e a política de alocação. 

A política de alocação fixa permite apenas utilização da política de substituição local.

Já na política de alocação variável, é possível utilizar tanto uma política de substituição local quanto global.

# Working Set

O mecanismo de memória virtual introduz um sério problema. Como cada processo possui na M.P apenas algumas páginas alocadas, o sistema deve manter um conjunto mínimo de frames buscando uma baixa taxa de paginação. AO mesmo tempo, o sistema deve impedir que um processo tenha um número excessivo de páginas na memória.

Nota: a situação em que há um elevado número de page faults é chamada de trashing, e provoca queda no desempenho do sistema devido ao elevado número de operações de E/S.

O conceito de working set surgiu com o objetivo de reduzir o problema do trashing e está relacionado ao princípio da localidade. Existem dois tipos de localidade. A localidade espacial que é a tendência de que após uma referência a uma posição de memória sejam realizadas novas referências a endereços próximos. Por usa vez, a localidade temporal é a tedência de que após uma referência a uma posição de memória, esta mesma posição seja referenciada novamente.

Por conta desse princípio, no início da execução de um programa, observa-se um elevado número de page faults, pois não existe nenhum frame de programa na memória principal. Com o decorrer de suas execução, as páginas são carregadas para a memória e o número de page faults diminui. Após um período de estabilidade, o programa gera novamente uma levada taxa de paginação, que depois de algum tempo volta a estabilizar. Esse fenômeno geralmente se repete inúmeras vezes durante a execução de um processo.

A partir da observação desse conceito, foi criada a definição de working set, que é o conjunto de págines oferecidas por um processo durante determinado intervalo de tempo, esse intervalo de tempo é denominado janela do working set.

Dentro da janela do working set, o número de páginas distintas referenciadas é conhecido como o tamanho do working set.

O modelo proposto de working set possibilita prever quais páginas são necessárias à execução de um programa de forma eficiente. Caso a janela do working set seja apropriadamente selecionada, em função da localidade do programa, o S.O deverá manter as páginas do working set de cada processo residentes na M.P. Considerando que a localidade de um programa varia durante a execução, o tamanho do working set do processo também varia, ou seja, seu limite de páginas reais deve acompanhar essa variação.

Caso o limite de páginas reais de um processo seja maior do que o working set, menor será a chance de ocorrer um page fault. Por outro lado, as páginas do processo ocuparão um espaço desnecessário na memória. No caso do limite ser menor, a taxa de paginação será alta. Outro fato importante que pode ser observado é a existência de um ponto onde o aumento de limite de páginas reais não implica em diminuição direta da taxa de paginação, sendo este ponto alcançado muito antes de o programa ser totalmente carregado na memória.

A implementação desse modelo é muito difícil e, devido a necessidade de retirar páginas e manter páginas em funçao da última janela de tempo, o modelo working set somente é implementado em sistemas que utilizam a política de alocação de páginas variável.

Uma maneira de se implementar esse modelo é analizar a taxa de paginação de cada processo conhecida como *estratégia de frequência de page faults* . Caso um processo tenha uma taxa de paginação acima de um limite definido pelo sistema, o processo deverá aumentar seu limite de páginas reais na tentativa de alcançar seu workin set. Por outro lado, se o processo tem uma taxa de paginação abaixo de um certo limite, o sistema poderá reduzir o limite de páginas sem comprometer seu desempenho.

## Algoritmos de substitução de páginas

Os algoritmos de substituição de páginas têm o objeto de selecionar os frames que tenham as menores chances de serem referenciados em um futuro próximo. Apartit do princípio da localidade, a maioria dos algoritmos tenta prever o comportamento futuro das aplicações em função do comportamento passado, avaliando o número de vezes que uma página foi referenciada, o momento em que foi carregada para a M.P e o intervalo de tempo da última referênicia.

A melhor estratégia de substituição de páginas seria aquela que escolhesse um frame que não fosse mais utilizado no futuro ou que levasse o maior tempo para ser referenciado novamente. Porém, quanto mais sofisticado o algorítimo de substituição, maior o overhead para o sistema operacional. Seguem alguns algorítimos:

 * Ótimo
O algoritmos ótimo seleciona pra a substituição uma página que não será mais referenciada no futuro, ou aquela que levará um maior intervalo de tempo para ser utilizada novamente. Apesar de esse algoritmos garantir a menor taxa de paginação, ele é impossível de ser implementado pois o S.O não tem como conhecer de antemão o comportamento futuro das aplicações. Essa estratégia é utilizada como modelo comparativo.

* Aleatório
O algoritmo aleatório não utiliza critério de seleção. Todas as páginas alocadas tem a mesma chance de serem selecionadas. Essa é a estratégia que consome menos recursos do sistema, porém, tem baixa eficiência.

 * FIFO
No algoritmo FIFO, a página que primeiro for utilizada será a primeira a ser escolhida, ou seja, a que está a mais tempo na M.P. É razoável pensar que uma página que esteja a mais tempo na memória seja aquela que deva ser selecionada, porém isto nem sempre é verdade. No caso de uma página ser referenciada constantemente (como uma estrutura de dados), o fator tempo torna-se irrelevante e o sistema tem que referenciar a mesma página diversas vezes ao longo do tempo.

 * LFU (least-frequently-used)
O algoritmo LFU seleciona a página menos referenciada. Para isso, é mantido um contador com o número de referências para cada página na memória principal.

Esta também parece ser uma boa estratégia, porém, páginas que estão há pouco tempo na M.P podem ser, justamente, aquelas selecionadas. É possível também que páginas muito referenciadas no passado não sejam mais utilizadas. Por conta desses problemas, esse algoritmo também é raramente implementado.

* NRU (not-recently-used)
O algoritmo NRU é semelhante ao LRU, porém, com menor sofisticação. Para a implementação desse algoritmo é necessário um bit adicional conhecido com bit de referência. Esse bit indica se a página foi utilizada recentemente.

Periodicamente, o sistema altera o valor do bit de referência para 0. Dessa forma, é possível distinguir quais frames foram recentemente referenciados. No momento de substituição de páginas o sistema seleciona um dos frames que não tenha sido utilizado recentemente.

O NRU torna-se mais eficiente se o bit de modificação for utilizado em conjunto com um bit de referência. NEsse caso, é possível classificar as páginas em quatro categorias. A ordem de prioridade para trocar é crescente, ou seja, a primeira página a ser trocada é aquela com o menor valor em categoria.

![[Pasted image 20230531131228.png]]

 * FIFO com buffer de páginas
Essa variação do FIFO combina uma lista de páginas alocadas (LPA) com uma lista de páginas livres (LPL). 

A LPA organiza as páginas alocadas a mais tempo na memória no início da lista, da mesma forma, a LPL organiza todos os frames livres da M.P. SEmpre que um processo necessita alocar uma nova página, o sistema utiliza a primeira utiliza a primeira página da LPL, colocando-a no final da LPA. Caso o processo tenha que liberar uma página,  será selecionada o frame em uso a mais tempo na memória, ou seja, o primeiro na LPA, colocando-o no final da LPL.

No caso, uma página na LPL continua disponível na memória principal. Caso a página seja novamente referenciada e ainda não tenha sido alocada, basta reitra-la da LPL e devolvela ao processo. Nesse caso, a LPL funciona como um buffer de páginas, evitando o acesso a memória secundária. POr outro lado, se a página não for mais referenciada, com o passar do tempo irá chegar ao início da LPL e será utilizada para um outro processo.

Esse algoritmo de substituição é simples e eficiente, sem o custo de outras implementações.

 * FIFO circular (clock)
Nesse algoritmo, as páginas estão alocadas em uma estrutura de lista circular, semelhante a um relógio. Este algoritmo é implementado, com pequenas variações na maioria dos sistemas Unix.

Nessa implementação existe um ponteiro que aponta para a página mais antiga na lista. Cada página possui um bit de referência, indicando se a página foi recentemente referenciada.  Quando é necessário substituir uma página, o sistema verifica se o frame apontado possui o bit de referencia desligado, caso não, passa para a próxima pagina. Além disso, o ponteiro após passar por uma página troca o bit de referência para zero.

É possível melhorar a eficiência desse algoritmo utilizando o bit de modificação, juntamente com o bit de referência.

## Tamanho da página

O tamanho da página está associado a arquitetura do hardware e varia de acordo com o processador. Normalmente está entre 512 e 16 M de endereços.

Nesse caso, páginas pequenas necessitam de tabelas de mapeamentos maiores e provocam maior taxa de paginação. Entretanto, páginas grandes apesar de diminuir o tamanho da tabela de páginas aumenta a fragmentação interna.

O principal argumento a favor do uso de páginas pequenas é a melhor utilização da memória. A partir do princípio da localidade, com páginas pequenas teríamos na memória apenas partes dos programas com maiores chances de serem executadas. QUanto maior o tamanho da página, maiores as chances de ter na memória código pouco referenciado.

Outra preocupação é o tempo de leitura e gravação da memória secundária. Devido ao seu modo de funcionamento, o tempo de operações de E/S com duas páginas de 512bytes é muito maior do que em uma página com 1024bytes.

Com o aumento do espaço de endereçamento e da velocidade de acesso a M.P, a tendência no projeto de S.O com memória virtual é a adoção de páginas maiores, apesar dos problemas citados.

## Paginação em múltiplos níveis

Em sistemas que implementam apenas um nível de paginação, o tamanho das tabelas de páginas pode ser um problema.

Uma boa solução é utilização de tabelas de páginas em múltiplos níveis. A ideia é que o princípio de localidade seja aplicado também as tabelas de mapeamento, ou seja, apenas informações sobre páginas realmente necessárias estariam na M.P..
![[Pasted image 20230531133305.png]]


## Translation lookaside buffer

O mapeamento de endereços virtuais em reais implica que pelo menos dois acessoas a M.P serão feitos: primeiramente o da tabela de páginas e a própria página.

Como a maioria das aplicações referencia um número reduzido de frames na M.P, seguindo o princípio da localidade, somente uma pequena fração da tabela de mapeamento é realmente necessária. Com base nesse princípio, foi introduzida uma memória especial chamada *translation lookaside buffer* (TLB), com o intuito de mapear endereços virtuais em endereços físicos, sem a necessidade de acesso a tabela de páginas.

O TLB funciona como um cache, mantendo apenas as traduções dos endereços virtuais mais recentemente referenciados.

Na tradução de um endereço virtual, o sistema verifica primeiro o TLG. Então, há situações de TLG hit e TLB miss.

Sempre que a TLB fica com todas as suas entradas cheias, para que uma nova entrada aconteça um endereço deve ser liberado. Geralmente a ténica de relocação é aleatória, raramente utiliza-se também um NRU.

### Proteção de memória

Como existe o compartilhamento da memória principal, o S.O deve impedir que um processo tenha acesso ou modifique uma página do sistema sem autorização.

Um primeiro nível de proteção é inerente ao próprio mecanismo de memória virtual por paginação. Nesse esquema, cada processo tem sua própria tabela de mapeamento e a tradução dos endereços é realizada pelo sistema. A ptroteção de acesso é realizada individualmente em cada página da memória principal, utilizando-se as entradas da tabela de mapeamento, onde alguns bits especifícam os acessos permitidos.

Os tipos básicos de acesso são leitura e gravação. 

![[Pasted image 20230531155557.png]]


## Compartilhamento de memória

Em sistemas que implementam memória virtual, é bastante simples a implementação da reentrância, possibilitando o compartilhamento de código entre os diversos processos. Para isso, basta que as entradas das tabelas de mapeamento dos processos apontem para os mesmos frames na memória principal, evitando, assim, várias cópias de um mesmo programa na memória.

## Memória virtual por segmentação
....








[[EXERCÍCIOS]]