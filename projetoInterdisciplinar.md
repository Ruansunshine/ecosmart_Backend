# PROJETO INTERDISCIPLINAR: SISTEMA ECOSMART

---

## 1. DESCRIÇÃO DO PROJETO

O Sistema EcoSmart é uma aplicação desenvolvida em Java com arquitetura MVC (Model-View-Controller), focada em soluções sustentáveis e inteligentes. O projeto implementa uma plataforma completa para monitoramento e controle de dispositivos IoT em ambientes domésticos e corporativos, visando otimização energética e sustentabilidade ambiental.

---

## 2. TECNOLOGIAS UTILIZADAS

### 2.1 Stack Principal
- **Java**: Linguagem de programação principal
- **Spring Boot**: Framework para desenvolvimento de aplicações Java
- **Spring Framework**: Conjunto de ferramentas para desenvolvimento empresarial
- **Maven**: Gerenciador de dependências e estrutura de projeto

### 2.2 Dependências do Projeto
- **Spring Web**: Para criação de APIs REST e gerenciamento de rotas HTTP
- **Spring Data JPA**: Facilita operações no banco de dados com ORM (Hibernate)
- **Lombok**: Elimina código repetitivo como getters, setters e construtores
- **Validation**: Garante que dados recebidos ou enviados estão corretos
- **DevTools**: Proporciona hot reload - reinicia a aplicação automaticamente após alterações
- **MySQL Driver**: Permite a conexão do Spring com bancos de dados MySQL

---

## 3. ARQUITETURA E PADRÕES DE DESIGN

### 3.1 Padrão MVC (Model-View-Controller)
**Status**: ✅ **IMPLEMENTADO**

- **Model**: Define as entidades e modelos de dados do sistema
  - `Ambiente.java`: Entidade que representa ambientes monitorados
  - `Objeto.java`: Entidade que representa objetos IoT do sistema
  - `Relatorio.java`: Entidade para geração de relatórios
  - `Usuario.java`: Entidade que representa usuários do sistema
- **View**: Interface REST API (controladores retornam dados JSON)
- **Controller**: Gerencia requisições HTTP e coordena operações

### 3.2 Padrões de Design Implementados
- ✅ **MVC (Model-View-Controller)**: Separação clara entre camadas
- ✅ **Repository Pattern**: Camada de acesso a dados isolada
- ✅ **Service Layer Pattern**: Lógica de negócio centralizada
- ✅ **Dependency Injection**: Implementado através do Spring Framework
- ✅ **Configuration Pattern**: Configurações centralizadas (CorsConfig)
- ✅ **RESTful Pattern**: Controllers seguem padrões REST com endpoints padronizados

---

## 4. PADRÕES DE PROJETO IMPLEMENTADOS

### 4.1 Resumo dos Padrões por Categoria
**Total: 6 padrões de projeto implementados**

#### Padrões Criacionais: 2
- **Builder** - `Usuario.java`
- **Factory Method** - `Objeto.java`

#### Padrões Estruturais: 2
- **Facade** - `Ambiente.java`
- **Decorator** - `AmbienteService.java`

#### Padrões Comportamentais: 2
- **Strategy** - `UsuarioService.java`
- **Template Method** - `ObjetoService.java`

### 4.2 Padrão Builder (Criacional)
**Localização**: `Usuario.java`  
**Finalidade**: Permitir criação de objetos Usuario de forma mais legível e flexível com interface fluente

**Descrição**: O padrão Builder é um padrão de design criacional que permite construir objetos complexos passo a passo. Ele separa a construção de um objeto de sua representação, permitindo que o mesmo processo de construção possa criar diferentes representações do objeto.

**Implementação**:
```java
// Classe interna Builder para construção fluente
public static class UsuarioBuilder {
    private String nome;
    private String email;
    private String senha;
    
    public UsuarioBuilder nome(String nome) {
        this.nome = nome;
        return this;
    }
    
    public UsuarioBuilder email(String email) {
        this.email = email;
        return this;
    }
    
    public Usuario build() {
        Usuario usuario = new Usuario();
        usuario.nome = this.nome;
        usuario.email = this.email;
        return usuario;
    }
}

// Método estático para iniciar o builder
public static UsuarioBuilder builder() {
    return new UsuarioBuilder();
}
```

### 4.3 Padrão Factory Method (Criacional)
**Localização**: `Objeto.java`  
**Finalidade**: Criar objetos pré-configurados para diferentes tipos de dispositivos IoT com configurações específicas por categoria

**Descrição**: O padrão Factory Method é um padrão de design criacional que fornece uma interface para criar objetos em uma superclasse, mas permite que as subclasses alterem o tipo de objetos que serão criados. Neste caso, utiliza métodos estáticos para criar diferentes tipos de eletrodomésticos com configurações específicas.

