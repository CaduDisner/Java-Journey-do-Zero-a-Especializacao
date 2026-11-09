# Troubleshooting — Diagnóstico e Solução de Problemas

> **Módulo:** 00-Setup  
> **Etapa:** 04 — Troubleshooting  
> **Sistema operacional:** Windows 10  
> **Java:** JDK 25 LTS  
> **IDE:** IntelliJ IDEA  
> **Versionamento:** Git e GitHub

---

## 1. Objetivo

Este documento reúne problemas que podem ocorrer durante a instalação, configuração e utilização inicial do ambiente Java.

O objetivo não é apenas apresentar soluções prontas, mas desenvolver uma habilidade essencial para qualquer profissional de tecnologia: diagnosticar problemas de maneira estruturada.

Ao encontrar um erro, devemos:

1. Identificar o comportamento apresentado;
2. Reproduzir o problema;
3. Interpretar a mensagem de erro;
4. Investigar a origem;
5. Aplicar uma correção controlada;
6. Validar se o problema foi resolvido;
7. Registrar a solução quando necessário.

**Princípio fundamental:** não altere diversas configurações simultaneamente sem compreender o que está modificando.

---

## 2. Metodologia de diagnóstico

Antes de consultar um problema específico, siga esta sequência.

### 2.1. Identificar a camada do problema

Um erro Java pode ter diferentes origens.

| Camada | Exemplos |
|---|---|
| Sistema operacional | Permissão, arquitetura, diretório |
| Instalação | JDK incompleto ou ausente |
| Variáveis de ambiente | JAVA_HOME e PATH |
| Compilação | Erro de sintaxe ou referência |
| Execução | Classe não encontrada ou exceção |
| IDE | SDK incorreto ou configuração inválida |
| Git | Repositório, branch ou autenticação |
| GitHub | Permissão, chave SSH ou remote |

### 2.2. Coletar evidências

Antes de alterar qualquer configuração, execute comandos que ajudem a identificar o estado atual.

Java:

```powershell
java -version
javac -version
```

Localização dos executáveis:

```powershell
where.exe java
where.exe javac
```

Variáveis:

```powershell
$env:JAVA_HOME
$env:Path
```

Git:

```powershell
git --version
git status
git config --global --list
```

SSH:

```powershell
ssh -T git@github.com
```

Registre as mensagens apresentadas.

---

# PARTE I — PROBLEMAS DE INSTALAÇÃO E CONFIGURAÇÃO JAVA

## 3. Erro: 'java' não é reconhecido como comando

### Sintoma

Ao executar:

```powershell
java -version
```

O PowerShell apresenta uma mensagem semelhante a:

```text
java : O termo 'java' não é reconhecido como nome de cmdlet,
função, arquivo de script ou programa operável.
```

### Causas possíveis

- O JDK não está instalado;
- O diretório `bin` não está no PATH;
- O PATH foi configurado incorretamente;
- O terminal foi aberto antes da alteração das variáveis;
- O executável está em outro diretório;
- Existe conflito entre instalações Java.

### Diagnóstico

Primeiro, verifique:

```powershell
$env:JAVA_HOME
```

Depois:

```powershell
Test-Path "$env:JAVA_HOME\bin\java.exe"
```

Se retornar `False`, o JAVA_HOME pode estar incorreto.

Verifique se o executável existe:

```powershell
Get-ChildItem "C:\Program Files\Eclipse Adoptium"
```

### Solução

1. Identifique o diretório correto do JDK.
2. Configure JAVA_HOME para a raiz da instalação.
3. Adicione `%JAVA_HOME%\bin` ao PATH.
4. Salve as alterações.
5. Feche o terminal.
6. Abra um novo PowerShell.
7. Execute novamente:

```powershell
java -version
```

---

## 4. Erro: 'javac' não é reconhecido

### Sintoma

O comando:

```powershell
javac -version
```

não é reconhecido.

Entretanto, `java -version` funciona.

### Causa provável

O sistema consegue localizar um executável Java, mas não consegue localizar o compilador.

Isso pode acontecer quando:

- Existe apenas um ambiente de execução disponível;
- O PATH aponta para uma instalação incompleta;
- O PATH aponta para um diretório diferente do JDK;
- Existe conflito entre versões instaladas.

