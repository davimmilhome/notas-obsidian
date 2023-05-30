
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



[[EXERCÍCIOS]]