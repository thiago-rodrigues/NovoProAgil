# NovoProAgil
Começando Novamente o Curso de Asp.Net Core Web API, Angular e EF Core

# Instalações

01. [.NetCore](https://dotnet.microsoft.com/download/dotnet-core)
02. [Angular](https://angular.io)
03. [Angular CLI](https://cli.angular.io)
04. [BootStrap](https://getbootstrap.com)
05. [FontAwesome](https://fontawesome.com)


# Arquivos Importantes

01. [appsettings.json]() - Configurações do BD ambiente de Produção
02. [appsettings.Development.json]() - Configurações do BD ambiente de Desenvolvimento

```
Exemplo:

"ConnectionStrings":{
    "DefaultConnection" : "Data Source=SFM.db"
  }

```
03. [Properties\launchSettings.json]() - Define se utiliza o arquivo de configuração do ambiente de produção ou desenvolvimento "Development" ou "Production" e algumas outras configurações do projeto.
04. [Startup.cs]() - Arquivos de configurações gerais do projeto WebApi - Ex: Liberação Cors, Redirecionamento https, configuração do services


Exemplo Config Context:
```
        public void ConfigureServices(IServiceCollection services)
        {
            services.AddDbContext<SFMContext>(
                x => x.UseSqlite(Configuration.GetConnectionString("DefaultConnection"))
            );
        }

```
Exemplo Config Interface:
```
        public void ConfigureServices(IServiceCollection services)
        {            
            services.AddScoped<IBancoRepository, BancoRepository>();
            services.AddScoped<IAgenciaRepository, AgenciaRepository>();
            services.AddScoped<IContaRepository, ContaRepository>();            
        }

```

# Configuração de Tracking e NoTracking

Exemplo de Config Geral no Repository:

```
        public ContaRepository(SFMContext context)
        {
            _context = context;
            _context.ChangeTracker.QueryTrackingBehavior = QueryTrackingBehavior.NoTracking;
        }
```
Exemplo de Config em Query:

```
            query = query.AsNoTracking()
                    .OrderByDescending(c => c.DataEvento)
                    .Where(c => c.Id == EventoId);

```



# Comandos Importantes
### DotNet

01. dotnet --list-sdks
02. dotnet --version
03. dotnet watch run
04. dotnet new webapi -n NomeProjeto
05. dotnet new classlib -n NomeProjeto
06. dotnet new sln -n NomeProjeto
07. dotnet add NomeProjeto1 reference NomeProjeto2
08. dotnet sln NomeSolution add NomeProjeto

### Angular

01. npm install -g @angular/cli
02. ng new my-dream-app
03. ng serve -o
04. ng serve

### BootStrap e FontAwesome

01. npm install bootstrap
02. npm install --save-dev @fortawesome/fontawesome-free

### EF - Entity FrameWork

01. dotnet ef --startup-project NomeProjetoWebApi migrations add init
02. dotnet ef --startup-project NomeProjetoWebApi database update

# Divisão de Responsabilidades

01. App Angular - Front End
02. WebApi Project
03. Domain ClassLib
04. Repository ClassLib
05. Referenciar Dominio no Repositorio
06. Referenciar Dominio e Repositorio na WebApi
07. Referenciar Dominio, Repositorio e WebApi no Arquivo de Solução

### Packge - ctrl+shift+P - Nuget Package Management
01. Microsoft.EntityFrameworkCore
02. Microsoft.EntityFrameworkCore.Sqlite


Em casos de relacionamento N=>N e chaves compostas deve-se sobrescrever o método OnModelCreating no Context para especificar as chaves
```    
    protected override void OnModelCreating(ModelBuilder modelBuilder){
        modelBuilder.Entity<PalestranteEvento>()
        .HasKey(PE => new{PE.EventoId, PE.PalestranteId});
    }
```
Outros Exemplos:
```
    modelBuilder.Entity<Banco>()
        .HasMany(A => A.Agencias)
        .WithOne(B => B.Banco)
        .HasForeignKey(A => new {A.BancoCodigo});
    
    modelBuilder.Entity<Agencia>()
        .HasMany(C => C.Contas)
        .WithOne(A => A.Agencia)
        .HasForeignKey(C => new {C.AgenciaNumero, C.AgenciaDigVerificador, C.BancoCodigo});

    modelBuilder.Entity<Agencia>()
        .HasKey(A => new {A.Numero, A.DigVerificador, A.BancoCodigo});  

    modelBuilder.Entity<Conta>()
        .HasKey(C => new {C.Numero, C.DigVerificador, C.AgenciaNumero, C.AgenciaDigVerificador, C.BancoCodigo});

```

### Extensões Visual Studio Code
01. Angular Files
02. Angular Language Service
03. Angular Snippets (Version x)
04. Angular2-Switcher
05. Auto Rename Tag
06. Bracket Pair Colorizer
07. C#
08. C# Extensions
09. Debugger for Chrome
10. Live Server
11. Material Icon Theme
12. Npm
13. Nuget Package Manager
14. Path Intellisense
15. Prettier - Code Formatter
16. TSLint
17. ERD Editor