### Diagnóstico

Execute:

```powershell
where.exe java
```

E:

```powershell
where.exe javac
```

Depois:

```powershell
Test-Path "$env:JAVA_HOME\bin\javac.exe"
```

### Solução

Confira se JAVA_HOME aponta para um JDK completo.

O caminho esperado deve conter:

```text
bin\java.exe
bin\javac.exe
```

Se o compilador existir no diretório, mas não for encontrado pelo terminal, revise o PATH.

Adicione:

```text
%JAVA_HOME%\bin
```

Abra um novo terminal e teste:

```powershell
javac -version
```

---

## 5. Erro: JAVA_HOME aponta para um diretório inexistente

### Sintoma

Ao executar:

```powershell
Test-Path "$env:JAVA_HOME"
```

O resultado é:

```text
False
```

### Causa

O caminho configurado não corresponde a um diretório existente.

Isso pode acontecer após:

- Atualização do JDK;
- Desinstalação de uma versão antiga;
- Alteração manual do diretório;
- Erro de digitação.

### Solução

Identifique a instalação:

```powershell
Get-ChildItem "C:\Program Files\Eclipse Adoptium"
```

Copie o caminho real do JDK 25.

Atualize JAVA_HOME nas variáveis de ambiente do Windows.

Depois, abra um novo terminal e valide:

```powershell
$env:JAVA_HOME
```

```powershell
Test-Path "$env:JAVA_HOME\bin\javac.exe"
```

O resultado esperado é `True`.

---

## 6. Problema: múltiplas versões do Java instaladas

### Sintoma

O comando:

```powershell
java -version
```

apresenta uma versão diferente da esperada.

Por exemplo, o ambiente deveria utilizar Java 25, mas o terminal apresenta Java 17 ou Java 21.

### Causas possíveis

- Mais de um JDK instalado;
- PATH com diretórios antigos;
- JAVA_HOME apontando para uma versão;
- PATH priorizando outra versão;
- Configurações diferentes entre terminal e IDE.

### Diagnóstico

Execute:

```powershell
where.exe java
```

E:

```powershell
where.exe javac
```

Também execute:

```powershell
$env:JAVA_HOME
```

A ordem dos resultados de `where.exe` ajuda a identificar qual executável poderá ser localizado primeiro pelo PATH.

### Solução

1. Identifique todas as instalações.
2. Escolha o JDK 25 como referência da jornada.
3. Configure JAVA_HOME para essa instalação.
4. Ajuste a ordem do PATH.
5. Evite manter entradas antigas desnecessárias.
6. Abra um novo terminal.
7. Valide novamente.

Não é obrigatório desinstalar todos os outros JDKs.

É possível manter diferentes versões para projetos distintos, desde que as configurações estejam claras.

---

## 7. Problema: JAVA_HOME aponta para a pasta bin

### Sintoma

O caminho configurado está semelhante a:

```text
C:\Program Files\Eclipse Adoptium\jdk-25...\bin
```

### Por que está errado?

JAVA_HOME deve apontar para a raiz do JDK.

A pasta `bin` deve ser acrescentada apenas no PATH.

### Configuração correta

JAVA_HOME:

```text
C:\Program Files\Eclipse Adoptium\jdk-25...
```

PATH:

```text
%JAVA_HOME%\bin
```

---

## 8. Problema: PATH configurado incorretamente

### Sintoma

Os executáveis não são encontrados, mesmo com o JDK instalado.

### Diagnóstico

Execute:

```powershell
$env:Path -split ';'
```

Procure a entrada:

```text
%JAVA_HOME%\bin
```

Também verifique:

```powershell
where.exe java
```

### Solução

Acesse as variáveis de ambiente do Windows.

Confirme se a entrada do Java está presente.

Não substitua todo o PATH.

O PATH contém informações necessárias para diversos programas e serviços.

Se houver entradas duplicadas ou caminhos antigos, remova apenas aquilo que tiver identificado com segurança.

---

## 9. Problema: terminal não reconhece alterações nas variáveis

### Sintoma

Você configurou JAVA_HOME e PATH, mas o terminal continua apresentando o comportamento anterior.

