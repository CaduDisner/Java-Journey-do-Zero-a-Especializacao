# First Steps — Primeiros Passos com Java

> **Módulo:** 00-Setup  
> **Etapa:** 03 — First Steps  
> **Sistema operacional:** Windows 10  
> **Java:** JDK 25 LTS  
> **IDE:** IntelliJ IDEA  
> **Versionamento:** Git e GitHub

---

## 1. Objetivo

Chegou o momento de escrever e executar nosso primeiro programa Java.

Até aqui, instalamos as ferramentas e configuramos o ambiente. Agora vamos compreender como o código-fonte é criado, compilado e executado.

Ao finalizar esta etapa, você deverá ser capaz de:

- Criar um arquivo `.java`;
- Entender a estrutura básica de uma classe;
- Escrever um método `main`;
- Utilizar `System.out.println()`;
- Compilar um programa utilizando `javac`;
- Executar uma classe utilizando `java`;
- Criar um projeto Java no IntelliJ IDEA;
- Executar e depurar um programa;
- Criar o primeiro commit;
- Enviar o projeto ao GitHub.

**Importante:** não queremos apenas copiar comandos. Queremos entender o que cada comando faz e por que ele é necessário.

---

## 2. Antes de começar: como Java funciona?

Java utiliza um processo de compilação e execução.

Diferentemente de algumas linguagens que são executadas diretamente a partir do código-fonte, o Java tradicionalmente transforma o código-fonte em bytecode antes da execução.

O fluxo simplificado é:

```text
         Código-fonte
            .java
              |
              v
        Compilador javac
              |
              v
           Bytecode
            .class
              |
              v
       Java Virtual Machine
             JVM
              |
              v
        Programa executado
```

### 2.1. Código-fonte

É o código escrito pelo desenvolvedor.

Exemplo:

```java
System.out.println("Hello, World!");
```

O código-fonte é armazenado em arquivos com extensão `.java`.

### 2.2. Compilação

O compilador `javac` analisa o código-fonte e, quando não encontra erros de compilação, gera arquivos `.class` contendo bytecode.

### 2.3. Bytecode

É uma representação intermediária do programa.

Ele não é o código-fonte original e também não corresponde diretamente ao código de máquina específico de um processador.

### 2.4. Execução

A JVM carrega e executa o bytecode.

Durante a execução, a JVM pode interpretar instruções e utilizar compilação JIT (*Just-In-Time*) para transformar partes do programa em código nativo, conforme necessário.

Essa arquitetura é uma das razões pelas quais Java consegue oferecer portabilidade entre diferentes plataformas que possuem uma JVM compatível.

---

## 3. Primeiro programa pelo terminal

Antes de utilizar o IntelliJ IDEA, vamos executar um programa manualmente.

Isso é importante porque a IDE automatiza várias tarefas que precisamos compreender.

### 3.1. Criar o diretório do primeiro programa

Abra o PowerShell.

Execute:

```powershell
cd C:\Desenvolvimento\Java\Estudos
```

Crie uma pasta:

```powershell
New-Item -ItemType Directory -Name FirstJavaProgram
```

Entre nela:

```powershell
cd FirstJavaProgram
```

### 3.2. Criar o arquivo Java

Crie um arquivo chamado:

```text
HelloWorld.java
```

Você pode criá-lo pelo editor de sua preferência ou executar:

```powershell
New-Item -ItemType File -Name HelloWorld.java
```

Abra o arquivo e escreva:

```\\java
public class HelloWorld {

    public static void main(String[] args) {

        System.out.println("Hello, World!");

    }

}
```

Salve o arquivo.

---

## 4. Entendendo o código

Vamos analisar cada parte.

### 4.1. Declaração da classe

```java
public class HelloWorld {
```

Essa linha declara uma classe chamada `HelloWorld`.

Em Java, classes são estruturas fundamentais para organizar código e representar comportamentos e dados.

A palavra `public` é um modificador de acesso.

A palavra `class` indica que estamos declarando uma classe.

`HelloWorld` é o nome escolhido.

Como a classe é pública, o nome do arquivo-fonte precisa corresponder ao nome da classe pública:

```text
HelloWorld.java
```

Java diferencia letras maiúsculas e minúsculas.

Portanto:

```text
HelloWorld
```

é diferente de:

```text
helloworld
```

### 4.2. Método main

```\\java
public static void main(String[] args) {
```

