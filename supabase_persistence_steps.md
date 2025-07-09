## Passos para Implementar Persistência com Supabase

Para integrar a persistência de dados da sua aplicação com o Supabase, siga os passos abaixo:

### 1. Configuração do Projeto Supabase

*   **Crie um Novo Projeto no Supabase**: Acesse o [Supabase Dashboard](https://app.supabase.com/) e crie um novo projeto. Escolha uma região e defina uma senha forte para o banco de dados.

*   **Obtenha as Credenciais**: Após a criação do projeto, vá em `Project Settings` > `API` para encontrar a `Project URL` e a `anon public` `key`. Você precisará dessas informações para inicializar o cliente Supabase na sua aplicação.

*   **Crie as Tabelas no Banco de Dados**: No Supabase Dashboard, vá em `Table Editor` e crie as tabelas que espelham a estrutura de dados atual da sua aplicação. As tabelas devem ser:
    *   `minhas_tarefas`
        *   `id` (UUID ou INTEGER, PRIMARY KEY)
        *   `titulo` (TEXT)
        *   `descricao` (TEXT)
        *   `concluida` (BOOLEAN)
        *   `data_criacao` (TEXT ou TIMESTAMP WITH TIME ZONE)
        *   `ordem` (INTEGER)
    *   `favoritos`
        *   `id` (UUID ou INTEGER, PRIMARY KEY)
        *   `pessoa` (TEXT)
        *   `tarefa` (TEXT)
    *   `tarefas_por_pessoa`
        *   `id` (UUID ou INTEGER, PRIMARY KEY)
        *   `pessoa` (TEXT)
        *   `tarefa` (TEXT)
    *   `accordion_order`
        *   `id` (UUID ou INTEGER, PRIMARY KEY)
        *   `accordion_id` (TEXT)
        *   `position` (INTEGER)

    Certifique-se de configurar as `Row Level Security (RLS)` para cada tabela. Inicialmente, você pode desativar o RLS para testes, mas é crucial ativá-lo e configurar políticas adequadas para a segurança da sua aplicação em produção.

### 2. Instalação do Cliente Supabase

Como sua aplicação é baseada em HTML/JavaScript puro, você pode incluir a biblioteca Supabase via CDN no seu `index.html`:

```html
<script src="https://cdn.jsdelivr.net/npm/@supabase/supabase-js@2"></script>
```

### 3. Inicialização do Cliente Supabase

No seu arquivo JavaScript (ou dentro da tag `<script>` no `index.html`), inicialize o cliente Supabase com suas credenciais:

```javascript
const SUPABASE_URL = 'SUA_PROJECT_URL';
const SUPABASE_ANON_KEY = 'SUA_ANON_PUBLIC_KEY';

const supabase = Supabase.createClient(SUPABASE_URL, SUPABASE_ANON_KEY);
```

### 4. Migração dos Dados Existentes (Opcional)

Se você já tem dados no `localStorage` ou SQLite local, precisará de uma lógica para migrá-los para o Supabase na primeira vez que o usuário abrir a aplicação com a nova integração. Isso pode ser feito lendo os dados locais e inserindo-os no Supabase.

### 5. Operações CRUD com Supabase

Substitua suas funções atuais de `saveMinhasTarefas()`, `saveFavoritos()`, `saveTarefasPorPessoa()`, `saveAccordionOrder()` e `loadFromSQLite()` por chamadas à API do Supabase.

**Exemplos:**

*   **Inserir Dados (Ex: Nova Tarefa)**:

    ```javascript
    async function adicionarTarefaSupabase(titulo, descricao) {
      const { data, error } = await supabase
        .from('minhas_tarefas')
        .insert([
          { 
            titulo: titulo, 
            descricao: descricao, 
            concluida: false, 
            data_criacao: new Date().toISOString(),
            ordem: minhasTarefas.length // Ou lógica de ordenação mais robusta
          }
        ])
        .select();

      if (error) {
        console.error('Erro ao adicionar tarefa:', error);
        showStatus('Erro ao adicionar tarefa no Supabase', true);
      } else {
        console.log('Tarefa adicionada:', data);
        // Atualize seu array local 'minhasTarefas' com o 'data' retornado
        showStatus('Tarefa adicionada com sucesso no Supabase!');
      }
    }
    ```

*   **Ler Dados (Ex: Carregar Minhas Tarefas)**:

    ```javascript
    async function loadMinhasTarefasSupabase() {
      const { data, error } = await supabase
        .from('minhas_tarefas')
        .select('*')
        .order('ordem', { ascending: true }); // Ordenar se a coluna 'ordem' existir

      if (error) {
        console.error('Erro ao carregar tarefas:', error);
        showStatus('Erro ao carregar tarefas do Supabase', true);
        return [];
      } else {
        console.log('Tarefas carregadas:', data);
        return data.map(row => ({
          id: row.id,
          titulo: row.titulo,
          descricao: row.descricao,
          concluida: row.concluida,
          dataCreacao: new Date(row.data_criacao).toLocaleDateString("pt-BR"),
          ordem: row.ordem
        }));
      }
    }
    ```

*   **Atualizar Dados (Ex: Concluir Tarefa)**:

    ```javascript
    async function toggleConcluidaSupabase(id, concluida) {
      const { data, error } = await supabase
        .from('minhas_tarefas')
        .update({ concluida: concluida })
        .eq('id', id)
        .select();

      if (error) {
        console.error('Erro ao atualizar tarefa:', error);
        showStatus('Erro ao atualizar tarefa no Supabase', true);
      } else {
        console.log('Tarefa atualizada:', data);
        showStatus('Tarefa atualizada com sucesso no Supabase!');
      }
    }
    ```

*   **Excluir Dados (Ex: Excluir Tarefa)**:

    ```javascript
    async function excluirTarefaSupabase(id) {
      const { error } = await supabase
        .from('minhas_tarefas')
        .delete()
        .eq('id', id);

      if (error) {
        console.error('Erro ao excluir tarefa:', error);
        showStatus('Erro ao excluir tarefa no Supabase', true);
      } else {
        console.log('Tarefa excluída com sucesso');
        showStatus('Tarefa excluída com sucesso do Supabase!');
      }
    }
    ```

### 6. Sincronização e Offline (Considerações Avançadas)

O Supabase oferece recursos de Realtime para sincronização de dados em tempo real. Para uma experiência offline-first mais robusta, você pode considerar:

*   **Supabase Realtime**: Use as subscriptions do Supabase para ouvir mudanças no banco de dados e atualizar a UI em tempo real.
*   **Soluções Offline-First**: Para cenários complexos de offline, onde os usuários precisam operar por longos períodos sem conexão, e sincronizar dados quando online, você pode integrar com bibliotecas como [PowerSync](https://www.powersync.com/) ou [ElectricSQL](https://electric-sql.com/), que fornecem uma camada de sincronização entre um banco de dados local (como SQLite ou IndexedDB) e o Supabase Postgres.

### 7. Segurança (Row Level Security - RLS)

É **fundamental** configurar as políticas de Row Level Security (RLS) no seu banco de dados Supabase para controlar quem pode ler, inserir, atualizar e excluir dados. Isso garante que os usuários só possam acessar os dados aos quais têm permissão, especialmente quando a autenticação for implementada.

Ao seguir esses passos, você poderá migrar a persistência de dados da sua aplicação para o Supabase, aproveitando seus recursos de backend como serviço.