### Causa

O processo do terminal pode estar utilizando os valores de ambiente existentes no momento em que foi iniciado.

### Solução

1. Feche o PowerShell.
2. Feche o CMD, se estiver aberto.
3. Abra um novo terminal.
4. Execute:

```powershell
$env:JAVA_HOME
```

```powershell
java -version
```

Se o problema persistir, confira as variáveis diretamente nas configurações do Windows.

---

# PARTE II — PROBLEMAS DE COMPILAÇÃO E EXECUÇÃO

## 10. Erro: class HelloWorld is public, should be declared in a file named HelloWorld.java

### Sintoma

O compilador apresenta uma mensagem semelhante a:

```text
class HelloWorld is public, should be declared in a file named HelloWorld.java
```

### Causa

O nome do arquivo não corresponde ao nome da classe pública.

Exemplo incorreto:

Arquivo:

```text
Main.java
```

Código:

```java
public class HelloWorld {
}
```

### Solução

Renomeie o arquivo para:

```text
HelloWorld.java
```

Ou altere o nome da classe pública para corresponder ao arquivo.

Lembre-se de que Java diferencia letras maiúsculas e minúsculas.

---

## 11. Erro: ';' expected

### Sintoma

O compilador apresenta:

```text
';' expected
```

### Causa

Uma instrução que exige ponto e vírgula não foi finalizada corretamente.

Exemplo incorreto:

```java
System.out.println("Hello, World!")
```

Exemplo correto:

```java
System.out.println("Hello, World!");
```

### Solução

Revise a linha indicada pelo compilador.

Observe também as linhas anteriores, pois um erro de sintaxe pode afetar a interpretação das instruções seguintes.

---

## 12. Erro: reached end of file while parsing

### Sintoma

O compilador apresenta:

```text
reached end of file while parsing
```

### Causa

Normalmente existe uma chave de fechamento ausente.

Exemplo incorreto:

```\\java
public class HelloWorld {

    public static void main(String[] args) {

        System.out.println("Hello");
```

Exemplo correto:

```\\java
public class HelloWorld {

    public static void main(String[] args) {

        System.out.println("Hello");

    }

}
```

### Solução

Confira a abertura e o fechamento dos blocos.

Cada `{` precisa corresponder a uma `}`.

Utilize os recursos de identificação de pares de chaves disponíveis no IntelliJ IDEA.

---

## 13. Erro: Could not find or load main class

### Sintoma

Ao executar:

```powershell
java HelloWorld
```

A JVM apresenta:

```text
Error: Could not find or load main class HelloWorld
```

### Causas possíveis

- O arquivo `.class` não foi criado;
- O terminal está em outro diretório;
- O classpath está incorreto;
- O nome da classe foi digitado incorretamente;
- A estrutura de pacotes não corresponde ao comando utilizado.

### Diagnóstico

Execute:

```powershell
Get-ChildItem
```

Confira se existe:

```text
HelloWorld.class
```

Se não existir, compile novamente:

```powershell
javac HelloWorld.java
```

### Solução

Se o arquivo estiver no diretório atual:

```powershell
java -cp . HelloWorld
```

Se estiver em outro diretório, informe o caminho correto no classpath.

---

## 14. Erro: Main method not found

### Sintoma

A JVM encontra a classe, mas informa que não existe um método principal adequado.

### Causa

O método `main` pode estar ausente ou declarado incorretamente.

Exemplo tradicional:

```\\java
public static void main(String[] args) {
    System.out.println("Hello, World!");
}
```

Erros comuns:

```\\java
public void main(String[] args)
```

```\\java
public static void Main(String[] args)
```

```\\java
public static int main(String[] args)
```

### Solução

Confira:

- Nome `main` em letras minúsculas;
- Modificador `public`;
- Modificador `static`;
- Retorno `void`;
- Parâmetro `String[] args`.

**Observação:** versões modernas do Java possuem evoluções nas regras de declaração de métodos de entrada. Nesta etapa, utilizaremos a forma tradicional por ser amplamente reconhecida e adequada para nossos primeiros estudos.

---

## 15. Problema: programa compila, mas não apresenta resultado

### Causas possíveis