Esse é o método de entrada tradicional utilizado pela JVM para iniciar uma aplicação Java.

Vamos entender cada elemento:

| Elemento | Significado |
|---|---|
| `public` | Modificador de acesso |
| `static` | Permite invocar o método sem criar uma instância da classe |
| `void` | Indica que o método não retorna um valor |
| `main` | Nome do método de entrada tradicional |
| `String[] args` | Array de strings utilizado para receber argumentos |

Não precisamos dominar todos esses conceitos agora.

Eles serão estudados com profundidade nos módulos de fundamentos e orientação a objetos.

Neste momento, precisamos reconhecer a estrutura e compreender sua finalidade.

### 4.3. Exibição de mensagens

```java
System.out.println("Hello, World!");
```

Essa instrução exibe uma mensagem no console.

Vamos separar:

- `System`: classe disponibilizada pela biblioteca padrão Java;
- `out`: fluxo de saída padrão;
- `println`: método que escreve uma linha e adiciona uma quebra de linha;
- `"Hello, World!"`: texto que será exibido.

O ponto e vírgula encerra a instrução.

### 4.4. Chaves

```java
{
}
```

As chaves delimitam blocos de código.

No exemplo, temos:

- Um bloco para a classe;
- Um bloco para o método `main`.

A organização por blocos será muito importante quando estudarmos métodos, condições, loops e classes.

---

## 5. Compilando o programa

Agora vamos utilizar o compilador Java.

No PowerShell, dentro da pasta em que está o arquivo, execute:

```powershell
javac HelloWorld.java
```

Se não houver erros de compilação, será criado um arquivo:

```text
HelloWorld.class
```

Nossa pasta ficará assim:

```text
FirstJavaProgram/
│
├── HelloWorld.java
└── HelloWorld.class
```

### 5.1. O que aconteceu?

O comando:

```powershell
javac HelloWorld.java
```

solicitou ao compilador que analisasse o arquivo-fonte e gerasse o bytecode correspondente.

O compilador verifica diversos aspectos, incluindo:

- Sintaxe;
- Estrutura do código;
- Tipos;
- Referências;
- Regras de compilação da linguagem.

Se houver um erro, o compilador apresentará uma mensagem indicando o problema.

### 5.2. Executando o programa

Agora execute:

```powershell
java HelloWorld
```

Resultado esperado:

```text
Hello, World!
```

Observe que não precisamos escrever:

```powershell
java HelloWorld.class
```

O comando `java` recebe o nome da classe principal, não o nome do arquivo `.class`.

---

## 6. Entendendo o classpath

O classpath é o caminho utilizado pela plataforma Java para localizar classes e recursos necessários à aplicação.

No nosso exemplo, o arquivo `.class` está no diretório atual.

Por isso, o comando funciona:

```powershell
java HelloWorld
```

Podemos indicar explicitamente o classpath:

```powershell
java -cp . HelloWorld
```

O parâmetro `-cp` significa classpath.

O ponto (`.`) representa o diretório atual.

Esse conceito será importante quando começarmos a trabalhar com múltiplas classes, pacotes e dependências.

---

## 7. Criando o primeiro projeto no IntelliJ IDEA

Agora vamos repetir o processo utilizando nossa IDE.

### 7.1. Criar um projeto

Abra o IntelliJ IDEA.

Na tela inicial, selecione:

```text
New Project
```

Configure:

| Campo | Configuração |
|---|---|
| Name | FirstJavaProject |
| Location | Diretório de estudos |
| Language | Java |
| Build system | IntelliJ |
| JDK | JDK 25 |
| Add sample code | Desativado, se desejarmos criar tudo manualmente |

O diretório pode ser:

```text
C:\Desenvolvimento\Java\Estudos\FirstJavaProject
```

Clique em Create.

### 7.2. Reconhecendo a estrutura

A IDE poderá apresentar uma estrutura semelhante a:

```text
FirstJavaProject/
│
├── .idea/
│
├── src/
│
└── FirstJavaProject.iml
```

Os elementos podem variar conforme as opções e a versão da IDE.

- `.idea`: configurações do projeto no IntelliJ;
- `src`: diretório de código-fonte;
- `.iml`: arquivo de configuração do módulo.

### 7.3. Criar a classe

Clique com o botão direito sobre `src`.

Selecione:

```text
New > Java Class
```

Nome:

```text
HelloWorld
```

O IntelliJ criará a classe.

Escreva:

