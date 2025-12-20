# Passo a Passo: Conectar Supabase ao Power BI

Este guia detalha o processo completo para conectar o Supabase PostgreSQL ao Power BI Desktop, incluindo a resolução do erro de certificado SSL.

## 📋 Visão Geral do Processo

```mermaid
flowchart TD
    A[Iniciar] --> B{Lembra da Senha do Banco?}
    B -->|Não| C[Resetar Senha]
    B -->|Sim| D[Obter Informações do Supabase]
    C --> D
    D --> E[Abrir Power BI Desktop]
    E --> F[Configurar Conexão PostgreSQL]
    F --> G{Erro de Certificado?}
    G -->|Sim| H[Desmarcar Criptografia]
    G -->|Não| I[Configurar Credenciais]
    H --> I
    I --> J[Selecionar Tabelas]
    J --> K[Carregar Dados]
    K --> L[Concluído]
    
    style A fill:#e1f5ff
    style L fill:#c8e6c9
    style B fill:#fff9c4
    style C fill:#ffccbc
    style G fill:#fff9c4
    style H fill:#ffccbc
```

## 🔐 Passo 0: Validar Senha do Banco de Dados

### 0.1 Verificar se Você Lembra da Senha

```mermaid
flowchart TD
    A[Iniciar Processo] --> B{Lembra da Senha do Banco?}
    B -->|Sim| C[Senha Disponível]
    B -->|Não| D[Precisa Resetar Senha]
    
    C --> E[Continuar para Passo 1]
    D --> F[Acessar Database Settings]
    F --> G[Resetar Senha]
    G --> H[Anotar Nova Senha]
    H --> E
    
    style A fill:#e1f5ff
    style B fill:#fff9c4
    style D fill:#ffccbc
    style E fill:#c8e6c9
```

**⚠️ IMPORTANTE:** Antes de começar, certifique-se de que você tem a senha do banco de dados que foi cadastrada quando você registrou o projeto no Supabase.

**Se você NÃO lembra da senha:**

1. Acesse o link abaixo (substitua `[seu-project-ref]` pelo seu project-ref do Supabase):
   ```
   https://supabase.com/dashboard/project/[seu-project-ref]/database/settings
   ```

2. Role até a seção **"Database password"**

3. Clique em **"Reset database password"**

4. Defina uma nova senha e **anote-a em local seguro**

5. Após resetar a senha, continue para o Passo 1

**Se você JÁ tem a senha:**

- Continue diretamente para o Passo 1

## 🔍 Passo 1: Obter Informações de Conexão no Supabase

### 1.1 Acessar o Editor do Supabase

```mermaid
flowchart LR
    A[Acessar URL do Editor] --> B[Página do Editor Carregada]
    B --> C[Botão Connect Visível]
    C --> D[Abas de Conexão]
    
    style A fill:#e1f5ff
    style D fill:#c8e6c9
```