- Não existe uma instrução de saída;
- O método executado não contém o código esperado;
- A saída está sendo apresentada em outra janela;
- A execução foi iniciada em outra classe;
- O programa terminou sem produzir saída.

### Diagnóstico

Confira se existe:

```\\java
System.out.println("Hello, World!");
```

Verifique também se o método `main` está sendo executado.

No IntelliJ, confira a janela Run e o nome da configuração de execução.

---

## 16. Problema: pacote e diretório não correspondem

### Sintoma

A IDE apresenta erros relacionados a pacotes ou a classe não é localizada durante a execução.

### Exemplo

Código:

```java
package com.javajourney;
```

O arquivo precisa estar em uma estrutura correspondente:

```text
src/
└── com/
    └── javajourney/
        └── HelloWorld.java
```

### Causa

Em Java, a declaração de pacote precisa corresponder à organização esperada dos arquivos-fonte.

### Solução

Confira:

1. A declaração `package`;
2. O diretório em que o arquivo está;
3. A raiz de fontes configurada na IDE;
4. O nome completo da classe.

Para executar uma classe que pertence a um pacote, utilize o nome qualificado.

Exemplo:

```powershell
java -cp . com.javajourney.HelloWorld
```

Esse comando pressupõe que o diretório atual seja a raiz que contém `com`.

---

## 17. Problema: compilação funciona, mas a execução apresenta versão incompatível

### Sintoma

Uma aplicação compilada em uma versão do Java não pode ser executada em uma JVM mais antiga.

Pode aparecer uma mensagem semelhante a:

```text
UnsupportedClassVersionError
```

### Causa

O bytecode foi gerado para uma versão de Java superior à versão da JVM utilizada na execução.

### Diagnóstico

Confira:

```powershell
java -version
```

```powershell
javac -version
```

Se necessário, compare com a configuração do projeto no IntelliJ.

### Solução

Garanta que o compilador e o ambiente de execução sejam compatíveis.

Para nossa jornada, manteremos o JDK 25 como referência.

---

# PARTE III — PROBLEMAS NO INTELLIJ IDEA

## 18. IntelliJ IDEA não reconhece o JDK

### Sintoma

A IDE apresenta mensagens como:

```text
No SDK specified
```

Ou o projeto aparece sem um SDK configurado.

### Causa

O projeto não está associado a um JDK válido.

### Solução

1. Abra Project Structure.
2. Acesse a configuração de SDKs.
3. Adicione um JDK.
4. Selecione o diretório raiz da instalação.
5. Associe o SDK ao projeto.
6. Confirme a configuração.

Caminho ilustrativo:

```text
C:\Program Files\Eclipse Adoptium\jdk-25...
```

Não selecione apenas a pasta `bin`.

---

## 19. IntelliJ apresenta erro mesmo com Java instalado

### Causas possíveis

- Projeto associado a um SDK antigo;
- Configuração de linguagem incompatível;
- JDK não selecionado no projeto;
- Índices ainda sendo atualizados;
- Configuração de execução incorreta.

### Solução

Verifique:

- Project SDK;
- Language Level;
- Configuração de execução;
- Diretório do projeto;
- Mensagens apresentadas na janela Build.

Se necessário, recarregue o projeto e aguarde a indexação.

Não apague a pasta `.idea` como primeira tentativa: ela pode conter configurações importantes do projeto.

---

## 20. IntelliJ não apresenta o botão de execução

### Causas possíveis

- A classe não possui um método de entrada reconhecido;
- O arquivo não foi reconhecido como Java;
- O projeto não possui SDK;
- O arquivo está fora do diretório de fontes;
- A estrutura do projeto está incorreta.

### Solução

Confira se a classe possui:

```\\java
public static void main(String[] args) {
}
```

Verifique também se:

- O arquivo possui extensão `.java`;
- O projeto está configurado com JDK;
- A pasta `src` está reconhecida como diretório de fontes.

---

## 21. IntelliJ apresenta erros de compilação inesperados

### Procedimento

1. Leia a primeira mensagem de erro.
2. Abra a janela Build.
3. Identifique o arquivo e a linha.
4. Verifique se o código está correto.
5. Confirme o JDK utilizado.
6. Execute novamente.

