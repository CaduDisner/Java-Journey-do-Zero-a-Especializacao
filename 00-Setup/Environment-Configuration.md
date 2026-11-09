# Environment Configuration — Configuração do Ambiente

> **Módulo:** 00-Setup  
> **Etapa:** 02 — Environment Configuration  
> **Sistema operacional:** Windows 10  
> **Java:** JDK 25 LTS  
> **IDE:** IntelliJ IDEA  
> **Versionamento:** Git e GitHub

---

## 1. Objetivo

Nesta etapa, vamos configurar as ferramentas instaladas anteriormente para que possam trabalhar corretamente em conjunto.

Instalar um JDK não significa necessariamente que todos os programas conseguirão encontrá-lo. Para isso, precisamos configurar variáveis de ambiente e compreender como o sistema operacional localiza executáveis.

Também configuraremos o Git para registrar corretamente a autoria dos commits e estabelecer uma conexão segura com o GitHub.

Ao finalizar, teremos:

- `JAVA_HOME` configurado;
- `PATH` atualizado;
- Java e compilador acessíveis pelo terminal;
- IntelliJ IDEA reconhecendo o JDK;
- Git configurado globalmente;
- Chave SSH criada;
- Chave pública adicionada ao GitHub;
- Conexão SSH validada;
- Repositório pronto para receber nosso projeto.

## 2. Entendendo as variáveis de ambiente

Variáveis de ambiente são valores mantidos pelo sistema operacional e disponibilizados aos processos em execução.

Elas permitem que aplicações encontrem diretórios, executáveis e configurações sem precisar receber caminhos completos a cada execução.

Exemplo:

Quando digitamos:

```powershell
java -version
```

O Windows precisa localizar o executável `java`.

O PATH informa ao sistema quais diretórios devem ser consultados para encontrar comandos.

### 2.1. O que é JAVA_HOME?

`JAVA_HOME` é uma variável convencional utilizada por ferramentas Java para identificar o diretório principal do JDK.

Exemplo:

```text
JAVA_HOME=C:\Program Files\Eclipse Adoptium\jdk-25...
```

O valor precisa apontar para a pasta raiz do JDK, não para a pasta `bin`.

Correto:

```text
C:\Program Files\Eclipse Adoptium\jdk-25...
```

Incorreto:

```text
C:\Program Files\Eclipse Adoptium\jdk-25...\bin
```

### 2.2. O que é PATH?

`PATH` é uma variável que contém uma lista de diretórios nos quais o Windows procura executáveis.

Para Java, precisamos disponibilizar:

```text
%JAVA_HOME%\bin
```

Isso permite executar comandos como:

```powershell
java
javac
jar
javadoc
```

sem informar o caminho completo do executável.

### 2.3. Diferença entre JAVA_HOME e PATH

| Variável | Responsabilidade |
|---|---|
| `JAVA_HOME` | Identifica a instalação principal do JDK |
| `PATH` | Permite localizar comandos executáveis |

Uma ferramenta pode consultar `JAVA_HOME` diretamente, enquanto outra pode simplesmente tentar localizar `java.exe` pelo PATH.

Por isso, configurar as duas variáveis é uma prática importante.

---

## 3. Configurando JAVA_HOME no Windows 10

### 3.1. Identificar o diretório do JDK

Abra o Explorador de Arquivos e acesse:

```text
C:\Program Files\Eclipse Adoptium\
```

Localize a pasta referente ao JDK 25.

O nome pode variar conforme a atualização instalada.

Exemplo:

```text
C:\Program Files\Eclipse Adoptium\jdk-25...
```

Copie o caminho completo da pasta.

### 3.2. Abrir as configurações de variáveis de ambiente

1. Abra o menu Iniciar.
2. Pesquise por `Editar as variáveis de ambiente do sistema`.
3. Abra a opção correspondente.
4. Na janela Propriedades do Sistema, clique em **Variáveis de Ambiente**.

Serão exibidas duas áreas:

- Variáveis de usuário;
- Variáveis do sistema.

### 3.3. Criar JAVA_HOME

Na seção de variáveis do sistema:

1. Clique em **Novo**.
2. Nome da variável:

```text
JAVA_HOME
```

3. Valor da variável:

```text
C:\Program Files\Eclipse Adoptium\jdk-25...
```

Substitua o exemplo pelo caminho real da instalação.

4. Confirme a criação.

**Por que utilizar variáveis do sistema?**

Elas ficam disponíveis para diferentes usuários e processos do Windows, dependendo das permissões e da configuração. Para um computador de uso pessoal, também seria possível utilizar uma variável de usuário.

Nesta jornada, utilizaremos a configuração de sistema para manter o ambiente consistente.

---

## 4. Configurando PATH

Na mesma janela:

