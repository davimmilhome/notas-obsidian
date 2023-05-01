# Sistema Operacional

Um Sistema Operacional (S.O) não é executado de maneira linear, suas rotinas são executadas concorrentemente em função de eventos assíncronos, ou seja, podem ocorrer a qualquer momento.

Resumindo, um sistema operacional têm duas funções básicas:

* Facilidade de acesso aos recursos do sistema

O S.O ajuda a gerenciar o acesso de diversos componentes de hardware que podem estar presentes na máquina, sem que o usuário se preocupe com a comunicação interna entre esses dispositivos. Além disso, o S.O serve de Interface entre o usuário e os recursos do sistema.

O S.O como interface entre usuário e recursos do sistema, permite um trabalho mais eficientes com menos chance de erro. Essa interface com o usuário abstrai a comunicação diretamente com os recrursos do sistema, sendo assim, é como se usuário estivesse em um ambiente simulado, também chamado de máquina virtual. Ou seja, para o usuário, é como se somente o S.O  existisse, esse é o conceito de ambiente virtual. 

Em geral, o fluxo de interação é o seguinte:

Usuário > Aplicações > S.O > Hardware

* Compartilhamento de recursos do sistema de forma organizada e protegida

Nos sistemas computacionais,  os usuários compartilham recursos, para isso é necessário configurar o uso concorrente desses recursos. Se imaginarmos uma impressora, deverá existir algum tipo de controle para que a impressão de um usuário não interfira nas demais impressões.

Não somente um S.O deve gerenciar as operações concorrentes para mais de um usuário, mas também gerenciar essas operações para diversas aplicações de um mesmo usuário.

Da mesma maneira que as linguagens de programação, os sistemas operacionais evoluíram no sentido de facilitar o trabalho de codificação, submissão, execução e depuração de programas. Com esse fim, os sistemas operacionais incorporaram seu próprio conjunto de rotinas para as operações de entrada e saída (Input/Output Control System- IOCS).

O IOCs eliminou a necessidade de programadores desenvolverem suas prórpias rotinas de leitura e gravação específicas para cada dispositivo. Essa facilidade de comunicação criou o conceito de independência de dispositivos, introduzidos pelos S.O.s.

No final da década de cinquenta foi introduzido o conceito de memória hierarquizada, a base para o conceito de memória virtual. Basicamente,  o sistema implementava uma paginação por demanda para transferir informações da memória secundária para a principal.

Além disso, com a evolução dos computadores, surgiu um método de processamento que visa diminuir o tempo ocioso das unidades de processamento, o método de processamento BATCH. Eventualmente, outros métodos de processamento também foram sendo implementados para melhorar a utilização dos procesadores.


# Máquina de Camadas

Uma operação efetuada pelo software pode ser implementada em hardware, enquanto uma instrução executada pelo hardware pode ser simulada via software, essa decisão fica a cargo do projetista do S.O. Sendo assim, tanto hardware como software são logicamente equivalentes.

Como citado anteriormente, a aplicação do usuário interage diretamente com o S.O, como se hardware não existisse. Essa visão abstrata também pode classificar o sistema como uma máquina de camadas. Sendo assim,  inicialmente existem dois níveis:

 - Hardware > nível 0 
 - S.O > nível 1

# Tipos de sistemas operacionais


## Sistemas monoprogramáveis

Os primeiros sistemas desenvolvidos eram voltados tipicamente para a execução de um único programa. As aplicações aguardavam o termino do programa corrente. Esses sistemas forçam com que o processador, memória e outros periféricos fiquem a serviço exclusivo da execução de um único programa.

Observe que, os recursos do computador como a memória são sub-utilizados. Nesse sentido, os sistemas monoprogramáveis são mais simples de implementação pois não há necessidade de se preocupar com problemas advindos do compartilhamento de recursos.

## Sistemas mutiprogramáveis/multitarefas

Nesse tipo de sistema os recursos computacionais são compartilhados entre os diversos usuários e aplicações. O S.O se preocupa em gerenciar o acesso concorrente aos seus diversos recursos, como memória, processador e periféricos.

Antes disso, sempre que um programa realizava uma operação, o processador ficava ocioso aguardando o termino da operação. A multiprogramação permitiu que vários programas compartilhassem a memória ao mesmo tempo e, enquanto um programa esperava por uma operação, o processador executava outro programa.

Como existe a possibilidade de compartilhamento de recursos, há uma diminuição nos custos e, além disso, há uma redução geral no tempo de execução das aplicações. **Dependendo do n° de usuários, os sistemas multiprogrmáveis podem ser classificados como monousuários ou multi-usuários.**

Os sistemas multiprogramáveis podem também podem ser classificados pela forma que suas aplicações são gerenciadas, o tipo de processamento adotado. São eles:

 - Batch
 - Tempo Compartilhado (time-sharing)
 - Tempo Real (real-time)

Um determinado S.O pode suportar um ou mais desses tipos de processamento.

### Sistemas batch

Foi o primeiro tipo de processamento adotado nos S.O multiprogramáveis, os JOBS aguardam numa fila para serem processados. O processamento batch tem  a característica de não exigir a interação do usuário com a aplicação.

Atualmente, os sistemas operacionais implementam ou simulam o processamento batch, não existindo sistemas exclusivamente dedicados a esse tipo de processamento.

### Sistemas de tempo compartilhado (time-sharing)

