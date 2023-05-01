O sistema operacional é formado por um conjunto de rotinas que oferece serviços aos usuários e às suas aplicações. Esse conjunto de rotinas é denominado núcleo do sistema, também chamado de Kernel.

# Funções da kernel

Diferentemente de uma aplicação convencional, com sequenciamento de início, meio e fim, as rotinas do sistemas são executadas de maneira concorrente e sem uma ordem predefinada, com base em eventos assíncronos.

![[Pasted image 20230325201214.png]]

Nota: observe que na imagem acima o usuário interage de três formas com a kernel do sistema, por meio de utilitários, linguagem de comandos e aplicações.

De maneira geral, podemos atribuir como principais funções da kernel: 

* Tratamento de interrupções e exceções
* criação e eliminação de processos e threads
* sincronização e comunicação entre processos e threads
* escalonamento e controle dos processos e threads
* gerência de memória
* gerência de arquivos
* gerências de I/O
* suporte a redes locais e distribuídas
* contabilização do uso do sistema
* auditoria e segurança do sistema

Além disso, o S.O deve ser responsável pelo controle da utilização da CPU, de forma a impedir que algum programa monopolize o seu uso inadequadamente.

Como vários programas ocupam a memória simultaneamente, cada usuário deve possuir uma área reservada onde seus dados e códigos são armazenados. O sistema operacional implementa mecanismos de proteção, de forma a não permitir uma violação de acesso, ou seja, a tentativa de um programa acessar uma posição de memória fora de sua área.

# Modo de acesso

De maneira a garantir a segurança do sistema operacional e dos dados da máquina o S.O faz uma gerência das intruções por uma implementação chamada modo de acesso. De maneira generalista, são implementados dois módos de acesso, modo usuário e modo Kernel. No modo usuário, a aplicação somente pode executar instruções não privilegiadas (acesso reduzido a instruções), enquanto isso, no modo Kernel o usuário teria acesso a todas as intruções da máquina.

O modo de acesso é determinado por um conjunto de bits  no registrador de status do processador. Resumindo, as intruções privilegiadas só podem ser executadas quando o modo de acesso encontra-se em kernel. As instruções não-privilegiadas são as que não oferecem risco ao sistema e podem ser executadas em modo usuário.

# Rotinas do S.O e System Calls (Syscalls)

O núcleo do sistema é composto por rotinas que oferecem serviços aos usuários e aplicações, todas as funções do núcleo são implementadas em rotinas que necessariamente possuem instruções privilegiadas. A informação anterior implica que o processador deve estar necessariamente em modo kernel, o que exige a implementação de mecanismos de proteção.

Dessa forma, todo o controle de execução de rotinas do sistema operacional é realizado pelo mecanismo system call. Ou seja, quando uma aplicação deseja utilizar uma rotina do sistema operacional, o mecanismo de syscall é ativado. Inicialmente é verificado os privilégios por parte da aplicação, em caso negativo o sistema retorna ao programa chamador que a operação não é possível.

Já no caso positivo, o sistema salva o conteúdo corrente nos registradores, troca o modo de acesso para Kernel e realiza o desvio para a rotina alterando o registrador PC com o endereço da rotina chamada. Ao termino da execução da rotina de sistema, o modo de acesso volta para usuário e o contexto dos registradores é restaurado para que a aplicação continue a execução a partir da instrução que chamou a rotina do sistema.

![[Pasted image 20230325205549.png]]

Dessa forma, o mecanismo de syscalls e de proteção de hardware garantem a segurança e a integridade do sistema.

## Chamada a rotinas do S.O

As rotinas do sistema e as syscalls podem ser entendidas como uma porta de entrada para a Kernel do S.O. Sempre que uma aplicação desejar algum serviço do sistema, deve ser realizada uma chamada a uma de suas rotinas através de uma syscall.

![[Pasted image 20230326084923.png]]

A maioria dos usuários desconhece os detalhes envolvidos, por exemplo, em um simples comando de leitura a um arquivo de uma linguagem de alto nível. De foma simples, o comando da linguagem de alto nível é convertido pelo compilador para uma chamada a uma rotina específica que, quando executada, verifica a ocorrência de erros e retorna os dados ao programa de maneira transparente. As rotinas do S.O podem ser divididas por uim grupo de função.

![[Pasted image 20230326085300.png]]

Cada S.O pussi seu próprio conjunto de rotinas, com nomes, parâmetros e formas de ativção específicas. Uma tentativa de criar uma biblioteca de chamadas objetivando uma padronização foi elaborada pelos institutos ISO e IEEE, resultando em um conjunto conhecido como POSIX (Portable Operating System Interface for Unix). O POSIX estabeleceu uma biblioteca-padrão, permitindo que uma aplicação desenvolvida seguindo esse conjunto de chamadas possa ser portada para os principais sistemas. A maioria dos S.Os fornece algum suporte ao padrão POSIX.

## Linguagem de comandos

A linguagem de comandos permite ao usuário se comunicar de forma simples com o S.O.  Cada comando, depois de digitado pelo usuário é interpretado pelo shell (ou interpretador de comandos), que verifica a sintaxe, faz chamadas a rotinas do sistema e  apresenta um resultado. Em geral, o interpretador de comandos não faz parte da Kernel do sistema e isso possibilita maior flexibilidade na criação de diferentes linguagens de controle para um mesmo sistema. Por exemplo, o Unix fornece, basicamente, três interpretadores de comando: Bourne Shell, C Shell e Korn Shell.

Na maioria dos S.Os as linguagens de comando evoluiram de forma a permitir uma interação mais amigável com os usuários, utilizando interfaces gráficas como janelas e ícones. Na maioria dos casos, a interface gráfica é apenas mais um nível de abstração entre o usuário e os serviços do sistema.