1. Localize a variável `Path`.
2. Selecione-a.
3. Clique em Editar.
4. Clique em Novo.
5. Adicione:

```text
%JAVA_HOME%\bin
```

6. Confirme todas as janelas.

### 4.1. Por que utilizar %JAVA_HOME%\bin?

Em vez de escrever o caminho completo do JDK no PATH, utilizamos uma referência à variável JAVA_HOME.

Isso facilita futuras atualizações.

Por exemplo, se mudarmos o caminho do JDK, podemos atualizar JAVA_HOME sem precisar modificar todas as referências ao diretório.

### 4.2. Cuidado com o PATH

Não apague as entradas existentes.

O PATH pode conter caminhos importantes para:

- Windows;
- Git;
- PowerShell;
- Python;
- Node.js;
- outras ferramentas.

Adicionar uma nova entrada é diferente de substituir toda a variável.

---

## 5. Validando a configuração Java

Feche os terminais abertos anteriormente e abra um novo PowerShell.

Isso é necessário porque os processos já abertos normalmente não recebem automaticamente as alterações feitas nas variáveis de ambiente.

Execute:

```powershell
$env:JAVA_HOME
```

O resultado deve apresentar o diretório do JDK.

Depois:

```powershell
$env:Path -split ';' | Select-String 'Java|Adoptium'
```

Esse comando ajuda a identificar entradas relacionadas ao Java.

Agora valide:

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

### 5.1. Verificar se o JAVA_HOME aponta para um JDK válido

Execute:

```powershell
Test-Path "$env:JAVA_HOME\bin\java.exe"
```

E:

```powershell
Test-Path "$env:JAVA_HOME\bin\javac.exe"
```

O resultado esperado é:

```text
True
```

Se aparecer `False`, o caminho informado em JAVA_HOME está incorreto ou não corresponde a uma instalação completa do JDK.

### 5.2. Verificar a versão utilizada

Execute:

```powershell
& "$env:JAVA_HOME\bin\java.exe" -version
```

E:

```powershell
& "$env:JAVA_HOME\bin\javac.exe" -version
```

Esses comandos executam diretamente os programas presentes no diretório apontado por JAVA_HOME.

Isso é útil para diferenciar problemas de instalação de problemas no PATH.

---

## 6. Configurando o JDK no IntelliJ IDEA

O IntelliJ IDEA precisa saber qual JDK utilizar nos projetos.

### 6.1. Configurar o SDK

Abra o IntelliJ IDEA.

Acesse:

```text
File > Project Structure
```

Localize a área de SDKs ou a configuração de SDK do projeto, conforme a versão instalada.

Adicione um JDK se ainda não houver um configurado.

Selecione o diretório de instalação:

```text
C:\Program Files\Eclipse Adoptium\jdk-25...
```

Aponte para a pasta raiz do JDK.

Não selecione a pasta `bin`.

### 6.2. Configurar o Project SDK

Ao configurar um projeto, utilize:

```text
Project SDK: JDK 25
```

A opção de linguagem ou nível de linguagem deverá ser compatível com Java 25.

### 6.3. Verificar a configuração

Confirme que:

- O SDK aparece como JDK 25;
- O diretório está correto;
- O IntelliJ reconhece as bibliotecas Java;
- O projeto não apresenta erro de SDK ausente.

**Observação:** a interface pode mudar entre versões do IntelliJ IDEA. O nome exato dos menus pode variar, mas o objetivo é sempre associar o projeto a um JDK instalado.

---

## 7. Configuração inicial do Git

Agora vamos configurar a identidade que será registrada nos commits.

Abra o PowerShell ou Git Bash.

### 7.1. Configurar nome

```bash
git config --global user.name "Seu Nome"
```

Substitua pelo nome que deseja utilizar nos commits.

### 7.2. Configurar e-mail

```bash
git config --global user.email "seu-email@example.com"
```

Utilize o e-mail associado à sua conta GitHub ou um endereço de e-mail privado fornecido pelo GitHub.

**Importante:** não coloque dados pessoais em arquivos públicos sem necessidade. O GitHub oferece a opção de utilizar um endereço de e-mail `noreply`.

### 7.3. Configurar branch principal

```bash
git config --global init.defaultBranch main
```

Isso define `main` como nome padrão da branch inicial de novos repositórios.

### 7.4. Conferir as configurações

```bash
git config --global --list
```

Para verificar valores individualmente:

```bash
git config --global user.name
```

```bash
git config --global user.email
```

```bash
git config --global init.defaultBranch
```

### 7.5. Entendendo --global

O parâmetro `--global` aplica a configuração ao usuário atual do sistema.

Existem três escopos principais:

| Escopo | Aplicação |
|---|---|
| `--system` | Configuração do sistema inteiro |
| `--global` | Configuração do usuário atual |
| `--local` | Configuração do repositório atual |

