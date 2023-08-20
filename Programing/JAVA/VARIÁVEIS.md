As variáveis em um código Java são declaradas utilizando uma estrutura formal que é a seguinte:

tipo nomeVariavel = valor;

Nota: observe a notação utilizada, camelcase começando com letra minúscula.

Exemplo: String strinQualquer = new String("HelloWorld");

Além disso, também é possível criar constantes em Java da seguinte forma:

final tipo CONST_CASE = valor;
# Tipos

Os tipos principais do Java são primitivos, classes e  Enums. Especificamente para o Java os tipos primitivos de fato são primitivos, não possuem nenhum método implementado. 

Por conta da característica citada acima no Java existem Wrapers, que são invólucros dos tipos primitivos e servem para adicionar métodos a esses tipos e, além disso, podem ser nulos (diferente de uma variável primitiva que não pode ser nula) .

Então, acontece que, quando se utiliza tipos primitivos numéricos e, tenta-se acessar esses tipos fora de escopo, o valor zero é atribuído de maneira arbitrária, por conta disso, é mais comum se utilizar o wrappers já que eles podem ser inicializados com o null.




![[Pasted image 20230812195558.png]]

nota: quando se divide dois ints automaticamente o resultado é um int e o valor real é ignorado.

Nota: caso você utilize, por exemplo, um valor como 23.4 o Java considera esse valor como double nativamente. Para especificar que se deseja trabalhar com um float é necessário utilizar a notação 23.4F

Nota: strings no java são imutáveis, caso você atribuir um novo valor a uma string, não é alterado o endereço no endereço de memória, mas sim desginado um novo endereço de memória. Por conta disso, trabalhar em um loop com strings/concatenação de strings não é adequado.

## Enums

Os enums são um tipo especial do java que servem, de forma geral, para criar listas de opções.

O enum é uma espécie de classe especial e pode ser criado da seguinte forma dentro de seu arquivo:



public enum DiasDaSemana {

SEGUNDA,

TERÇA,

QUARTA,

QUINTA,

SEXTA,

SABADO

DOMINGO,

}

Observe que os elementos dentro do enum não são strings, mas sim valores. Eles podem ser chamados dentro de sua classe main dessa forma

  

DiasDaSemana.SEGUNDA



Então, é possível construir uma função com o seguinte parâmetro

  
eDiaLetivo(DiasDaSemana dia) {

...

}

Dessa forma, essa função somente irá aceitar inputs do enum, dias da semana


Na maior parte das vezes, o enum não tem métodos, entretanto, é possível que ele tenha alguns métodos relevantes associados.

