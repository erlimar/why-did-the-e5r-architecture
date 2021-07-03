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

- DDD e suas camadas
- Camada cebola e a comunicação por interface
- Porque não uso interface de serviços de negócio
- Lazy load
- Configuração/Opção/Preferência, Config/Option/Setting
- Extensões

### Estrutura da solução .NET

- Estrutura de diretório físico
```
artifacts/
buid/
dist/
src/
test/
```

- Estrutura de diretório da solução
```
Root/
Source/
  Core/
  Infrastructure/
    Data/
    Gateway/
    Util/
  UserInterface/
    Tool/
    Web/
    Worker/
Test
  UnitTest/
  IntegrationTest/
  End2EndTest/
```