**Implementação**:
```java
// Factory Method para criar Lâmpada
public static Objeto criarLampada(String nome, Integer potencia) {
    Objeto objeto = new Objeto();
    objeto.setTipoObjeto("LAMPADA");
    objeto.setNomeObjeto(nome);
    objeto.setPotencia(potencia);
    objeto.setStatus("DESLIGADO");
    objeto.setAtivo(1);
    return objeto;
}

// Factory Method para criar Geladeira
public static Objeto criarGeladeira(String nome, Integer potencia) {
    Objeto objeto = new Objeto();
    objeto.setTipoObjeto("GELADEIRA");
    objeto.setStatus("LIGADO"); // Configuração específica
    return objeto;
}

// Factory Method genérico
public static Objeto criarEletrodomestico(String nome, String tipo, Integer potencia) {
    Objeto objeto = new Objeto();
    objeto.setTipoObjeto(tipo.toUpperCase());
    // Configurações padrão...
    return objeto;
}
```

**Vantagens**: Encapsula a lógica de criação, facilita a manutenção, permite configurações específicas por tipo de dispositivo e melhora a legibilidade do código.

### 4.4 Padrão Facade (Estrutural)
**Localização**: `Ambiente.java`  
**Finalidade**: Simplificar operações complexas que envolvem múltiplos relacionamentos da entidade Ambiente

**Descrição**: O padrão Facade é um padrão de design estrutural que fornece uma interface simplificada para um subsistema complexo. Ele esconde a complexidade do sistema e fornece uma interface mais fácil de usar para o cliente, encapsulando as chamadas para múltiplos objetos em uma única interface.

**Implementação**:
```java
/**
 * Método Facade: Obtém informações completas do ambiente
 * Encapsula a complexidade de acessar múltiplos relacionamentos
 */
public String obterResumoCompleto() {
    StringBuilder resumo = new StringBuilder();
    resumo.append("Ambiente: ").append(nome);
    
    if (objetos != null && !objetos.isEmpty()) {
        resumo.append(" | Objetos: ").append(objetos.size());
    }
    
    if (usuarios != null && !usuarios.isEmpty()) {
        resumo.append(" | Usuários: ").append(usuarios.size());
    }
    
    return resumo.toString();
}

/**
 * Método Facade: Simplifica validações complexas
 */
public boolean isAmbienteCompleto() {
    return nome != null && !nome.trim().isEmpty() &&
           descricao != null && !descricao.trim().isEmpty();
}
```

**Vantagens**: Reduz a complexidade para o cliente, centraliza operações relacionadas, melhora a manutenibilidade e oferece uma interface mais limpa para operações complexas.

### 4.5 Padrão Strategy (Comportamental)
**Localização**: `UsuarioService.java`  
**Finalidade**: Definir diferentes estratégias de busca de usuários baseadas em critérios específicos, permitindo escolher o algoritmo em tempo de execução

**Descrição**: O padrão Strategy é um padrão de design comportamental que permite definir uma família de algoritmos, encapsulá-los e torná-los intercambiáveis. O Strategy permite que o algoritmo varie independentemente dos clientes que o utilizam, promovendo flexibilidade na escolha de diferentes comportamentos.

**Implementação**:
```java
/**
 * Interface Strategy para diferentes estratégias de busca
 */
public interface UsuarioBuscaStrategy {
    List<Usuario> buscar(String criterio);
}

/**
 * Strategy concreta: Busca por nome (contém)
 */
private class BuscaPorNomeStrategy implements UsuarioBuscaStrategy {
    @Override
    public List<Usuario> buscar(String nome) {
        return usuarioRepository.findByNomeContainingIgnoreCase(nome);
    }
}

/**
 * Context - usa as strategies
 */
private UsuarioBuscaStrategy buscaStrategy;

public void setBuscaStrategy(UsuarioBuscaStrategy strategy) {
    this.buscaStrategy = strategy;
}

public List<Usuario> buscarComStrategy(String criterio) {
    if (buscaStrategy == null) {
        buscaStrategy = new BuscaPorNomeStrategy(); // Strategy padrão
    }
    return buscaStrategy.buscar(criterio);
}
```

**Vantagens**: Permite alternar algoritmos dinamicamente, facilita a adição de novos tipos de busca, melhora a manutenibilidade e separa a lógica de diferentes estratégias.

### 4.6 Padrão Template Method (Comportamental)
**Localização**: `ObjetoService.java`  
**Finalidade**: Definir um algoritmo padrão para operações de busca com etapas customizáveis, garantindo consistência no logging e validação

**Descrição**: O padrão Template Method é um padrão de design comportamental que define o esqueleto de um algoritmo em uma superclasse, mas permite que subclasses sobrescrevam etapas específicas do algoritmo sem alterar sua estrutura. Neste caso, estabelece um fluxo padrão para todas as operações de busca.

