Algumas notas sobre ensino da arquitetura
=========================================

## Abordagem para ensino

1. Usar a biblioteca como utilitário
2. Conceito mais usado: Persistência de dados
3. Vários conceitos envolvidos
4. Estrutura da solução .NET

### Usar a biblioteca como utilitário

- Checker
- Hash
- Enum
- etc.

### Conceito mais usado: Persistência de dados

- Repository
- Storage, Store, Alias
- Transaction, TransactionScope e Unit Of Work
- Não dependência do Linq. O mundo além do EntityFramework
- Conceitos e nomes padronizados
- IEnumerable vs IQueryable

### Vários conceitos envolvidos

- DDD e a arte de falar (através de nomes) com os envolvidos
- Clean Architecture e suas camadas
- Camada cebola e a comunicação por interface
- Porque não uso interface de serviços de negócio?
- Lazy load
- Configuração/Opção/Preferências, Configuration/Option/Settings
- Extensões

### Estrutura da solução .NET

Projetos .NET tem duas visões bastante distintas: a _visão física_ no sistema de arquivos e a _visão virtual_ na
solução dentro da IDE. Procuro mantê-las o mais parecido, mas nem sempre é possível por uma série de
razões técnicas, normalmente envolvidas com o desenvolvimento multi-plataforma.

Fisicamente, nossa estrutura de diretórios se parece com isso:
```
. my-project-folder/
|
|-- artifacts/
|-- benchmarks/
|-- buid/
|-- dist/
|-- samples/
|-- src/
+-- test/
```

Aqui vale destacar:

* Os subdiretórios `artifacts` e `dist` são criados durante o processo de build e ignoradas em `.gitignore`
* O subdiretório `build` guarda os scripts ou projetos do processo de construção do aplicativo. Ex [Cake](https://cakebuild.net)
* No subdiretório `src` temos os códigos fontes do aplicativo em si
* No subdiretório `test` temos os códigos de testes do aplicativo

> Usamos aqui nomes sempre com caracteres *minúsculos*, e quando há nomes compostos separamos por 
> *traços* ou *sublinhados* ao invés de *espaços*. Isso evitará muita dor de cabeça se você usa vários sistemas operacionais
> como Windows, Linux e macOS, ou se usa algum script, seja no processo de construção ou outra situação.

Já a representação virtual vista na sua IDE preferencial (recomendo [JetBrains Rider](https://www.jetbrains.com/rider), [Visual Studio](https://visualstudio.com) ou [VS Code](https://code.visualstudio.com) com extensão [vscode-solution-explorer](https://github.com/fernandoescolar/vscode-solution-explorer)) é um pouco diferente e se parece com isso:
```
. MySolution.sln/
|
|-- Root/
|-- Source/
|   |-- Core/
|   |-- Infrastructure/
|   |   |-- Data/
|   |   |-- Gateway/
|   |   +-- Util/
|   +--UserInterface/
|      |-- App/
|      |-- Net/
|      |-- Tool/
|      +-- Worker/
+-- Test/
    |-- End2End/
    |-- Integration/
    +-- Unit/
```

Destacamos:

* No subdiretório `Root` guardamos os arquivos que estão na raiz do repositório e que não estão diretamente
  relacionados a um projeto da solução, mas que precisam ser editados da mesma forma.
  Ex: arquivos `.gitignore`, `.editorconfig`, `README.md`, `LICENSE`, etc.
* Em `Source` temos o código em si de nossos componentes de software
* Em `Test` temos o código de teste de nossos componentes

> Usamos aqui nomes em *PascalCase* porque já estamos na visão virtual da IDE, e dessa forma nossa estrutura
> se relaciona de forma homogênia tanto com os nomes de arquivos, classes e outros objetos manipulados pela IDE
> e que usam o mesmo padrão.
 
#### Um pouco mais sobre `Source/Core`

A parte central de nosso código é o negócio em si, ou seja, o código de nossos métodos e regras de negócio.
Aqui dividimos em dois principais componentes: **Domínio** e **Negócio**.

O **domínio** ou **domain** é o nosso componente que define o modelo e toda a abstração do negócio.
Se considerarmos um negócio ou empresa chamada **MinhaCompanhia** que tem um projeto chamado **MinhaLoja**,
podemos ter o componente de domínio chamado **MinhaCompanhia.MinhaLoja.Domain**.

O **negócio** ou **business** é nosso componente que cria os métodos de negócio e implementam as regras
em si. No caso o componente recebe o nome do próprio projeto em si, ou seja, **MinhaCompanhia.MinhaLoja**.

> Um caso específico que mudaria a nomenclatura seria onde o negócio é muito grande, e você precisasse
> de uma abstração que seria compartilhada entre várias outras soluções, então valeria a pena ter um 
> componente de software de negócio chamado **MinhaCompanhia.MinhaLoja.Business**, e esse ser compartilhado
> entre vários outros projetos, pois nesse caso o termo *Business* salienta seu significado quando usado
> por outros projetos.

#### Um pouco mais sobre `Source/Infrastructure`

Aqui ficam os códigos que implementam componentes auxiliares ao negócio em si, que formam sua infraestrutura.
Dividimos em três subcategorias/subdiretórios:

* `Data` - Ficam as implementações de persistência de dados que são pertencentes ao negócio.
  Ex: Repositórios de dados
* `Gateway` - Ficam as implementações de acesso a dados e/ou serviços externos, ou seja,
  que não são pertencentes ao negócio. Chamamos de portas/portões de acesso externo.
  Ex: Api de consulta CPF/CNPJ, serviços de disparo de e-mails e notificações, etc.
* `Util` - Ficam qualquer outro que não se enquadre nos dois anteriores. Chamamos de utilitários de
  infraestrutura.

#### Um pouco mais sobre `Source/UserInterface`

A interface com usuário pode ter várias formas: Um aplicativo móvel, um website ou api, um utilitário de linha
de comando, um serviço de rede de baixo nível, etc.
Aqui os dividimos em quatro subcategorias/subdiretórios:

* `App` - Qualquer utilitário com interface gráfica que será executada pelo usuário diretamente em seu
  computador. Ex: Aplicativos desktop ou para dispositivos móveis
* `Net` - Qualquer serviço de rede. Ex: Website, webapi, serviço TCP/IP, etc.
* `Tool` - Qualquer ferramenta utilitária que será executada pelo usuário diretamente em seu computador.
  Normalmente um executável de linha de comando
* `Worker` - Qualquer serviço que executará em segundo plano, sem a interação direta com o usuário, e
  que não habilite acesso pela rede.
  
> NOTA: Porque um **Worker** apesar de não ter interação direta do usuário está na categoria de
> interface com o usuário? R: Ainda continua fazendo uma interface entre o usuário e nosso negócio, só que
> o usuário aqui é o próprio sistema operacional.

#### Um pouco mais sobre `Test`

Dividimos nossos testes em:

* `End2End` - Testes fim a fim, ou seja, que simulam a utilização/navegação do usuário final
* `Integration` - Teste de integração entre nossos serviços de negócio e sua infraestrutura
* `Unit` - Teste das unidades isoladas de nosso negócio