Não confunda avisos de inspeção com erros de compilação.

Uma IDE pode apresentar sugestões e alertas mesmo quando o programa compila normalmente.

---

# PARTE IV — PROBLEMAS COM GIT E GITHUB

## 22. Erro: git não é reconhecido

### Sintoma

```text
git : O termo 'git' não é reconhecido
```

### Causas possíveis

- Git não instalado;
- Git não adicionado ao PATH;
- Terminal aberto antes da instalação;
- Instalação incompleta.

### Solução

Verifique se o Git está instalado.

O caminho padrão costuma estar relacionado a:

```text
C:\Program Files\Git\
```

O executável pode estar em:

```text
C:\Program Files\Git\cmd
```

Se necessário, adicione o diretório apropriado ao PATH.

Abra um novo terminal e execute:

```powershell
git --version
```

---

## 23. Erro: Author identity unknown

### Sintoma

Ao tentar realizar um commit, o Git apresenta uma mensagem semelhante a:

```text
Author identity unknown
```

### Causa

O Git não possui nome e e-mail configurados para registrar a autoria.

### Solução

Configure:

```bash
git config --global user.name "Seu Nome"
```

```bash
git config --global user.email "seu-email@example.com"
```

Confira:

```bash
git config --global --list
```

Depois, tente realizar o commit novamente.

---

## 24. Erro: not a git repository

### Sintoma

```text
fatal: not a git repository
```

### Causa

O terminal está em um diretório que não pertence a um repositório Git.

### Diagnóstico

Execute:

```powershell
Get-Location
```

Confira se está na pasta correta.

### Solução

Entre no diretório do projeto:

```powershell
cd C:\Desenvolvimento\Git\Java-Journey
```

Verifique:

```bash
git status
```

Se ainda não existir um repositório, inicialize:

```bash
git init
```

Não execute `git init` em diretórios aleatórios sem compreender o impacto.

---

## 25. Erro: Permission denied (publickey)

### Sintoma

Ao tentar realizar um push, aparece:

```text
Permission denied (publickey).
```

### Causas possíveis

- Chave pública não cadastrada no GitHub;
- Chave privada incorreta;
- Chave não carregada no agente;
- SSH Agent parado;
- Git utilizando outro cliente SSH;
- URL remota incorreta.

### Diagnóstico

Confira as chaves carregadas:

```powershell
ssh-add -l
```

Teste a conexão:

```powershell
ssh -T git@github.com
```

Confira o remote:

```bash
git remote -v
```

### Solução

1. Verifique se a chave pública correta está cadastrada no GitHub.
2. Confirme se a chave privada correspondente existe localmente.
3. Inicie o SSH Agent.
4. Adicione a chave com `ssh-add`.
5. Teste novamente.

### Conflito entre OpenSSH do Windows e Git for Windows

O Windows pode possuir uma implementação OpenSSH própria, enquanto o Git for Windows também disponibiliza ferramentas SSH.

Isso pode causar incompatibilidades entre o cliente SSH e o agente que mantém a chave carregada.

Se você estiver utilizando o OpenSSH do Windows e quiser que o Git utilize explicitamente esse cliente, poderá configurar:

```bash
git config --global core.sshCommand "C:/Windows/System32/OpenSSH/ssh.exe"
```

Faça isso somente se tiver identificado que o problema está relacionado à escolha do cliente SSH.

---

## 26. Erro: Repository not found

### Sintoma

O GitHub apresenta:

```text
ERROR: Repository not found.
```

### Causas possíveis

- Nome do repositório incorreto;
- Usuário incorreto na URL;
- Repositório inexistente;
- Conta sem permissão;
- URL remota apontando para outro projeto.

### Diagnóstico

Execute:

```bash
git remote -v
```

Confira a URL.

### Solução

Se estiver incorreta:

```bash
git remote set-url origin git@github.com:SEU_USUARIO/Java-Journey.git
```

Substitua o usuário e o nome do repositório pelos valores reais.

Depois:

```bash
git remote -v
```

---

## 27. Erro: failed to push some refs

### Sintoma

O Git informa que não conseguiu enviar os commits.

### Causas possíveis

