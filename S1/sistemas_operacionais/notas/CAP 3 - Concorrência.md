Em sistemas multiprogramáveis, diferentemente dos monoprogramáveis, a sub-utilização do processador e da memória é reduzida. Isso também acontece para dispositivos de entrada e saída como impressora e disco.

## Interrupções e Excessões

Interrupções e excessões são eventos inesperados, ocasaionando um desvio no fluxo da execução de determinado programa. Elas podem ser utilizadas na sinalização por algum dispositivo de hardware ou pelo próprio programa.

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

No segundo método, utiliza-se um registrador de status que armazena o tipo de evento ocorrrido. Com essa informação, só existe uma única rotina rotina de tratamento que, ao iniciar, testa o registrador para identificar qual é o tipo de interrupção. (É como se fosse um bloco único de código).

As instruções são resultados de eventos assíncronos, ou seja, não relacionados a instrução que está sendo executada. Por conta disso, podem ocorrer multiplas interrupções simultâneamente. Uma maneira de evitar essa situação é a rotina de tratamento ignorar as demais interrupções após a primeira. Interrupções com essa característica são chamadas de interrupções mascaráveis.

Em alguns tipos de processador, as interrupções seguintes não podem ser ignoradas, dessa forma, existe a necessidade de um dispositivo chamado controlador de pedidos de interrupção que avalia as interrupções geradas e suas prioridades de atendimento.

### Exceções

Uma excessão, diferentemente das interrupções, é resultado direto da execução de instrução do próprio programa como, por exemplo, uma divisão por zero ou ocorrência de overflow.

Observe que, a exceção é gerada então por um evento síncrono (diferente da excessão), resultado direto da execução do programa. Fazendo uma associação, se um programa que causa esse tipo de eventos for executado com a mesma entrada de dados, a excessão ocorrerá sempre na mesma instrução

Dessa forma, quando uma excessão acontece, a exceução éinterrompida e o controle é desviado para uma rotina de tratamento de exceções. Normalmente, o mecanismo de tratamento é definido pelo próprio programador, evitadndo que o programa seja encerrado abruptamente.

## Operações de I/O

Nos primórdios, a comunicação entre o processador e periféricos era controlada por um grupo de instruções (instruções de I/O), executadas pelo próprio processador. Essas instruções continham detalhes específicos de cada periférico. Ou seja, havia uyma dependência entre I/O e processador.

Visando solucionar o problema, surgiu o *controloador de interface* que permitiu independência entre processador e dispositivos de I/O. Isso simplificou as instruções de I/O por não ser mais necessário especificar detalhes de operações específicos de cada periférico.

![[Pasted image 20230504054146.png]]





[[EXERCÍCIOS]]