![[Pasted image 20230326090116.png]]

As linguagem de comando podem oferecer a possibilidade de criar programas com estruturas de decisão e iteração. Esses programas nada mais são que uma sequência de comandos armazenados em um arquivo texto, denominados arquivos batch ou shell scripts.

## Ativação e desativação do sistema

Os componentes do S.O devem ser caregados para a memória principal toda vez que o computador é ligado, isso é feito por intermédio de um proceso denominado boot.

O processo se inicia com a execução de um programa chamado boot loader, que fica em um endereço fixo na memória ROM da máquina. Esse programa chama a execução de outro programa, o POST (Power-On Self Test), que identifica possíveis problemas de hardware. Após isso, o procedimento de ativação verifica se há no sistema computacional algum sistema operacional residente, em caso negativo uma mensagem de erro é apresentada. Se um dispositivo com S.O for encontrado, um conjunto de instruções é carregado para a memória principal. Além da carga, a ativação do sistema também consiste na execução de arquivos de inicialização onde são especificados procedimentos de customização e configuração de hardware.

Na maioria dos sistemas também existe o processo de desativação ou shutdown. Este procedimento permite que as aplicações e componentes do S.O sejam desativados ordenadamente, garantindo sua integridade.

# Arquiteturas do Núcleo

Nos sistemas operacionais atuais, grande parte do código é escrito em linguagem C/C++.

Além de facilitar o desenvolvimento, a utilização de linguagens de alto nível permite uma maior portabilidade. Uma desvantagem do uso desse tipo de linguagem em relação a programação em assembly é a perca de desempenho, por conta disso, estrutura criticas do sistema como device drivers, escalonador e rotinas de interrupção ainda são desenvolvidas em assembly.

A estrutura da Kernel de um sistema operacional e o inter-relacionamento entre seus diversos componentes pode variar conforme a concepção e objetivo do projeto. A seguir, seguem as principais arquiteturas: monolítica, arquitetura de camadas, máquina virtual e arquitetura microkernel.

## Arquitetura monolítica

A arquitetura monolítica pode ser comparada com uma aplicação formada por vários módulos que são compilados separadamente e depois linkados, formando um único grande executável.

Os primeiros S.Os foram desenvolvidos com base nesse modelo, porém, isso torna a manutenção e desenvolvimento em si bastante difíceis. Devido a sua simplicidade e bom desempenho, a estrutura monolítica foi adotada no projeto MS-DOS e nos primeiros Unix.

![[Pasted image 20230326091706.png]]

## Arquitetura de camadas

Com o aumento da complexidade e tamanho do código dos S.Os tecnicas de programação estruturada e modular foram incoporadas ao seu projeto. Na arquitetura de camadas o sistema é dividido em níveis sobrepostos. Cada camada ofere um conjunto de funções que podem ser utilizadas exclusivamente pelas camandas superiories.

A vantagem da arquitetura é camadas é isolar as funções do S.O, facilitando sua manutenção e depuração, além de criar uma hierarquia de níveis de modo de acesso, protegendo mais as camadas internas. Cada nova camada implica uma mudança no modo de aceso. 

Atualmente, a maioria dops sistemas comerciais utiliza o modelo de duas camadas, onde existem os modos de acesso usuário e Kernel. A maioria das versões Unix e Windows são baseadas nesse modelo.

![[Pasted image 20230326092101.png]]


## Máquina virtual

Um sistema computacional é formado por níveis, onde a camada de mais baixo nível é o hardware. O modelo de máquina virtual (ou VM) cria um nível intermediário entre o hardware e o S.O, denominado gerência de máquinas virtuais. Esse nível cria diversas máquinas virtuais independentes, onde cada uma oferece uma cópia virtual do hardware, incluindo modos de acesso, interrupções, dispositivos de I/O etc.

Como cada máquina virtual é independente das demais, é possível que cada VM tenha seu próprio sistema operacional e que seus usuários executem suas aplicações como se todo o computador estivesse dedicado a cada um deles.

Além de permitir a convivência de sistemas operacionais diferentes no mesmo computador, esse modelo cria isolamento total entre cada VM, oferecendo grande segurança para cada máquina individualmente, não sendo afetadas em problemas em outras VMs. A desvantagem dessa arquitetura é sua grande complexidade, devido a necessidade de se compartilhar e gerenciar os recursos de hardware entre as diversas VMs.

![[Pasted image 20230326092639.png]]

## Arquitetrura Microkernel

Uma tendência nos sistemas operacionais modernos é tornar o núcleo do sistema operacional menor e o mais simples possível. Para implementar essa ideia, os serviços do sistema são disponibilizados através de processos onde cada um é responsável por oferecer um conjunto específico de funções, como gerência de arquivos, gerência de memória  e escalonamento.

Sempre que uma aplicação deseja algum serviço, é realizada uma solicitação ao processo responsável. Neste caso, a aplicação que solicita o serviço é chamada de cliente, enquanto o processo que responde a solicitação é chamado de servidor. A principal função do núcleo nessa arquitetura é realizar a comunicação,ou seja, a troca de mensagens entre cliente e servidor.

Esse tipo de arquitetura tem sua vantagem na facilidade de adicionar novos ervidores a medida que o número de clientes aumenta, conferindo uma grande escalibiladade ao S.O. Porém, apesar de suas vantagens, a implementação deste tipo de modelo na prática é muito difícil. Primeiramente existe o problema de desempenho devido a necessidade de mudança de modo de acesso a cada comunicação entre cliente e servidor. Outro problema é que certas funções do S.O exigem acesso direto ao hardware, como operações de E/S.

[[EXERCÍCIOS]]