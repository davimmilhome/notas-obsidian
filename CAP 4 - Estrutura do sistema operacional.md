O sistema operacional é formado por um conjunto de rotinas que oferece serviços aos usuários e às suas aplicações. Esse conjunto de rotinas é denominado núcleo do sistema, também chamado de Kernel.

# Funções da kernel

Diferentemente de uma aplicação convencional, com sequenciamento de início, meio e fim, as rotinas do sistemas são executadas de maneira concorrente e sem uma ordem predefinada, com base em eventos assíncronos.

![[Pasted image 20230325201214.png]]

Nota: observe que na imagem acima o usuário interage de três formas com a kernel do sistema, por meio de utilitários, linguagem de comandos e aplicações.

De maneira geral, podemos atribuir como principais funções da kernel: 

* Tratamento de interrupções e excessões
* criação e eliminação de processos e threads
* sincronização e comunicação entre processos e threads
* escalonamento e controle dos processos e threads
* gerencia de memória
* gerência de arquivos
* gerências de I/O
* suporte a redes locais e distribuidas
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




[[EXERCÍCIOS]]