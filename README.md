# Porque criei a [`Arquitetura E5R`][e5r]

Basicamente porque senti a necessidade de padronizar a forma como eu mesmo escrevia
código DotNet nos projetos que participo, além de organizar em um único local minhas
idéias para então, ou defendê-las em alguma discussão, ou simplesmente para compartilhá-las
e ter uma documentação mínima para outros então criticarem.

## Algumas notas sobre artigos espalhados na interner

De tudo que coloquei na arquitetura, se não tudo, mas a maioria vem de idéias de outras
pessoas espalhadas na Internet. Algumas das minhas idéias vão de acordo ou diretamente
contra essas idéias. Aqui deixo algumas notas sobre alguns artigos que li e se
concordo ou não, e os pontos com minhas notas.

Artigo: [Onion Architecture In .Net 5](https://medium.com/nerd-for-tech/onion-architecture-in-net-5-deb04efe9df0)
> por *Jay Krishna Reddy* <br />
> no https://medium.com/nerd-for-tech

* Neste artigo critico basicamente o fato de usarmos na camada de **Domínio** componentes de
infraestrutura. No exemplo do artigo acoplamos os componentes do *Entity Framework* na camada,
e no meu ponto de vista isso não é o ideal, pois a camada de domínio basicamente abstrai o
nosso modelo de dados de negócio.
* **BaseEntity.cs** é um ponto que vejo muita gente usando, mas de uma forma geral eu creio que
  não se aplica bem para vários campos além da identificação. No meu caso eu optei por haver uma
  interface base `IIdentifiable.cs` que cubra essa parte da identificação.

[e5r]: https://github.com/e5r/e5r.architecture
