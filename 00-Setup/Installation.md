# Installation — Instalação do Ambiente Java

> **Módulo:** 00-Setup  
> **Etapa:** 01 — Installation  
> **Sistema operacional:** Windows 10  
> **Java:** JDK 25 LTS  
> **Distribuição:** Eclipse Temurin  
> **IDE:** IntelliJ IDEA  
> **Versionamento:** Git

---

## 1. Objetivo

Esta etapa tem como objetivo instalar todas as ferramentas necessárias para iniciar o desenvolvimento de aplicações utilizando Java.

Ao finalizar este documento, teremos:

- JDK 25 instalado;
- JVM disponível para executar aplicações Java;
- Compilador `javac` funcionando;
- IntelliJ IDEA instalado;
- Git instalado;
- Estrutura inicial de diretórios de desenvolvimento criada;
- Todas as ferramentas prontas para serem configuradas e utilizadas.

A instalação é apenas o primeiro passo. Posteriormente, configuraremos as variáveis de ambiente, a integração entre as ferramentas e a autenticação com o GitHub.

## 2. Preparação do ambiente

Antes de instalar qualquer ferramenta, precisamos verificar algumas informações do sistema operacional.

### 2.1. Verificar a versão e arquitetura do Windows

Pressione:

`Windows + R`

Digite:

```powershell
msinfo32
```

Localize as informações:

- Nome do sistema operacional;
- Versão;
- Tipo de sistema.

Para esta jornada, consideraremos Windows 10 de 64 bits.

Também é possível verificar a arquitetura pelo PowerShell:

```powershell
Get-CimInstance Win32_OperatingSystem |
Select-Object Caption, Version, OSArchitecture
```

**Por que verificar isso?**

Os instaladores são distribuídos para diferentes arquiteturas, como x64 e ARM64. Utilizar um instalador incompatível pode impedir a instalação ou causar problemas de execução.

### 2.2. Verificar se já existe Java instalado

Abra o PowerShell e execute:

```powershell
java -version
```

Depois:

```powershell
javac -version
```

E:

```powershell
where.exe java
```

Esses comandos ajudam a identificar instalações anteriores.

Interpretação:

| Comando | Finalidade |
|---|---|
| `java -version` | Verifica a versão do ambiente de execução Java |
| `javac -version` | Verifica a versão do compilador Java |
| `where.exe java` | Mostra os executáveis Java encontrados no PATH |

Se o sistema informar que o comando não é reconhecido, isso não significa necessariamente que o Java nunca foi instalado. Pode significar apenas que o executável não está disponível no PATH.

Não remova instalações antigas sem identificar primeiro sua origem e utilização.

---

## 3. Organização dos diretórios

Antes de começar a desenvolver, vamos estabelecer uma estrutura organizada para armazenar estudos, exercícios, projetos e documentação.

Crie os seguintes diretórios:

```text
C:\
└── Desenvolvimento\
    ├── Java\
    │   ├── Estudos\
    │   ├── Exercicios\
    │   ├── Projetos\
    │   └── Documentacao\
    │
    ├── Git\
    │
    └── Outros\
```

Você pode criar toda a estrutura pelo PowerShell:

```powershell
$directories = @(
    "C:\Desenvolvimento\Java\Estudos",
    "C:\Desenvolvimento\Java\Exercicios",
    "C:\Desenvolvimento\Java\Projetos",
    "C:\Desenvolvimento\Java\Documentacao",
    "C:\Desenvolvimento\Git",
    "C:\Desenvolvimento\Outros"
)

foreach ($directory in $directories) {
    New-Item -ItemType Directory -Path $directory -Force
}
```

### Por que organizar dessa maneira?

- **Estudos:** códigos produzidos durante as aulas e explicações.
- **Exercicios:** resolução de atividades e desafios.
- **Projetos:** aplicações completas e projetos de portfólio.
- **Documentacao:** anotações, materiais técnicos e referências.
- **Git:** repositórios e materiais relacionados ao versionamento.
- **Outros:** ferramentas e arquivos complementares.

