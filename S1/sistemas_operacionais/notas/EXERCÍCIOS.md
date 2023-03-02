 # [[CAP 1 - VISÃO GERAL]]

1° Como seria utilizar um computador sem um S.O? Quais são as principais funções de um S.O?.

	Ao utilizar um computador sem um S.O o usuário teria que se preocupar com a interação entre os dispositivos de entrada e saída, sendo necessário muito conhecimento de hardware e se tornando uma operação sucetível a erros.
	As principais funções de um sistema operacional são servir de interface entre o usuário e o hardware, ou seja, abstrair, de maneira geral, a necessidade do usuário interagir e configurar seu hardware e, além disso, é o responsável pelo gerenciamento de recursos. Ou seja, suponha que você tenha uma impressora, é necessário que algum programa gerencie a fila de impressão para que a impressão de um determinado usuário não impacte a de outro, o S.O atua nesse gerenciamento não somente para usuários diferentes mas também para um mesmo usuário com diferentes aplicações.

2° Quais as principais dificuldades  que um programador teria no desenvolvimento de uma aplicação sem um S.O?

	Esse usuário teria que ter um conhecimento muito elevado de hardware e saber configurar suas diversas interações, além disso, o usuário estaria muito sucetível a erros. Ademais, o programa desenvolvido seria dependete do tipo de hardware utilizado, ou seja, cada programa teria que ser desenvolvido especificamente para um hardware.

3° Eplique o conceito de máquina virtual. Qual a vantagem de utilizar esse conceito?

	O S.O abstrai a camada de interação entre o usuário e o hardware, ou seja, o modelo de interação passa a ser:
	usuário > aplicação > S.O > Hardware
	Nesse sentido, o usuário somente precisa se preocupar com a camada do S.O, não precisando dar enfoque ao hardware, isso  leva a vantagem de independência do hardware.

4° Defina o conceito de máquina de camadas.

	Basicamente, como S.O abstrai a necessidade de comunicação com o hardware, as interações passam a ser dividas em camadas. Ou seja, como se fossem níveis de interação, seguindo a seguinte linha de raciocínio:
	sistema operacional (camada 1)  > hardware (camada 0)

5° Quais os tipos de sistemas operacionais existentes?

	Existem os sistemas monoprogramáveis, nos quais somente é possível executar um programa por vez, ou seja, a execução de um determinado JOB toma para si todos os recursos da máquina.
	Além disso, existemos sistemas multiprogramáveis que podem ser monousuários ou multiusuários. Esse tipo de sistema pode empregar uma ou mais estratégias de processamento, como o processamento BATCH, time-sharing e real-time.
	Por fim, existem os sistemas muti-processadores,que podem ser fortemente acomplados ou fracamente acoplados.

6° Por que dizemos que existe uma subutilização de recursos em sistemas monoprogramáveis?

	Porque os recursos da máquina ficam "travados" até o final da execução de um determinado programa.
	
7° Qual a diferença entre sistemas monoprogramáveis e sistemas multiprogramáveis?

	A capacidade de executar mais de uma tarefa ao mesmo tempo, e de suportar mais de um usuário.

8° Quais as vantagens dos sistemas multiprogramáveis?

	Em geral, barateamento de custos pois os recursos podem ser compartilhados por diferentes programas e usuários, além disso, diminuição no tempo geral de execução de programas.


9° Um sistema monousuário pode ser multiporgramável? Dê um exemplo.

	Sim, por exemplo um sistema operacional de uma geladeira que pode executar programas de temperatura interna e regulação de consumo de energia ao mesmo tempo.

10° Quais são os tipos de sistemas multiprogramáveis?

	Eles podem ser monousuários ou multiusuários,  podendo adotar os tipos de processamento batch, time-sharing ou real time.

11° O que caracteriza o processamento batch? Quais aplicações podem ser processadas nesse tipo de ambiente?

	O processamento batch é caracterizado por criar uma espécie de fila de processamento, basicamente, os programas ficam em fila de execução e passam a utilizar o recurso do processador assim que este é desocupado por outro programa

12° Como funcionam os sistemas de time-sharing? Quais as suas vantagens?

	No caso do time-sharing, cada programa recebe uma pequena fatia de tempo para a sua execução, sendo assim, mais de um programa pode ser executado "ao mesmo tempo". Nesse caso, mesmo que o recurso esteja sendo dividido, do ponto de vista da aplicação é como se  todo o recurso estivesse disponível exclusivamente para ela.