A configuração local pode sobrescrever a global.

---

## 8. Configurando autenticação SSH com GitHub

### 8.1. O que é SSH?

SSH significa **Secure Shell**.

É um protocolo que permite estabelecer conexões seguras entre computadores.

No contexto do GitHub, podemos utilizá-lo para autenticar operações Git sem precisar informar usuário e senha a cada operação.

A autenticação utiliza um par de chaves:

- Chave privada: permanece no computador e nunca deve ser compartilhada;
- Chave pública: pode ser cadastrada no GitHub.

### 8.2. Verificar chaves existentes

No PowerShell:

```powershell
Get-ChildItem "$env:USERPROFILE\.ssh"
```

Procure arquivos semelhantes a:

```text
id_ed25519
id_ed25519.pub
```

Se já existir uma chave que você utiliza, não a sobrescreva.

Se não existir, prossiga.

### 8.3. Gerar uma chave Ed25519

Execute:

```powershell
ssh-keygen -t ed25519 -C "seu-email@example.com"
```

O comando utiliza:

- `ssh-keygen`: ferramenta de geração de chaves;
- `-t ed25519`: escolhe o algoritmo Ed25519;
- `-C`: adiciona um comentário para identificar a chave.

Quando solicitado o caminho, pressione Enter para utilizar o padrão:

```text
C:\Users\SEU_USUARIO\.ssh\id_ed25519
```

Depois, defina uma passphrase.

A passphrase protege a chave privada caso alguém tenha acesso ao arquivo.

### 8.4. Identificar os arquivos gerados

Serão criados dois arquivos:

```text
id_ed25519
id_ed25519.pub
```

A diferença é fundamental:

| Arquivo | Tipo | Compartilhamento |
|---|---|---|
| `id_ed25519` | Chave privada | Nunca compartilhar |
| `id_ed25519.pub` | Chave pública | Pode ser cadastrada no GitHub |

Nunca publique ou envie a chave privada.

### 8.5. Iniciar o serviço SSH Agent

O SSH Agent ajuda a manter a chave disponível durante a sessão, evitando digitar a passphrase repetidamente.

Abra o PowerShell como administrador para configurar o serviço:

```powershell
Get-Service -Name ssh-agent
```

Configure a inicialização:

```powershell
Set-Service -Name ssh-agent -StartupType Manual
```

Inicie o serviço:

```powershell
Start-Service ssh-agent
```

Se a política da máquina impedir essa alteração, verifique as permissões e as políticas locais antes de tentar contorná-las.

### 8.6. Adicionar a chave privada ao agente

Em um PowerShell normal:

```powershell
ssh-add "$env:USERPROFILE\.ssh\id_ed25519"
```

Informe a passphrase quando solicitada.

Para conferir as chaves carregadas:

```powershell
ssh-add -l
```

### 8.7. Copiar a chave pública

Execute:

```powershell
Get-Content "$env:USERPROFILE\.ssh\id_ed25519.pub" |
Set-Clipboard
```

O conteúdo da chave pública será copiado.

### 8.8. Adicionar a chave ao GitHub

1. Entre em https://github.com/
2. Acesse sua conta.
3. Abra Settings.
4. Acesse SSH and GPG keys.
5. Clique em New SSH key.
6. Defina um título que identifique o computador.
7. Selecione Authentication Key.
8. Cole a chave pública.
9. Salve.

A chave pública pode ser compartilhada com o GitHub. A chave privada continua armazenada localmente.

### 8.9. Testar a conexão

No PowerShell:

```powershell
ssh -T git@github.com
```

Na primeira conexão, o SSH poderá perguntar se você confia na identidade do servidor.

Confira a impressão digital apresentada com as informações oficiais do GitHub antes de aceitar.

Uma conexão bem-sucedida apresentará uma mensagem semelhante a:

```text
Hi USERNAME! You've successfully authenticated,
but GitHub does not provide shell access.
```

Essa mensagem indica que a autenticação funcionou.

O GitHub não oferece um terminal SSH interativo por essa conexão; ela é utilizada para operações Git.

---

## 9. Configurações adicionais do Git

### 9.1. Configurar o tratamento de finais de linha

No Windows, os arquivos de texto podem utilizar CRLF como final de linha. Em muitos ambientes Unix, utiliza-se LF.

Uma configuração comum para Windows é:

```bash
git config --global core.autocrlf true
```

Essa configuração permite que o Git normalize finais de linha no repositório e ajuste os arquivos no checkout.

O comportamento precisa ser consistente com as regras do projeto. Em repositórios colaborativos, um `.gitattributes` pode estabelecer a política de finais de linha de maneira explícita.

### 9.2. Configurar o nome padrão da branch

```bash
git config --global init.defaultBranch main
```

### 9.3. Conferir todas as configurações

```bash
git config --global --list
```