```\\\java
public class HelloWorld {

    public static void main(String[] args) {

        System.out.println("Hello, World!");

    }

}
```

### 7.4. Executar pela IDE

Ao lado do método `main`, o IntelliJ normalmente apresenta um ícone de execução.

Clique nele e selecione Run.

A IDE compilará e executará o programa.

O resultado será apresentado na janela Run.

### 7.5. Comparando terminal e IDE

| Terminal | IntelliJ IDEA |
|---|---|
| Criamos arquivos manualmente | A IDE auxilia na criação |
| Executamos `javac` | A IDE aciona o processo de compilação |
| Executamos `java` | A IDE inicia a aplicação |
| Consultamos erros no terminal | A IDE apresenta inspeções e mensagens |
| Configuramos comandos manualmente | A IDE organiza configurações de execução |

**Lembre-se:** o IntelliJ facilita o trabalho, mas não substitui o conhecimento sobre o funcionamento do Java.

---

## 8. Primeiro contato com o debugger

O debugger permite acompanhar a execução do programa.

Ele é uma ferramenta fundamental para entender o comportamento do código e investigar erros.

### 8.1. O que é breakpoint?

Um breakpoint é um ponto de interrupção.

Quando a execução chega a esse ponto, o debugger pausa o programa.

### 8.2. Criar um breakpoint

No IntelliJ:

1. Abra `HelloWorld.java`.
2. Clique na margem esquerda da linha:

```java
System.out.println("Hello, World!");
```

3. Um indicador aparecerá na linha.
4. Clique com o botão direito no método `main` ou utilize a opção Debug.

A execução deverá parar no breakpoint.

### 8.3. Controles básicos

| Controle | Função |
|---|---|
| Step Over | Executa a próxima linha sem entrar em métodos |
| Step Into | Entra no método chamado |
| Step Out | Sai do método atual |
| Resume Program | Continua a execução |
| Stop | Encerra a execução |

O debugger será aprofundado nos módulos posteriores.

---

## 9. Segundo exercício: personalizando a saída

Vamos modificar o primeiro programa.

```\\java
public class HelloWorld {

    public static void main(String[] args) {

        System.out.println("================================");
        System.out.println("        JAVA JOURNEY");
        System.out.println("================================");

        System.out.println("Minha primeira aplicação Java.");
        System.out.println("JDK: 25");
        System.out.println("Etapa: First Steps");

        System.out.println("================================");

    }

}
```

Compile e execute novamente.

Observe que o programa executa as instruções sequencialmente.

### Desafio

Modifique o programa para exibir:

1. O nome da jornada;
2. Seu objetivo com Java;
3. A versão do JDK;
4. O nome da etapa atual;
5. Uma mensagem de conclusão.

Não copie simplesmente o exemplo. Escreva sua própria versão.

---

## 10. Terceiro exercício: criando uma segunda classe

Agora vamos observar que um projeto Java pode conter mais de uma classe.

Crie uma classe chamada:

```text
JourneyInformation
```

Código:

```\\java
public class JourneyInformation {

    public static void main(String[] args) {

        System.out.println("Java Journey");
        System.out.println("Fundamentos em construção.");

    }

}
```

Agora temos duas classes com métodos `main`:

```text
src/
│
├── HelloWorld.java
└── JourneyInformation.java
```

Cada uma pode ser executada como uma aplicação independente, desde que sua estrutura esteja correta.

Esse exercício é uma introdução ao conceito de múltiplas classes.

Ainda não estamos estudando comunicação entre objetos, pacotes ou orientação a objetos em profundidade.

---

## 11. Primeiro contato com Git

Vamos versionar nosso projeto.

Abra o terminal na raiz do repositório `Java-Journey`.

### 11.1. Verificar o estado

```bash
git status
```

O comando apresenta os arquivos modificados, adicionados ou ainda não rastreados.

### 11.2. Adicionar arquivos

```bash
git add .
```

Esse comando adiciona ao staging os arquivos que não forem ignorados pelas regras do Git.

O staging é a área de preparação do próximo commit.

### 11.3. Conferir o staging

```bash
git status
```

### 11.4. Criar o primeiro commit

```bash
git commit -m "docs: add initial Java Journey setup"
```

O commit registra um conjunto de alterações no histórico do repositório.

Uma boa mensagem de commit deve indicar de maneira objetiva o que foi realizado.

### 11.5. Conferir o histórico

```bash
git log --oneline
```