Essa separação ajuda a evitar que exercícios, projetos e arquivos temporários fiquem misturados.

---

## 4. Instalação do Java Development Kit (JDK)

### 4.1. O que é Java?

Java é uma linguagem de programação de propósito geral, orientada a objetos e projetada para permitir o desenvolvimento de aplicações em diferentes plataformas.

Uma de suas características mais importantes é a capacidade de executar programas em diferentes sistemas operacionais por meio da Java Virtual Machine (JVM).

Java é utilizado em:

- Sistemas corporativos;
- Aplicações bancárias;
- APIs e serviços backend;
- Sistemas distribuídos;
- Aplicações web;
- Sistemas de processamento de dados;
- Plataformas empresariais.

### 4.2. O que é JDK?

JDK significa **Java Development Kit**.

É o conjunto de ferramentas necessário para desenvolver aplicações Java.

Entre seus principais componentes estão:

| Componente | Responsabilidade |
|---|---|
| `javac` | Compilar código-fonte Java |
| `java` | Iniciar a execução de aplicações Java |
| JVM | Executar bytecode Java |
| Bibliotecas Java | Disponibilizar funcionalidades para os programas |
| Ferramentas adicionais | Diagnóstico, documentação e desenvolvimento |

### 4.3. Diferença entre JDK, JVM e JRE

É importante compreender essas três nomenclaturas desde o início.

**JVM — Java Virtual Machine**

É a máquina virtual responsável por executar o bytecode Java.

O código-fonte Java não é executado diretamente pela JVM. Primeiro ele é compilado para bytecode, que posteriormente será interpretado ou compilado em tempo de execução pela máquina virtual.

**JRE — Java Runtime Environment**

É a denominação tradicional do ambiente de execução, composto pela JVM e pelos componentes necessários para executar aplicações Java.

Nas distribuições modernas, a separação entre JDK e JRE não funciona exatamente como nas versões antigas. Não precisamos instalar um JRE separado para desenvolver com o JDK.

**JDK — Java Development Kit**

É o kit de desenvolvimento que contém o compilador e as ferramentas necessárias para criar, compilar e executar aplicações.

Representação simplificada:

```text
Código-fonte (.java)
        |
        v
     javac
   Compilador
        |
        v
Bytecode (.class)
        |
        v
       JVM
        |
        v
Aplicação em execução
```

### 4.4. Por que utilizaremos Java 25 LTS?

Java 25 é uma versão LTS (*Long-Term Support*), ou seja, uma versão de suporte de longo prazo.

Nossa escolha considera:

- Estabilidade para uma jornada de estudos extensa;
- Recursos modernos da linguagem;
- Compatibilidade com ferramentas atuais;
- Relevância para desenvolvimento profissional;
- Uma versão de referência consistente para exercícios e projetos.

É importante diferenciar a versão do Java da distribuição do JDK.

Java 25 é a versão da plataforma. Eclipse Temurin é uma distribuição do OpenJDK que disponibiliza essa plataforma.

### 4.5. Baixar o JDK 25

Utilizaremos o Eclipse Temurin.

Acesse:

https://adoptium.net/temurin/releases/?version=25

Selecione:

| Opção | Valor |
|---|---|
| Version | 25 - LTS |
| Operating System | Windows |
| Architecture | x64 |
| Package Type | JDK |
| JVM | HotSpot |

Escolha o instalador `.MSI` para Windows.

**Atenção:** selecione JDK, não JRE. Precisamos do compilador `javac` para desenvolver aplicações.

Outra opção oficial é o Oracle JDK:

https://www.oracle.com/java/technologies/downloads/

Para esta jornada, manteremos o Eclipse Temurin como distribuição de referência.

### 4.6. Instalação do JDK

Após baixar o instalador:

1. Execute o arquivo `.MSI`.
2. Aceite os termos de licença.
3. Mantenha o diretório de instalação padrão, se não houver motivo para alterá-lo.
4. Confira as opções de instalação.
5. Finalize o processo.

