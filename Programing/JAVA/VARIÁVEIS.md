As variáveis em um código Java são declaradas utilizando uma estrutura formal que é a seguinte:

tipo nomeVariavel = valor;

Nota: observe a notação utilizada, camelcase começando com letra minúscula.

Exemplo: String strinQualquer = new String("HelloWorld");

Além disso, também é possível criar constantes em Java da seguinte forma:

final tipo CONST_CASE = valor;
## Tipos

Os tipos em Java se caracterizam em tipos primitivos e classes. Especificamente para o Java os tipos primitivos de fato são primitivos, não possuem nenhum método implementado. 

Por conta da característica citada acima no Java existem Wrapers, que são invólucros dos tipos primitivos e servem para adicionar métodos a esses tipos e, além disso, podem ser nulos (diferente de uma variável primitiva que não pode ser nula) .


![[Pasted image 20230812195558.png]]

Nota: caso você utilize, por exemplo, um valor como 23.4 o Java considera esse valor como double nativamente. Para especificar que se deseja trabalhar com um float é necessário utilizar a notação 23.4F