**Implementação**:
```java
/**
 * Template Method - Define o algoritmo padrão para operações de busca
 * Etapas: validação -> log início -> execução -> log resultado -> retorno
 */
private <T> T executarOperacaoBusca(String operacao, Supplier<T> busca, Object... parametros) {
    // Etapa 1: Validação dos parâmetros (pode ser sobrescrita)
    validarParametrosBusca(parametros);
    
    // Etapa 2: Log do início da operação
    logInicioOperacao(operacao, parametros);
    
    // Etapa 3: Execução da busca (implementação específica)
    T resultado = busca.get();
    
    // Etapa 4: Log do resultado
    logResultadoOperacao(operacao, resultado);
    
    // Etapa 5: Retorno do resultado
    return resultado;
}

/**
 * Etapa customizável - Validação de parâmetros
 */
private void validarParametrosBusca(Object... parametros) {
    for (Object param : parametros) {
        if (param == null) {
            throw new IllegalArgumentException("Parâmetro de busca não pode ser nulo");
        }
        if (param instanceof String && ((String) param).trim().isEmpty()) {
            throw new IllegalArgumentException("Parâmetro de texto não pode estar vazio");
        }
    }
}

// Exemplo de uso do Template Method
public List<Objeto> buscarPorNome(String nomeObjeto) {
    return executarOperacaoBusca("buscarPorNome",
            () -> objetoRepository.findByNomeObjeto(nomeObjeto), nomeObjeto);
}
```

**Vantagens**: Garante consistência entre operações, centraliza logging e validação, facilita manutenção e permite customização de etapas específicas mantendo a estrutura geral.

### 4.7 Padrão Decorator (Estrutural)
**Localização**: `AmbienteService.java`  
**Finalidade**: Adicionar funcionalidades extras às operações existentes (logging, validação, performance, auditoria) sem modificar o código original, permitindo composição dinâmica de responsabilidades

**Descrição**: O padrão Decorator é um padrão de design estrutural que permite adicionar novos comportamentos a objetos colocando-os dentro de invólucros (wrappers) que contêm os comportamentos. Ele oferece uma alternativa flexível à herança para estender funcionalidades, permitindo que responsabilidades sejam adicionadas dinamicamente através de composição.

**Implementação**:
```java
/**
 * Interface base para decorar operações de Ambiente
 * Padrão Decorator - Define o contrato para decorators
 */
public interface AmbienteOperationDecorator {
    void executarAntes();
    void executarDepois();
}

/**
 * Decorator para adicionar logging às operações
 * Padrão Decorator - Implementação concreta para logging
 */
private class LoggingDecorator implements AmbienteOperationDecorator {
    private String operacao;
    private Object parametros;

    public LoggingDecorator(String operacao, Object parametros) {
        this.operacao = operacao;
        this.parametros = parametros;
    }

    @Override
    public void executarAntes() {
        System.out.println("[LOG] Iniciando operação: " + operacao + " com parâmetros: " + parametros);
    }

    @Override
    public void executarDepois() {
        System.out.println("[LOG] Operação concluída: " + operacao);
    }
}

/**
 * Factory method para criar decorators
 * Padrão Factory Method - centraliza a criação de decorators
 */
public AmbienteOperationDecorator criarDecorator(TipoDecorator tipo, Object... params) {
    switch (tipo) {
        case LOGGING:
            return new LoggingDecorator(
                    params.length > 0 ? params[0].toString() : "operacao",
                    params.length > 1 ? params[1] : null
            );
        case VALIDATION:
            return new ValidationDecorator(
                    params.length > 0 && params[0] instanceof Ambiente ? (Ambiente) params[0] : null
            );
        // Outros tipos...
        default:
            return new LoggingDecorator("default", null);
    }
}

/**
 * Exemplo de uso com múltiplos decorators
 */
public Ambiente atualizarComDecorators(Ambiente ambiente) {
    LoggingDecorator logDecorator = new LoggingDecorator("atualizar", ambiente.getIdAmbiente());
    ValidationDecorator validationDecorator = new ValidationDecorator(ambiente);
    PerformanceDecorator performanceDecorator = new PerformanceDecorator("atualizar");

    // Executar decorators antes
    logDecorator.executarAntes();
    validationDecorator.executarAntes();
    performanceDecorator.executarAntes();

    // Operação principal
    Ambiente resultado = ambienteRepository.save(ambiente);

    // Executar decorators depois (ordem inversa)
    performanceDecorator.executarDepois();
    validationDecorator.executarDepois();
    logDecorator.executarDepois();

    return resultado;
}
```

**Vantagens**: Permite adicionar responsabilidades dinamicamente sem alterar código existente, oferece alternativa flexível à herança, possibilita composição de múltiplas funcionalidades e mantém o princípio aberto/fechado do SOLID.

---

## 5. ESTRUTURA DO PROJETO

```
com.ecosmart.eco/
├── controller/     # Controladores REST - gerencia requisições HTTP
├── model/          # Entidades e modelos de dados
├── service/        # Lógica de negócio da aplicação  
├── repository/     # Acesso e persistência de dados
├── CorsConfig.java # Configuração de CORS para requisições cross-origin
└── EcosmartApplication.java # Classe principal Spring Boot
```

---

## 6. COMO EXECUTAR

[Instruções serão adicionadas]

---

## 7. FUNCIONALIDADES

[Lista de funcionalidades será preenchida]

---

*Documento em construção - aguardando análise do código*