- O repositório remoto possui commits que ainda não estão no local;
- A branch local está desatualizada;
- Existe divergência entre os históricos;
- A branch de destino está protegida.

### Solução

Primeiro, confira o estado:

```bash
git status
```

Atualize as referências remotas:

```bash
git fetch origin
```

Confira as branches:

```bash
git branch -vv
```

Se houver commits remotos que precisam ser integrados, utilize o fluxo adequado para o projeto.

Em um cenário simples, quando a branch remota deve ser integrada à local:

```bash
git pull --rebase origin main
```

Resolva eventuais conflitos, continue o rebase quando necessário e depois envie:

```bash
git push
```

**Atenção:** não utilize `git push --force` como solução automática. Essa operação pode sobrescrever histórico remoto e prejudicar o trabalho de outras pessoas.

---

## 28. Problema: arquivo não aparece no GitHub

### Diagnóstico

Execute:

```bash
git status
```

Se o arquivo estiver ignorado, verifique:

```bash
git check-ignore -v caminho/do/arquivo
```

Esse comando ajuda a identificar a regra responsável por ignorar o arquivo.

### Solução

Se o arquivo deveria fazer parte do repositório:

1. Revise o `.gitignore`;
2. Confira o caminho;
3. Adicione o arquivo;
4. Crie um commit;
5. Faça push.

Não versionamos arquivos sensíveis apenas para fazê-los aparecer no GitHub.

---

## 29. Problema: merge conflict

### Sintoma

O Git apresenta conflitos ao integrar alterações.

Um arquivo pode conter marcadores semelhantes a:

```text
<<<<<<< HEAD
Código da versão atual
=======
Código da outra versão
>>>>>>> branch
```

### O que significa?

Duas alterações incompatíveis foram realizadas na mesma região de um arquivo.

O Git precisa que o desenvolvedor determine como o conteúdo final deverá ficar.

### Solução

1. Abra o arquivo com conflito.
2. Identifique as duas versões.
3. Decida como integrar as alterações.
4. Remova os marcadores de conflito.
5. Salve o arquivo.
6. Revise o resultado.
7. Marque o arquivo como resolvido:

```bash
git add caminho/do/arquivo
```

8. Conclua o merge ou rebase conforme a operação iniciada.

Nunca deixe os marcadores de conflito no código final.

---

# PARTE V — BOAS PRÁTICAS DE DIAGNÓSTICO

## 30. Como interpretar mensagens de erro

Uma mensagem de erro geralmente contém informações importantes:

- Nome do erro;
- Arquivo envolvido;
- Linha;
- Componente responsável;
- Contexto da falha.

Exemplo:

```text
HelloWorld.java:5: error: ';' expected
```

Interpretação:

| Parte | Significado |
|---|---|
| `HelloWorld.java` | Arquivo envolvido |
| `5` | Linha indicada |
| `error` | Classificação |
| `';' expected` | Descrição do problema |

Não ignore a mensagem completa.

O erro pode estar na linha indicada ou em uma instrução anterior.

## 31. Procedimento padrão de troubleshooting

Sempre que encontrar um problema, utilize este fluxo:

```text
Identificar o erro
       |
       v
Reproduzir o problema
       |
       v
Coletar informações
       |
       v
Identificar a camada
       |
       v
Formular uma hipótese
       |
       v
Testar uma correção
       |
       v
Validar o resultado
       |
       v
Registrar a solução
```

### Exemplo prático

Problema:

```text
javac não é reconhecido
```

Diagnóstico:

1. Verificar se o JDK está instalado;
2. Conferir se `javac.exe` existe;
3. Verificar JAVA_HOME;
4. Verificar PATH;
5. Abrir um novo terminal;
6. Executar `javac -version`.

Essa sequência é mais eficiente do que reinstalar o Java imediatamente.

---

## 32. Comandos úteis de diagnóstico

### Java

```powershell
java -version
```

```powershell
javac -version
```

```powershell
where.exe java
```

```powershell
where.exe javac
```

```powershell
$env:JAVA_HOME
```

```powershell
Test-Path "$env:JAVA_HOME\bin\java.exe"
```

```powershell
Test-Path "$env:JAVA_HOME\bin\javac.exe"
```

### Git

