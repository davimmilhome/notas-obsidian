
A gerência de memória é uma tarefa complexa e tem como objeto maximizar o número de usuários e aplicações utilizando eficientemente o espaço da memória principal.

O processador somente executa instruções localizadas na memória principal, portanto o sistema operacional deve sempre transferir programas da memória secundária para a memória principal antes de serem executados. Além disso, o tempo de acesso à memória secundária é muito superior ao da memória principal, sendo assim, o sistema operacional deve buscar reduzir o número de operações de E/S na memória secundária.

Mesmo na ausência de espaço livre, o sistema deve permitir que novos processos sejam executados. Isso só é possível através da transferência temporária de processos residentes na memória principal para a memória secundária. Esse mecanismo é conhecido como *swapping*

Outra preocupação na gerência de memória é permitir a execução de programas que sejam maiores que a memória física disponível, implementando técnicas como *overlay e memória virtual.*

Continuando, também é de preocupação do S.O fornecer mecanismos de compartilhamento de memória para que diferentes processos possam trocar dados de forma protegida.

# Alocação contígua simples

Nesse tipo de organização (para sistemas monoprogramáveis),  a memória principal é subdividida em duas áreas: uma para o S.O e outra para programa do usuário. Então, o programador deve desenvolver sua aplicação preocupado, apenas, em não ultrapassar o espaço de memória disponível.

![[Pasted image 20230529221936.png]]

Nessa implmentação, o usuário tem controle sobre toda a memória principal. Então, para proteger o S.O de um acesso indevido, alguns sistemas implementam proteção através de um registrador que delimita as áreas do sistema operacional e do usuário. Nesse caso, sempre que um programa faz referência a um endereço na memória, o sistema verifica se o endereço está dentro dos limites permitidos ao usuário.

Essa implementação, apesar de simples, não é eficiente pois limita o número de usuários com acesso ao recursos e também pode gerar subutilização da memória principal, caso essa não seja totalmente ocupada por um programa.

# Técnica de overlay

Na alocação contígua simples, todos os programas estão limitados ao tamanho da memória principal (MP) disponível ao usuário. Uma solução para esse problema é dividir o programa em, módulos, de forma que seja possível a execução independente de cada módulo, utilizando a mesma área da memória (área de overlay). Essa técnica é chamada de overlay.

A utilização dessa técnica exige cuidado, pois pode trazer implicações tanto na sua manutenção quanto no desempenho das aplicações, devido à possibilidade de troca excessiva de contexto entre os módulos.

# Alocação particionada

A alocação particionada é uma técnica que surgiu para ser implementada já em sistemas mutiprogramáveis, no sentido de tentar otimizar a utilização de recursos. Ela pode ser dividida em duas:

 * Alocação Particionada Estática

Nessa implementação, a memória era dividida em tamanhos fixos, chamados de partições. O tamanho das partições era estabelecido na fase de boot do sistema, em função dos programas que seriam utilizados.

![[Pasted image 20230529222932.png]]

Inicialmente, os programas somente podiam ser carregados e executados e uma partição, mesmo se outras estivessem livres. Essa limitação se devia aos compiladores e montadores, que geravam apenas códigos absolutos. No código absoluto, todas as referências a endereços no programa são posições físicas na memória principal, ou seja, o programa só poderia ser carregado a partir do endereço de memória especificado em seu próprio código. Esse tipo de gerência de memória chamou-se de *Alocação Particionada Estática Absoluta.*

Com a evolução dos compiladores, montadores, linkers e outros, *o código passou a ser relocável*. Ou seja, todas as referências a memória no programa são relativas ao início do código e não mais a endereços físicos. Dessa forma, os programas poderiam ser executados  partir de qualquer partição.

Para manter o controle sobre as partições, a gerência de memória mantém uma tabela com o endereço inicial de cada partição, seu tamanho e se está em uso.

Nesse esquema de alocação de memória, a proteção baseia-se em dois registradores, que indicam os limites inferior e superior da partição onde o programa se encontra. Caso o programa tente acessa uma posição de memória fora de seus limites definidos pelos registradores, ele é interrompido por violação de acesso.