O caminho poderá seguir um padrão semelhante a:

```text
C:\Program Files\Eclipse Adoptium\jdk-25...
```

O nome exato do diretório depende da atualização instalada.

**Importante:** nesta etapa, não precisamos configurar manualmente as variáveis de ambiente. Faremos isso no documento `environment-configuration/README.md`.

### 4.7. Primeira validação do Java

Abra um novo PowerShell e execute:

```powershell
java -version
```

Depois:

```powershell
javac -version
```

O resultado deve indicar a versão principal 25.

Exemplo ilustrativo:

```text
java version "25..."
```

E:

```text
javac 25...
```

Se ambos funcionarem, temos evidência de que o ambiente de execução e o compilador estão acessíveis pelo terminal.

Se não funcionarem, não tente alterar várias configurações aleatoriamente. Consulte o documento de troubleshooting.

---

## 5. Instalação do IntelliJ IDEA

### 5.1. O que é uma IDE?

IDE significa **Integrated Development Environment**, ou Ambiente de Desenvolvimento Integrado.

É uma ferramenta que reúne recursos para facilitar o desenvolvimento de software.

Uma IDE pode oferecer:

- Editor de código;
- Autocomplete;
- Depuração;
- Gerenciamento de projetos;
- Integração com Git;
- Navegação entre classes;
- Identificação de erros;
- Execução de aplicações;
- Integração com ferramentas de build.

### 5.2. Por que utilizaremos IntelliJ IDEA?

O IntelliJ IDEA será nossa IDE principal para Java.

Ele oferece recursos importantes para o aprendizado e para o desenvolvimento profissional, como:

- Análise inteligente de código;
- Sugestões de implementação;
- Refatoração;
- Depuração visual;
- Gerenciamento de dependências;
- Integração com Git;
- Suporte ao ecossistema Java.

### 5.3. Download

Acesse o site oficial:

https://www.jetbrains.com/idea/download/

Baixe a versão para Windows.

O IntelliJ IDEA passou a utilizar uma distribuição unificada. Os recursos essenciais de desenvolvimento Java estão disponíveis gratuitamente; funcionalidades adicionais podem depender de assinatura.

### 5.4. Instalação

1. Execute o instalador.
2. Escolha o diretório de instalação.
3. Confira as opções disponíveis.
4. Finalize a instalação.
5. Abra o IntelliJ IDEA.

Se o instalador apresentar opções como integração ao menu de contexto ou associação de arquivos, escolha conforme sua preferência.

Não é necessário instalar plugins adicionais neste primeiro momento.

### 5.5. Primeiro acesso

Ao abrir a IDE:

1. Aceite os termos apresentados.
2. Escolha o tema visual de sua preferência.
3. Mantenha a configuração inicial recomendada.
4. Aguarde a inicialização.

Não criaremos ainda um projeto definitivo. Isso será feito em `first-steps/README.md`.

### 5.6. Conhecendo o papel da IDE

É importante entender que o IntelliJ IDEA não substitui o JDK.

A IDE é uma ferramenta de desenvolvimento. O JDK fornece o compilador, as bibliotecas e as ferramentas que tornam possível construir aplicações Java.

A relação é:

```text
IntelliJ IDEA
      |
      | utiliza
      v
     JDK
      |
      | disponibiliza
      v
   Compilador
      |
      v
     JVM
      |
      v
  Aplicação Java
```

---

## 6. Instalação do Git

### 6.1. O que é Git?

Git é um sistema distribuído de controle de versão.

Ele permite registrar alterações realizadas nos arquivos de um projeto, recuperar versões anteriores, trabalhar com branches e integrar contribuições de diferentes desenvolvedores.

Git é uma ferramenta fundamental na engenharia de software.

### 6.2. O que é GitHub?

GitHub é uma plataforma de hospedagem e colaboração de repositórios Git.