### 9.4. Verificar qual arquivo contém uma configuração

```bash
git config --show-origin --get user.name
```

Esse comando informa a origem da configuração consultada.

---

## 10. Criando o repositório Java Journey

Agora vamos preparar o diretório do nosso projeto.

No PowerShell:

```powershell
cd C:\Desenvolvimento\Git
```

Crie o diretório:

```powershell
New-Item -ItemType Directory -Name Java-Journey
```

Entre nele:

```powershell
cd Java-Journey
```

Inicialize o Git:

```bash
git init
```

Verifique:

```bash
git status
```

O resultado deve indicar que existe um repositório Git e que ainda não há commits.

### 10.1. O que aconteceu?

Ao executar `git init`, o Git criou a estrutura interna de controle de versão na pasta `.git`.

Essa estrutura armazena informações sobre commits, branches, referências e outros dados necessários para o funcionamento do Git.

Não altere manualmente o conteúdo de `.git`.

### 10.2. Conectar o repositório ao GitHub

Crie um repositório vazio no GitHub com o nome:

```text
Java-Journey
```

Se o repositório local já possui arquivos, evite inicializar o repositório remoto com README, licença ou `.gitignore` para não criar históricos independentes desnecessariamente.

Depois, conecte o repositório local ao remoto:

```bash
git remote add origin git@github.com:SEU_USUARIO/Java-Journey.git
```

Substitua `SEU_USUARIO` pelo seu usuário real.

Confira:

```bash
git remote -v
```

A URL deve apontar para o repositório correto.

---

## 11. Segurança e boas práticas

Durante toda a jornada, adotaremos alguns princípios:

1. Nunca compartilhar chaves privadas.
2. Nunca publicar senhas ou tokens.
3. Nunca armazenar credenciais diretamente no código.
4. Não enviar arquivos `.env` com informações sensíveis.
5. Revisar alterações antes de realizar commits.
6. Utilizar autenticação segura.
7. Evitar executar scripts desconhecidos sem compreender sua origem.
8. Manter ferramentas atualizadas por fontes oficiais.

### 11.1. Exemplo de .gitignore

No futuro, nossos projetos poderão utilizar arquivos que não devem ser versionados.

Exemplo:

```gitignore
# Arquivos de compilação
*.class
out/
target/

# Arquivos de configuração local
.env
.env.*
!.env.example

# Arquivos de IDE
.idea/
*.iml

# Logs
*.log

# Arquivos temporários
*.tmp

# Sistema operacional
Thumbs.db
.DS_Store
```

O `.gitignore` evita que determinados arquivos sejam incluídos automaticamente no controle de versão.

Ele não remove arquivos que já foram adicionados ao histórico.

---

## 12. Checklist de configuração

### Java

- [ ] JAVA_HOME criado.
- [ ] JAVA_HOME aponta para a raiz do JDK 25.
- [ ] PATH contém `%JAVA_HOME%\bin`.
- [ ] `java -version` funciona.
- [ ] `javac -version` funciona.
- [ ] `where.exe java` apresenta o caminho esperado.
- [ ] `where.exe javac` apresenta o caminho esperado.

### IntelliJ IDEA

- [ ] IntelliJ reconhece o JDK 25.
- [ ] O SDK está configurado corretamente.
- [ ] O nível de linguagem é compatível com Java 25.

### Git

- [ ] Nome global configurado.
- [ ] E-mail global configurado.
- [ ] Branch padrão configurada como `main`.
- [ ] `git --version` funciona.
- [ ] `git config --global --list` apresenta os dados esperados.

### GitHub

- [ ] Chave SSH criada ou chave existente validada.
- [ ] Chave pública cadastrada.
- [ ] Chave privada protegida.
- [ ] SSH Agent configurado.
- [ ] `ssh -T git@github.com` autenticou corretamente.
- [ ] Repositório remoto criado.
- [ ] Remote `origin` configurado.

---

## 13. Resultado esperado

Ao concluir esta etapa, o ambiente estará configurado para desenvolver aplicações Java e versionar nosso trabalho.

A próxima etapa será prática.

Vamos criar nosso primeiro projeto Java, escrever o primeiro programa, entender o processo de compilação e execução e realizar nossos primeiros commits.

---

## 14. Fontes oficiais

- [Dev.java](https://dev.java/)
- [Oracle Java Documentation](https://docs.oracle.com/en/java/javase/25/)
- [Git Documentation](https://git-scm.com/doc)
- [GitHub Docs — Connecting with SSH](https://docs.github.com/en/authentication/connecting-to-github-with-ssh)
- [GitHub Docs — Testing your SSH connection](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/testing-your-ssh-connection?platform=windows)
- [GitHub Docs — Git Basics](https://docs.github.com/en/get-started/git-basics)

---

**Fim da etapa 02 — Environment Configuration.**