```bash
git --version
```

```bash
git status
```

```bash
git branch -vv
```

```bash
git remote -v
```

```bash
git log --oneline
```

```bash
git config --global --list
```

### SSH

```powershell
ssh -V
```

```powershell
ssh-add -l
```

```powershell
ssh -T git@github.com
```

---

## 33. Quando reinstalar uma ferramenta?

A reinstalação deve ser uma das últimas alternativas, não a primeira.

Considere reinstalar quando:

- O instalador apresentou falha;
- Arquivos essenciais estão ausentes ou corrompidos;
- A instalação não pode ser reparada;
- A ferramenta não inicia mesmo após o diagnóstico;
- Existe incompatibilidade confirmada na instalação.

Antes de reinstalar:

1. Registre a versão instalada;
2. Identifique o diretório;
3. Verifique configurações importantes;
4. Preserve projetos e arquivos;
5. Consulte a documentação oficial.

Nunca remova a pasta do projeto ou do repositório Git como tentativa genérica de resolver problemas da IDE.

---

## 34. Registro de problemas encontrados

Durante a Java Journey, podemos registrar problemas reais para construir nosso próprio histórico técnico.

Utilize este modelo:

### Registro de incidente

**Data:**

**Ferramenta:**

**Versão:**

**Descrição do problema:**

**Mensagem de erro:**

**Contexto em que ocorreu:**

**Hipótese inicial:**

**Diagnóstico realizado:**

**Solução aplicada:**

**Resultado após a correção:**

**O que aprendi:**

Esse registro será útil para desenvolver autonomia técnica e melhorar nossa capacidade de diagnóstico.

---

## 35. Checklist de saúde do ambiente

Execute esta verificação quando suspeitar que o ambiente está apresentando problemas.

### Java

- [ ] `java -version` funciona.
- [ ] `javac -version` funciona.
- [ ] JAVA_HOME aponta para o JDK correto.
- [ ] PATH contém o diretório `bin`.
- [ ] `where.exe java` apresenta o executável esperado.
- [ ] `where.exe javac` apresenta o compilador esperado.

### IntelliJ IDEA

- [ ] A IDE inicia normalmente.
- [ ] O projeto reconhece o JDK.
- [ ] A configuração de execução está correta.
- [ ] A compilação funciona.
- [ ] O debugger pode ser iniciado.

### Git

- [ ] `git --version` funciona.
- [ ] Identidade global configurada.
- [ ] Repositório reconhecido.
- [ ] Branch correta.
- [ ] Remote correto.
- [ ] Commit funciona.

### GitHub

- [ ] Chave pública cadastrada.
- [ ] SSH Agent disponível.
- [ ] Autenticação SSH funciona.
- [ ] Push funciona.
- [ ] Repositório remoto acessível.

---

## 36. Fontes oficiais para diagnóstico

- [Oracle — Java Documentation](https://docs.oracle.com/en/java/javase/25/)
- [Oracle — Java Troubleshooting](https://docs.oracle.com/en/java/javase/25/troubleshoot/)
- [Dev.java](https://dev.java/)
- [JetBrains — IntelliJ IDEA Help](https://www.jetbrains.com/help/idea/)
- [Git Documentation](https://git-scm.com/doc)
- [GitHub Docs — SSH](https://docs.github.com/en/authentication/connecting-to-github-with-ssh)
- [GitHub Docs — Git Troubleshooting](https://docs.github.com/en/get-started/git-basics)

---

## 37. Critério de conclusão

O objetivo deste documento não é memorizar todas as mensagens de erro.

O que precisamos desenvolver é a capacidade de investigar problemas.

Ao concluir o módulo, você deverá conseguir:

1. Identificar se um erro está relacionado à instalação ou configuração;
2. Verificar se o Java está disponível no PATH;
3. Diferenciar problemas de JDK, compilador e JVM;
4. Investigar erros básicos de compilação;
5. Identificar problemas de classpath;
6. Corrigir configurações do IntelliJ;
7. Investigar problemas de Git;
8. Diagnosticar falhas de autenticação SSH;
9. Interpretar mensagens de erro;
10. Documentar uma solução técnica.

---

**Fim da etapa 04 — Troubleshooting.**