Nesse tipo de sistema, cada programa é executado em pequenas fatias de tempo do processador. Caso o programa não seja concluído em uma fatia de tempo, ele é interrompido pelo S.O para o processamento de outro programa, enquanto aguarda uma nova fatia de tempo. Nesse caso, o sistema cria para cada usuário e aplicação um ambiente de trabalho próprio, dando a impresão de que todo o sistema está dedicado exclusivamente a eles.

 
### Sistemas de tempo real  (real-time)

Ele é bem semelhante aos sistemas de time-sharing, entretanto, o tempo de execução de um JOB precisa estar bem definido. Ou seja, no caso de um sistema onde o método de processamento é o time-sharing, o tempo de execução do programa pode variar, porém, no método de tempo real essa variação não pode ocorrer, pois poderia causar problemas irreparáveis durante a execução execução.

Nesse caso, não existe a ideia de fatia de tempo. Um programa utiliza o tempo que for necessário a sua execução até que apareça outro mais prioritário. Geralmente, a prioridade da aplicação é definida por ela mesma ao invés do S.O.

Esses sistemas normalmente são empregados em situações que o tempo de procesamento é um fator fundamental, por exemplo, tráfego aéreo, refinarias, usinas nucleares, etc.


## Sistemas com múltiplos processadores

São sistemas onde duas ou mais UCPs trabalham em conjunto. A vantagem desse tipo de sistema é permitir que vários programas sejam executados ao mesmo tempo ou que um mesmo programa seja subdividido para ser executado em  diversos processadores. Geralmente esse tipo de sistema é utilizado para o procesamento científico.

Outras características importantes que esses sistemas proporcionam são a escalabilidade, disponibilidade e balanceamento de carga.

 - Escalabilidade
Escalabilidade é a capacidade de aumentar o poder de processamento do sistema computacional apenas adicionando novos processadores, sem a necessidade de troca do sistema completo.

* Disponiblidade
Nesse caso,a diosponibilidade se caracteriza pela fácil substituição de uma unidade de processamento por outra, caso ocorra algum problema. Suponha que durante a execução de um determinado programa um processador entre em curto, a sua aplicação não será encerrada pois existem outras unidades em funcionamento.

* Balanceamento de carga
Nesse tipo de sistema, pode haver um deterrminado programa que faça o balanceamento de carga entre as unidades de processamento, ou seja,  o processamento pode ser dividido de maneira a otimizar a execução do programa entre os processadores.

---

Dentre os sistemas com múltiprocessadores, existem dois principais: fortemente acoplados e fracamente acomplados.

### Fortemente acoplados

Existe apenas uma memória principal sendo compartilhada por todos os processadores. A taxa de transferência entre processador e memória é muito maior que nos fracamente acoplados.

Nesse tipo de sistema, os dispositivos E/S são gerenciados por apenas um S.O. Ainda sim, esses sistemas podem ser divididos em dois tipos:

* SMP (Symmetric Mutiprocessors):
Caracterizam pelo tempo de acesso a memória principal sendo uniforme.

 * NUMA  (NON-UNIFORM Memory Access):
Carcterizam-se por possuirem diversos conjutos de processadores reunídos a memória principal, sendo que cada conjunto é conectado aos outros através de uma rede  de interconexão. O tempo de acesso a memória pelos processadores varia em função de sua localidade física.

### Fracamente acomplados

Cada sistema computacional tem suas próprias memórias individualis, além disso, a taxa de transferência entre processadores e a memória em sistemas pode variar em função da distância física dos processadores com relação memória.

Nos sistemas fracamente acoplados, cada sistema funciona de forma independente, possuíndo seu próprio S.O. Em decorrência disso, os sistemas fracamente acoplados também são conhecidos como sistemas multipcomputadores.


![[Pasted image 20230225111400.png]]

Nesse tipo de configuração, os usuários utilizam terminais não inteligentes conectados a linhas seriais dedicadas, linhas telefônicas públicas ou outras linhas de conexão para realizar o link de comunicação entre os processadores. Nesse modelo, o terminal não tem capacidade de processamento, o usuário envia um pedido ao sistema, que realiza o processamento e retorna uma resposta (sistema centralizado).

Com a evolução dos computadores pessoais e das redes de comunicação, surgiu um um modelo de computação chamado de rede de computadores. Em uma rede existem dois ou mais sistemas independentes (hosts), interligados através de linhas de comunicação, que oferecem serviços aos demais. Nesse modelo, a informação deixa de ser centralizada e passa a ser distribuída.

Então, com base no grau de interligação dos hosts, podemos dividir os sistemas fracamente acoplados em sistemas operacionais de rede e sistemas distribuídos.

 * Sistemas Operacionais de Rede (SOR)
Permitem que um host compartilhe seus recursos (ex, impressora) com os demais hosts da rede. Um exemplo desse tipo de sistema são as redes locais.

 * Sistemas Distribuídos
No SORs os usuários tem conhecimento dos hosts e de seus serviços, já nos sistemas distribuídos o sistema operacional esconde detalhes dos hosts individuais e passa a tratá-los como um objeto único. Nesse caso, os sistemas operacionais distribuídos perrmitem que uma aplicação seja dividida em partes e que cada parte seja executada por hosts diferentes.

Para o cliente é como se a rede não existisse, mas sim um único sistema centralizado, um exemplo de sistemas distribúídos são os clusters. Esse modelo facilita ainda mais a disponibilidade, escalabilidade e balanceamento.


*[[EXERCÍCIOS]]