Tanto em sistemas de alocação absoluta quanto nos de alocação relocável, normalmente, a partição onde um determinado programa é carregado não é totalmente preenchida.

* Alocação particionada dinâmica

Na Alocação Particionada Estática, o problema de fragmentação de memória interna (intrapartições), era muito evidente.

Dessa forma, na Alocação Particionada Dinâmica, foi eliminado o conceito de partições de tamanho fixo. Nesse esquema, cada programa utilizaria o espaço necessário, tornando esse espaço a sua partição. Então, não há fragmentação interna.

Entretanto, um tipo diferente de fragmentação começa a ocorrer nesse caso, quando os programas forem terminando e deixando espaços cada vez menores na memória, não permitindo o ingresso de novos programas.

![[Pasted image 20230529224153.png]]

Existem duas soluções para o problema da fragmentação externa. Na primeira solução, conforme os programas terminam, apenas os espaços livres adjacentes são reunidos, produzindo áreas livres de tamanho maior.

A segunda solução envolve a relocação de todas as partições ocupadas, criando uma única área livre contígua (relocação dinâmica). Esse tipo de mecanismo é conhecido como *Alocação Particionada Dinâmica Com Relocação*. Essa implementação reduz muito o problema da fragmentação, porém, seu algoritmo e consumo de recursos do sistema pode torna-lo inviável.

![[Pasted image 20230529224418.png]]

## Estratégias de alocação de partição

 * Best-fit

Nessa estratégia a partição escolhida para alocar um programa é aquela em que sobra a menor área livre. Nesse algoritmo, a lista de áreas livras está ordenada por tamanho, diminuindo o tempo de busca por uma área desocupada. A desvantagem desse método é que o algoritmo tende a deixar cada vez mais a memória com pequenas áreas não-contíguas, aumentando  a fragmentação.

 * Worst-fi
O contra´rio da estratégia best-fit, a worst-fit busca deixar o maior espaço possível disponível pós alocação. Essa ténica deixa espaços livres maiores que permitem um maior número de programas utilizando a memória e diminuindo a fragmentação.

* First-fit
A estratégia first-fit tem como vantagem sua rapidez, no caso, o sistema procura a primeira partição livre capaz de abrigar o programa. Essa é a estratégia mais rápida e que consome menos recursos do sistema.

# Swapping

A técnica de swapping foi introduzida para contornar o problema de insuficiência de memória principal.

Então, o swapping é uma técnica aplicada à gerência de memória para programas que esperam por memória livre para serem executados. Nessa situação, o sistema escolhe um processo residente, que é transferido da memória principal para a memória secundária (swap out). Posteriormente, o processo é carregado de volta a memória principal (swap in) e pode continuar como se nada tivesse ocorrido.

O algoritmo de escolha do processo a ser retirado da memória principal deve priorizar aquele com menores chances de ser executado, para evitar o swapping desnecessário de um processo que será executado logo em seguida. Os processos retirados da memória geralmente estão no estado de espera, porém, existe uma possibilidade de ele estar no estado de pronto. No primeiro caso, o processo é dito no estado de espera outswapped e no segundo caso no estado de pronto outswapped.

Para que o swapping seja possível, é essencial que o sistema ofereça um loader que implemente a relocação dinâmica de programas, pois um programa pode sair e voltar diversas vezes para a M.P, sendo necessária a relocação a cada carregamento.

A relocação dinâmica é realizada através de um registrador especial denominado registrador de relocação. No momento em que um programa é carregado, o registrador recebe o endereço inicial da posição que o programa irá ocupar. Toda vez que ocorrer uma referência a algum endereço, o endereço contido na instrução será somado ao conteúdo do registrador, gerando, assim, o endereço físico.

Nota: a operação swapping é custosa pois o acesso a memória secundária é lento, sendo assim, quando há pouca memória disponível o sistema pode ficar quase que exclusivo a execução de swapping, impedindo a execução de processos residentes.

[[EXERCÍCIOS]]