A diferença fundamental:

| Git | GitHub |
|---|---|
| Ferramenta de versionamento | Plataforma de hospedagem e colaboração |
| Funciona localmente | Funciona como serviço online |
| Registra alterações | Armazena e compartilha repositórios |
| Gerencia branches e commits | Oferece Pull Requests, Issues e recursos colaborativos |

Ter Git instalado não significa que você já esteja conectado ao GitHub.

Essa integração será configurada no documento de configuração do ambiente.

### 6.3. Download

Acesse:

https://git-scm.com/install/windows

Baixe o instalador adequado à arquitetura do Windows.

Também é possível consultar:

https://gitforwindows.org/

### 6.4. Instalação

Execute o instalador.

Durante o processo, serão apresentadas diversas opções.

Para nosso ambiente, as seguintes escolhas são adequadas:

- Editor padrão: escolha o editor de sua preferência; o Visual Studio Code é uma opção, caso esteja instalado.
- Nome inicial da branch: `main`.
- PATH: permitir que o Git seja utilizado pelo terminal e por outras ferramentas.
- SSH: manter a implementação OpenSSH disponível.
- HTTPS: utilizar a biblioteca padrão disponibilizada pelo instalador.
- Conversão de finais de linha: utilizar a opção recomendada para Windows, normalmente checkout CRLF e commit LF.
- Terminal: Git Bash é uma opção útil, sem impedir o uso do PowerShell.
- Comportamento de `git pull`: manter o padrão recomendado, que pode ser ajustado posteriormente.

As opções podem variar entre versões do instalador.

Não é necessário decorar todas elas agora. O importante é compreender que essas escolhas influenciam o comportamento do Git no ambiente local.

Finalize a instalação.

### 6.5. Validar o Git

Abra um novo PowerShell:

```powershell
git --version
```

O resultado deverá apresentar uma versão instalada.

Exemplo:

```text
git version 2.x.x
```

A numeração exata poderá ser diferente.

### 6.6. Abrir o Git Bash

O Git for Windows também disponibiliza o Git Bash.

Ele oferece um terminal com comandos e comportamento semelhantes aos ambientes Unix.

Para abrir:

1. Abra o menu Iniciar.
2. Pesquise por Git Bash.
3. Execute a aplicação.

Utilizaremos principalmente PowerShell e, quando necessário, Git Bash.

---

## 7. Checklist de instalação

Antes de avançar, confirme cada item:

- [ ] Windows 10 identificado e arquitetura verificada.
- [ ] Estrutura de diretórios criada.
- [ ] Eclipse Temurin JDK 25 instalado.
- [ ] `java -version` executado.
- [ ] `javac -version` executado.
- [ ] IntelliJ IDEA instalado.
- [ ] IntelliJ IDEA iniciado ao menos uma vez.
- [ ] Git for Windows instalado.
- [ ] `git --version` executado.
- [ ] Git Bash disponível.

## 8. Resultado esperado

Ao concluir esta etapa, teremos as principais ferramentas instaladas.

Ainda não estamos prontos para considerar o ambiente completamente configurado.

Precisamos configurar:

- `JAVA_HOME`;
- `PATH`;
- Git global;
- Identidade de commits;
- Chave SSH;
- Autenticação com GitHub;
- Integração do JDK com o IntelliJ IDEA.

Esses assuntos serão tratados em:

`environment-configuration/README.md`

---

## 9. Fontes oficiais

- [Eclipse Adoptium — Temurin Downloads](https://adoptium.net/temurin/releases/?version=25)
- [Oracle — Java Downloads](https://www.oracle.com/java/technologies/downloads/)
- [Dev.java — Learn Java](https://dev.java/)
- [JetBrains — IntelliJ IDEA](https://www.jetbrains.com/idea/download/)
- [Git — Install for Windows](https://git-scm.com/install/windows)
- [GitHub — Install Git](https://github.com/git-guides/install-git)

---

**Fim da etapa 01 — Installation.**