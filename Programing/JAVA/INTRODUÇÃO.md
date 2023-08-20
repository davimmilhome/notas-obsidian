Java é uma linguagem de programação fortemente tipada. A linguagem JAVA tem um comportamento único, sendo tanto compilada quanto interpretada.

Primeiramente, um código escrito em JAVA é compilado para javabytecode, que é um tipo de código que o interpretador java, chamado de JVM (Java Virtual Machine) consegue transpilar para linguagem de máquina, assim, executando o programa.

A ideal central por trás da linguagem Java é justamente a JVM, onde o objetivo principal e lema é: "write once, run everywhere".

O Java segue o paradigma de programação orientada a objetos, onde esse paradigma visa trazer objetos como algo genérico, que podem representar algo do mundo real.

Para programar em Java é necessário ter em seu ambiente a instalçaõ do *JDK* (Java Development Kit). O JDK é composto pelos elementos:

* Java Runtime Envriomment (JRE)
O JRE se caracteriza por tudo que é necessário para executar uma aplicação Java. 
Mesmo que o usuário não desenvolva a aplicação, ele precisa de uma JRE para executar um programa Java.

*  Bibliotecas básicas e API Java;
* Ferramentas de execução e desenvolvimento

---

Um código Java passa por três estágios em seu desenvolvimento, são eles:

a escrita em si > compilação para bytecode da JVM > transpilação para linguagem de máquina.

Para visualizar esses estágios de forma simples, você pode criar uma classe Java, como por exemplo:

public class OlaMundo {

public status void main(String[] args) {

  

System.out.println("Hello word");

}

}

Salvar um arquivo exatamente com o nome da classe (OlaMundo.jar) e então compilar o para javabytecode utilizando o comando "javac path/arquivo.jar".

O arquivo .class gerado pode ser executado pela JRE chamando "java pathdorarquivo.class" 

---

## Estrutura de projetos

Em projetos Java, im pacote é nada mais que uma pasta que contém códigos javas necessários a execução;

Em geral, dentro de um projeto Java vai ter uma pasta chamada src (source), onde todo o código estará alocado. Dentro da pasta src vão haver pacotes base, que são pastas e dentro dessas pastas existem subpacotes.

O padrão de nomeclatura do pacote de um projeto é o inverso do nome do projeto que se está trabalhando. Por exemplo, se você trabalha na ada.com.br o pacote seria:

br.com.ada;

De maneira geral, dentro de um arquivo tem somente uma classe pública (o que implica que em situações específicas podem haver sublcasses mas não é indicado) e o nome do arquivo em que essa classe se localiza deve ser igual ao nome da classe.