13° Qual a grande diferença entre sistemas de tempo compartilhado e de tempo real? Quais aplicações são indicadas para um sistema de tempo real?

	A grande diferença entre esses dois é o tempo de execução dos programas, no caso do real-time entende-se que o tempo de execução de um determinado programa não pode variar pois isso causaria erros na sua execução. Sendo assim, os programas são executados por ordem de prioridade, só deixando de ser executados quando um programa de prioridade mais alta entra na pilha. Dessa forma os recursos do sistema ficam exclusivos para um determinado programa.

	Aplicações recomendadas para real-time são as que utilizam grande processamento de dados e têm criticidade, como por exemplo, aplicações de usinas nucleares, tráfego àreo, etc.


14° O que são sistemas com múltiplos processadores e quais as vantagens em utilizá-los?

	São sistemas onde duas ou mais UCPs trabalham em conjunto. A vantagem desse tipo de sistema é permitir que vários programas sejam executados ao mesmo tempo ou que um mesmo programa seja subdividido para ser executado em  diversos processadores. Geralmente esse tipo de sistema é utilizado para o procesamento científico.

	Outras características importantes que esses sistemas proporcionam são a escalabilidade, disponibilidade e balanceamento de carga.

15° Qual a diferença entre sistemas fortemente acoplados e fracamente acoplados?

	Os sistemas fortemente acoplados dividem uma única memória principal e um único S.O entre as unidades de processamento. Já nos sistemas fracamente acoplados, cada conjunto de processador tem sua memória e s.o individual.

16° O que é um sistema SMP? Qual a diferença para um sistema assimétrico?

	O sistemas SMP são uma variante dos sistemas operacionais com multiplos processadores, fortemente acomplados. Nesse caso, caracterizam-se pelo tempo de acesso a memória principal ser uniforme.

	Já os sistemas assimétricos (NUMA) Carcterizam-se por possuirem diversos conjutos de processadores reunídos a memória principal, sendo que cada conjunto é conectado aos outros através de uma rede  de interconexão. O tempo de acesso a memória pelos processadores varia em função de sua localidade física.

17° O que é um sistema fracamente acoplado? Qual a diferença entre sistemas operacionais de rede e sistemas operacionais distribuídos?

	Nos sistemas fracamente acomplados, cada conjunto de processador tem sua memória principal e S.O individuais. Eles são subdvididos em dois tipos, SOR e S.O distribuidos.

	O SOR permite que um host compartilhe seus recursos (ex, impressora) com os demais hosts da rede. Um exemplo desse tipo de sistema são as redes locais.

	No SORs os usuários tem conhecimento dos hosts e de seus serviços, já nos sistemas distribuídos o sistema operacional esconde detalhes dos hosts individuais e passa a tratá-los como um objeto único. Nesse caso, os sistemas operacionais distribuídos perrmitem que uma aplicação seja dividida em partes e que cada parte seja executada por hosts diferentes. Para o cliente é como se a rede não existisse, mas sim um único sistema centralizado, um exemplo de sistemas distribúídos são os clusters. Esse modelo facilita ainda mais a disponibilidade, escalabilidade e balanceamento.

18° Quais os benefícios de um sistemas com múltiplos processadores em um computador pessoal?

	Por permitir que vários programas sejam executados ao mesmo tempo ou que um mesmo programa seja subdividido para ser executado em  diversos processadores, esses sistemas trazem como vantagem a escalabilidade, disponibilidade e balanceamento de carga.
	
19° Qual seria o tipo de sistema operacional recomendável para uso como servidor de aplicação em um ambiente corporativo?

	Nesse caso, acredito que seria adequado um sistema de multiprocessadores fracamente acomplados. Sendo este um sistema operacional de rede (SOR).

20° Qual seria o tipo de sistema operacional recomendado para executar uma aplicação que manipula um grande volume de dados e necessita de um baixo tempo de processamento?

	Nesse caso, poderiam ser sistemas multiprogramáveis ou sistemas com multiplos processadores. Como a necessidade de velocidade na execução, sistemas de multiplos processadores seria mais indicador por trazer escalabilidade, disponibilidade e balanceamento de carga. Nesse sentido, poderia ser um SMP que tem maior taxa de velocidade de acesso a memória com a estratégia de processamento de real-time.