O comando apresenta o histórico resumido dos commits.

---

## 12. Enviando o projeto para o GitHub

Se o repositório remoto já estiver configurado, podemos enviar nossos commits.

Verifique:

```bash
git remote -v
```

Confirme que o endereço está correto.

Se a branch local ainda não estiver nomeada como `main`:

```bash
git branch -M main
```

Envie os arquivos:

```bash
git push -u origin main
```

O parâmetro `-u` configura a relação de acompanhamento entre a branch local e a branch remota.

Nas próximas atualizações, normalmente poderemos utilizar:

```bash
git push
```

### 12.1. Fluxo básico de versionamento

Nosso fluxo inicial será:

```text
Modificar arquivos
       |
       v
   git status
       |
       v
    git add
       |
       v
   git commit
       |
       v
    git push
       |
       v
GitHub atualizado
```

Não precisamos enviar todos os arquivos gerados pela IDE.

Antes de adicionar tudo, verifique se o `.gitignore` está configurado corretamente e revise as alterações.

---

## 13. Conceitos importantes desta etapa

| Conceito | O que aprendemos |
|---|---|
| Código-fonte | Arquivo `.java` |
| Classe | Estrutura fundamental do Java |
| Método main | Ponto de entrada tradicional da aplicação |
| Compilador | `javac` |
| Bytecode | Arquivo `.class` |
| JVM | Ambiente de execução do bytecode |
| Terminal | Execução manual dos comandos |
| IDE | Ambiente integrado de desenvolvimento |
| Debugger | Ferramenta de inspeção da execução |
| Git | Controle de versão |
| Commit | Registro de alterações |
| Push | Envio de commits ao repositório remoto |

---

## 14. Checklist de conclusão

### Java

- [ ] Criei o arquivo `HelloWorld.java`.
- [ ] Entendi a estrutura básica da classe.
- [ ] Identifiquei o método `main`.
- [ ] Entendi a finalidade de `System.out.println()`.
- [ ] Compilei o programa utilizando `javac`.
- [ ] Identifiquei o arquivo `.class`.
- [ ] Executei a classe utilizando `java`.
- [ ] Compreendi a finalidade básica do classpath.

### IntelliJ IDEA

- [ ] Criei meu primeiro projeto.
- [ ] Configurei o JDK 25.
- [ ] Criei uma classe Java.
- [ ] Executei o programa pela IDE.
- [ ] Identifiquei a janela Run.
- [ ] Criei um breakpoint.
- [ ] Iniciei uma execução em modo Debug.

### Git e GitHub

- [ ] Verifiquei o estado do repositório.
- [ ] Adicionei arquivos ao staging.
- [ ] Criei um commit.
- [ ] Consultei o histórico.
- [ ] Enviei as alterações ao GitHub.
- [ ] Conferi o repositório remoto.

---

## 15. Critério de conclusão

Não considere esta etapa concluída apenas porque conseguiu visualizar a mensagem `Hello, World!`.

Você deverá conseguir explicar, com suas próprias palavras:

1. Qual é a diferença entre código-fonte e bytecode?
2. Qual é a responsabilidade do `javac`?
3. Qual é a responsabilidade da JVM?
4. Por que o arquivo se chama `HelloWorld.java`?
5. Qual é a finalidade do método `main`?
6. Qual é a diferença entre compilar e executar?
7. O que o IntelliJ IDEA automatiza?
8. Qual é a diferença entre Git e GitHub?
9. O que acontece entre `git add` e `git commit`?
10. Por que precisamos revisar os arquivos antes de enviá-los ao GitHub?

Se conseguir responder e reproduzir o exercício sem depender integralmente das instruções, você terá consolidado o primeiro contato prático com Java.

---

## 16. Fontes oficiais

- [Dev.java — Getting Started](https://dev.java/learn/)
- [Java 25 Documentation](https://docs.oracle.com/en/java/javase/25/)
- [Java Compiler — javac](https://docs.oracle.com/en/java/javase/25/docs/specs/man/javac.html)
- [Java Launcher — java](https://docs.oracle.com/en/java/javase/25/docs/specs/man/java.html)
- [IntelliJ IDEA Documentation](https://www.jetbrains.com/help/idea/)
- [Git Documentation](https://git-scm.com/doc)
- [GitHub Docs — Git Basics](https://docs.github.com/en/get-started/git-basics)

---

**Fim da etapa 03 — First Steps.**