**Ações:**
1. Acesse a URL do editor do seu projeto (substitua `[seu-project-ref]` pelo seu project-ref):
   ```
   https://supabase.com/dashboard/project/[seu-project-ref]/editor/[id]?schema=public&showConnect=true
   ```
   
   **Nota:** Você também pode acessar pelo dashboard:
   - Acesse [Supabase Dashboard](https://app.supabase.com)
   - Selecione seu projeto
   - Navegue até o **SQL Editor** ou **Table Editor**
   - O botão **"Connect"** estará visível no canto superior direito

### 1.2 Obter String de Conexão

```mermaid
flowchart TD
    A[Botão Connect Visível] --> B[Clicar em Connect]
    B --> C[Dialog: Connect to your project]
    C --> D[Selecionar Tab: Connection String]
    D --> E[Configurar Type: URI]
    E --> F[Configurar Source: Primary Database]
    F --> G[Alterar Method: Transaction pooler]
    G --> H[Clicar em View parameters]
    H --> I[Anotar Parâmetros]
    
    I --> J["Host: seu-host.pooler.supabase.com"]
    I --> K[Port: 6543]
    I --> L[Database: postgres]
    I --> M["User: postgres.seu-project-ref"]
    I --> N[Password: Senha do projeto]
    
    J --> O[Informações Anotadas]
    K --> O
    L --> O
    M --> O
    N --> O
    
    style A fill:#e1f5ff
    style G fill:#fff9c4
    style H fill:#fff9c4
    style I fill:#c8e6c9
    style O fill:#c8e6c9
```

**Passo a passo detalhado:**

1. **Clicar em Connect:**
   - No canto superior direito da página do editor, clique no botão **"Connect"**

2. **Selecionar a aba Connection String:**
   - No dialog que abrir, você verá várias abas no topo
   - Clique na aba **"Connection String"** (primeira aba)

3. **Configurar Type:**
   - No dropdown **"Type"**, selecione **"URI"**

4. **Configurar Source:**
   - No dropdown **"Source"**, selecione **"Primary Database"**

5. **Alterar Method para Transaction pooler:**
   - No dropdown **"Method"**, altere para **"Transaction pooler"**
   - ⚠️ **IMPORTANTE:** Use sempre "Transaction pooler" para melhor performance

6. **Visualizar Parâmetros:**
   - Clique no botão **"View parameters"** para expandir e ver os detalhes da conexão

7. **Anotar as Informações:**
   - Anote os seguintes parâmetros que aparecerão:
     - **Host:** `[seu-host].pooler.supabase.com` (exemplo: `aws-1-us-east-2.pooler.supabase.com`)
     - **Port:** `6543` (Transaction pooler)
     - **Database:** `postgres`
     - **User:** `postgres.[seu-project-ref]` (formato: `postgres.[project-ref]`)
     - **Password:** Use a senha que você validou no Passo 0 (ou resetou se necessário)

**Informações a anotar:**
- ✅ **Host:** `[seu-host].pooler.supabase.com` (exemplo: `aws-1-us-east-2.pooler.supabase.com`)
- ✅ **Porta:** `6543` (Transaction pooler)
- ✅ **Database:** `postgres`
- ✅ **User:** `postgres.[seu-project-ref]` (formato: `postgres.[project-ref]`)
- ✅ **Password:** A senha do banco de dados (validada no Passo 0)


## 💻 Passo 2: Configurar Conexão no Power BI Desktop

### 2.1 Abrir Power BI Desktop e Iniciar Conexão

```mermaid
flowchart TD
    A[Abrir Power BI Desktop] --> B[Página Inicial Tab]
    B --> C[Clicar em Obter Dados]
    C --> D[Buscar: post]
    D --> E[Selecionar: PostgreSQL database]
    E --> F[Clicar em Conectar]
    
    style A fill:#e1f5ff
    style E fill:#fff9c4
    style F fill:#c8e6c9
```

**Ações:**
1. Abra o **Power BI Desktop**
2. Na aba **"Página Inicial"** (Home), clique em **"Obter dados"** (Get Data)
3. Na busca, digite **"post"**
4. Selecione **"PostgreSQL database"**
5. Clique em **"Conectar"** (Connect)

### 2.2 Configurar Servidor e Banco de Dados

```mermaid
flowchart TD
    A[Dialog: Banco de dados PostgreSQL] --> B[Preencher Servidor]
    B --> B1["Servidor: seu-host.pooler.supabase.com"]
    B1 --> C[Preencher Banco de Dados]
    C --> C1[Banco de Dados: postgres]
    C1 --> D[Selecionar Modo: Importar]
    D --> E[Clicar em OK]
    
    style A fill:#e1f5ff
    style D fill:#fff9c4
    style E fill:#c8e6c9
```

**Configurações:**
- **Servidor:** `[seu-host].pooler.supabase.com` (use o host que você anotou do Supabase)
- **Banco de Dados:** `postgres`
- **Modo de Conectividade de Dados:** Selecione **"Importar"** (Import)
  - ⚠️ **Importante:** Use "Importar" para melhor performance, não "DirectQuery"

### 2.3 Configurar Credenciais

```mermaid
flowchart TD
    A[Dialog: Autenticação] --> B[Preencher Nome do usuário]
    B --> B1["Usuário: postgres.seu-project-ref"]
    B1 --> C[Preencher Senha]
    C --> C1[Senha: Senha do projeto]
    C1 --> D[Selecionar Nível]
    D --> D1["Nível: seu-host.pooler.supabase.com"]
    D1 --> E[Clicar em Conectar]
    
    style A fill:#e1f5ff
    style E fill:#c8e6c9
```

**Configurações:**
- **Nome do usuário:** `postgres.[seu-project-ref]` (use o user que você anotou do Supabase)
- **Senha:** Digite a senha do banco de dados
- **Selecione o nível:** Deixe o servidor selecionado (ex: `[seu-host].pooler.supabase.com`)

## ⚠️ Passo 3: Resolver Erro de Certificado SSL

### 3.1 Identificar o Erro

```mermaid
flowchart TD
    A[Tentar Conectar] --> B{Erro de Certificado?}
    B -->|Sim| C[Erro: Certificado remoto inválido]
    B -->|Não| D[Prosseguir para Seleção de Tabelas]
    
    C --> E[Ir para Configurações de Fonte]
    
    style B fill:#fff9c4
    style C fill:#ffccbc
    style D fill:#c8e6c9
```

**Erro esperado:**
```
Não é possível estabelecer conexão
Encontramos um erro ao tentar conectar.
Detalhes: 'O certificado remoto é inválido, de acordo com o procedimento de validação.'
```

### 3.2 Acessar Configurações de Fonte de Dados

```mermaid
flowchart TD
    A[Página Inicial Tab] --> B[Clicar em Transformar dados]
    B --> C[Menu Dropdown]
    C --> D[Selecionar: Configurações da fonte de dados]
    
    style A fill:#e1f5ff
    style D fill:#fff9c4
```

**Ações:**
1. Na aba **"Página Inicial"** (Home)
2. Clique em **"Transformar dados"** (Transform data) - botão com ícone de tabela e lápis
3. No menu dropdown, selecione **"Configurações da fonte de dados"** (Data source settings)

### 3.3 Editar Permissões

```mermaid
flowchart TD
    A[Dialog: Configurações da fonte de dados] --> B[Selecionar: Permissões globais]
    B --> C[Selecionar fonte PostgreSQL]
    C --> C1["Fonte: seu-host.pooler.supabase.com"]
    C1 --> D[Clicar em Editar Permissões]
    D --> E[Dialog: Editar Permissões]
    
    style A fill:#e1f5ff
    style D fill:#fff9c4
    style E fill:#c8e6c9
```

**Ações:**
1. No dialog **"Configurações da fonte de dados"**
2. Selecione o radio button **"Permissões globais"** (Global permissions)
3. Na lista, selecione sua fonte PostgreSQL (ex: `[seu-host].pooler.supabase.com`)
4. Clique em **"Editar Permissões..."** (Edit Permissions...)

### 3.4 Desmarcar Criptografia

```mermaid
flowchart TD
    A[Dialog: Editar Permissões] --> B[Seção: Criptografia]
    B --> C[Desmarcar: Criptografar conexões]
    C --> D[Clicar em OK]
    D --> E[Fechar Configurações]
    
    style A fill:#e1f5ff
    style C fill:#ffccbc
    style D fill:#c8e6c9
```

**Ações:**
1. No dialog **"Editar Permissões"**
2. Na seção **"Criptografia"** (Encryption)
3. **DESMARQUE** a opção **"Criptografar conexões"** (Encrypt connections)
   - ⚠️ **IMPORTANTE:** Esta é a solução para o erro de certificado SSL
4. Clique em **"OK"**
5. Feche o dialog de configurações

### 3.5 Tentar Conectar Novamente

```mermaid
flowchart TD
    A[Repetir Passo 2.3] --> B[Preencher Credenciais]
    B --> C[Clicar em Conectar]
    C --> D{Conexão Bem-sucedida?}
    D -->|Sim| E[Prosseguir para Seleção de Tabelas]
    D -->|Não| F[Verificar Credenciais]
    F --> A
    
    style D fill:#fff9c4
    style E fill:#c8e6c9
    style F fill:#ffccbc
```

**Ações:**
1. Repita o **Passo 2.3** (Configurar Credenciais)
2. Preencha usuário e senha novamente
3. Clique em **"Conectar"**
4. Se ainda houver erro, verifique:
   - Credenciais corretas
   - Host correto
   - Senha resetada (se necessário)

## 📊 Passo 4: Selecionar e Carregar Tabelas

### 4.1 Navegador de Dados

```mermaid
flowchart TD
    A[Conexão Bem-sucedida] --> B[Navegador de Dados]
    B --> C[Expandir Schema: public]
    C --> D[Selecionar Tabelas]
    
    D --> E[fato_lancamentos]
    D --> F[categorias_hierarquia]
    D --> G[contas]
    D --> H[fato_cartoes]
    D --> I[dim_cartoes]
    
    E --> J[Clicar em Carregar]
    F --> J
    G --> J
    H --> J
    I --> J
    
    style A fill:#c8e6c9
    style J fill:#fff9c4
```

**Tabelas Recomendadas:**
- ✅ `fato_lancamentos` - Tabela fato principal
- ✅ `categorias_hierarquia` - Dimensão de categorias
- ✅ `contas` - Dimensão de contas
- ✅ `fato_cartoes` - Tabela fato de cartões (se existir)
- ✅ `dim_cartoes` - Dimensão de cartões (se existir)

### 4.2 Carregar Dados

```mermaid
flowchart TD
    A[Selecionar Tabelas] --> B[Clicar em Carregar]
    B --> C[Power BI Processa Dados]
    C --> D{Dados Carregados?}
    D -->|Sim| E[Verificar Painel de Dados]
    D -->|Não| F[Verificar Erros]
    F --> G[Verificar Conexão]
    G --> A
    
    style B fill:#fff9c4
    style E fill:#c8e6c9
    style F fill:#ffccbc
```

**Ações:**
1. Marque as tabelas que deseja importar
2. Clique em **"Carregar"** (Load) ou **"Transformar dados"** (Transform data) se precisar fazer transformações
3. Aguarde o Power BI processar e carregar os dados
4. Verifique o painel **"Dados"** (Data) à direita para confirmar que as tabelas foram carregadas

## ✅ Resumo do Processo Completo

```mermaid
sequenceDiagram
    participant U as Usuário
    participant S as Supabase Dashboard
    participant PBI as Power BI Desktop
    
    U->>U: 0. Validar senha do banco
    alt Senha não lembrada
        U->>S: 0.1. Acessar Database Settings
        S->>U: 0.2. Página de configurações
        U->>S: 0.3. Resetar senha
        S->>U: 0.4. Nova senha definida
    end
    U->>S: 1. Acessar Editor (URL com showConnect=true)
    S->>U: 2. Página do editor carregada
    U->>S: 3. Clicar em Connect
    S->>U: 4. Dialog: Connect to your project
    U->>S: 5. Selecionar Tab: Connection String
    U->>S: 6. Configurar: Type=URI, Source=Primary Database
    U->>S: 7. Alterar Method: Transaction pooler
    U->>S: 8. Clicar em View parameters
    S->>U: 9. Parâmetros exibidos (Host, Port, User, etc.)
    U->>PBI: 10. Abrir Power BI Desktop
    U->>PBI: 11. Obter Dados > PostgreSQL
    PBI->>U: 12. Dialog de configuração
    U->>PBI: 13. Preencher Host e Database
    PBI->>U: 14. Dialog de credenciais
    U->>PBI: 15. Preencher usuário e senha
    PBI->>S: 16. Tentar conectar
    S-->>PBI: 17. Erro: Certificado inválido
    U->>PBI: 18. Configurações > Editar Permissões
    U->>PBI: 19. Desmarcar Criptografia
    U->>PBI: 20. Tentar conectar novamente
    PBI->>S: 21. Conectar (sem criptografia)
    S-->>PBI: 22. Conexão bem-sucedida
    PBI->>U: 23. Navegador de dados
    U->>PBI: 24. Selecionar tabelas
    PBI->>S: 25. Carregar dados
    S-->>PBI: 26. Dados carregados
    PBI->>U: 27. Dados prontos para uso
```

## 📝 Checklist de Configuração

Use este checklist para garantir que todos os passos foram seguidos:

### Validação Inicial
- [ ] Senha do banco validada (lembrada ou resetada)
- [ ] URL do editor acessada
- [ ] Botão Connect localizado

### Informações do Supabase
- [ ] Tab "Connection String" selecionada
- [ ] Type configurado: URI
- [ ] Source configurado: Primary Database
- [ ] Method alterado: Transaction pooler
- [ ] View parameters clicado
- [ ] Host anotado (ex: `[seu-host].pooler.supabase.com`)
- [ ] Porta anotada (`6543` para pooler)
- [ ] Database anotado (`postgres`)
- [ ] Usuário anotado (ex: `postgres.[seu-project-ref]`)
- [ ] Senha confirmada

### Configuração no Power BI
- [ ] Power BI Desktop aberto
- [ ] Conexão PostgreSQL iniciada
- [ ] Servidor configurado corretamente
- [ ] Database configurado (`postgres`)
- [ ] Modo selecionado: **Importar**
- [ ] Credenciais preenchidas
- [ ] Criptografia **DESMARCADA** (se houver erro)
- [ ] Conexão bem-sucedida
- [ ] Tabelas selecionadas
- [ ] Dados carregados

## 🔧 Troubleshooting

### Erro: "Certificado remoto inválido"

**Solução:**
1. Vá em **Transformar dados** > **Configurações da fonte de dados**
2. Selecione **Permissões globais**
3. Selecione sua fonte PostgreSQL
4. Clique em **Editar Permissões**
5. **DESMARQUE** "Criptografar conexões"
6. Clique em **OK**
7. Tente conectar novamente

### Erro: "Falha na autenticação"

**Soluções:**
1. Verifique se o usuário está no formato correto: `postgres.[project-ref]`
2. Verifique se a senha está correta
3. Se necessário, resete a senha em:
   ```
   https://supabase.com/dashboard/project/[seu-project-ref]/database/settings
   ```

### Erro: "Não é possível conectar ao servidor"

**Soluções:**
1. Verifique se o host está correto
2. Verifique sua conexão com a internet
3. Tente usar o Connection Pooler (porta 6543) em vez da Session mode (porta 5432)
4. Verifique se não há firewall bloqueando a conexão

## 📚 Referências

- [Documentação Supabase - Connection Pooling](https://supabase.com/docs/guides/database/connecting-to-postgres#connection-pooler)
- [Documentação Power BI - PostgreSQL](https://learn.microsoft.com/en-us/power-bi/connect-data/desktop-connect-to-postgresql-database)
- [Supabase Dashboard - Database Settings](https://supabase.com/dashboard/project/[seu-project-ref]/database/settings)

## ⚠️ Notas Importantes

1. **Criptografia Desmarcada:** Desmarcar "Criptografar conexões" resolve o erro de certificado SSL, mas reduz a segurança da conexão. Para produção, considere usar Power BI Gateway.

2. **Modo Importar vs DirectQuery:**
   - **Importar:** Melhor performance, dados são copiados para o Power BI
   - **DirectQuery:** Dados sempre atualizados, mas consultas mais lentas

3. **Connection Pooler:** Use sempre o Connection Pooler (porta 6543) para melhor performance e gerenciamento de conexões.

4. **Senha:** A senha do banco é a mesma que você definiu ao criar o projeto no Supabase. Se não lembrar, use o link de reset.

---

**Última atualização:** Guia genérico para conexão Supabase + Power BI

**Nota sobre URLs:**
- URL do Editor: `https://supabase.com/dashboard/project/[seu-project-ref]/editor/[id]?schema=public&showConnect=true`
- URL para Reset de Senha: `https://supabase.com/dashboard/project/[seu-project-ref]/database/settings`

