## Passos para Implementar Autenticação com Supabase

Para integrar a autenticação de usuários na sua aplicação utilizando o Supabase, siga os passos abaixo:

### 1. Configuração do Projeto Supabase

*   **Crie um Novo Projeto no Supabase**: Se ainda não o fez, crie um projeto no [Supabase Dashboard](https://app.supabase.com/).

*   **Obtenha as Credenciais**: Em `Project Settings` > `API`, você encontrará a `Project URL` e a `anon public` `key`. Estas são as credenciais que sua aplicação usará para se comunicar com o Supabase.

*   **Configure os Métodos de Autenticação**: No Supabase Dashboard, vá em `Authentication` > `Settings`. Aqui você pode habilitar os métodos de autenticação que deseja oferecer aos seus usuários:
    *   **Email/Senha**: Habilite esta opção para permitir que os usuários se cadastrem e façam login com email e senha.
    *   **Magic Link**: Permite que os usuários façam login clicando em um link enviado para o email, sem a necessidade de senha.
    *   **OAuth (Social Logins)**: Configure provedores como Google, GitHub, Facebook, etc., para permitir login social. Você precisará configurar as credenciais de API para cada provedor.
    *   **Outros**: OTP (One-Time Password), SSO (Single Sign-On), etc.

*   **Configurar URLs de Redirecionamento**: Para OAuth e Magic Link, é crucial configurar as URLs de redirecionamento válidas em `Authentication` > `Settings` > `Site URL` e `Redirect URLs`. Isso garante que o Supabase redirecione o usuário de volta para sua aplicação após a autenticação.

### 2. Instalação e Inicialização do Cliente Supabase

Assim como na persistência, inclua a biblioteca Supabase via CDN no seu `index.html`:

```html
<script src="https://cdn.jsdelivr.net/npm/@supabase/supabase-js@2"></script>
```

E inicialize o cliente Supabase no seu JavaScript:

```javascript
const SUPABASE_URL = 'SUA_PROJECT_URL';
const SUPABASE_ANON_KEY = 'SUA_ANON_PUBLIC_KEY';

const supabase = Supabase.createClient(SUPABASE_URL, SUPABASE_ANON_KEY);
```

### 3. Implementação da Autenticação na Aplicação

Você precisará criar uma interface de usuário para cadastro (Sign Up) e login (Sign In). O Supabase oferece métodos simples para isso:

*   **Cadastro de Usuário (Sign Up)**:

    ```javascript
    async function signUpUser(email, password) {
      const { data, error } = await supabase.auth.signUp({
        email: email,
        password: password,
      });

      if (error) {
        console.error('Erro no cadastro:', error.message);
        showStatus('Erro no cadastro: ' + error.message, true);
      } else {
        console.log('Usuário cadastrado:', data.user);
        showStatus('Cadastro realizado! Verifique seu email para confirmar.', false);
        // Redirecionar ou mostrar mensagem de sucesso
      }
    }
    ```

*   **Login de Usuário (Sign In)**:

    *   **Com Email e Senha**:

        ```javascript
        async function signInWithEmail(email, password) {
          const { data, error } = await supabase.auth.signInWithPassword({
            email: email,
            password: password,
          });

          if (error) {
            console.error('Erro no login:', error.message);
            showStatus('Erro no login: ' + error.message, true);
          } else {
            console.log('Usuário logado:', data.user);
            showStatus('Login realizado com sucesso!', false);
            // Redirecionar para a página principal da aplicação
          }
        }
        ```

    *   **Com Magic Link (Login sem Senha)**:

        ```javascript
        async function signInWithMagicLink(email) {
          const { error } = await supabase.auth.signInWithOtp({
            email: email,
          });

          if (error) {
            console.error('Erro ao enviar Magic Link:', error.message);
            showStatus('Erro ao enviar Magic Link: ' + error.message, true);
          } else {
            showStatus('Magic Link enviado! Verifique seu email.', false);
          }
        }
        ```

    *   **Com Provedores OAuth (Social Logins)**:

        ```javascript
        async function signInWithOAuth(provider) { // 'google', 'github', etc.
          const { data, error } = await supabase.auth.signInWithOAuth({
            provider: provider,
            options: {
              redirectTo: window.location.origin, // Redireciona de volta para a URL atual da sua app
            },
          });

          if (error) {
            console.error('Erro no login OAuth:', error.message);
            showStatus('Erro no login com ' + provider + ': ' + error.message, true);
          }
          // O usuário será redirecionado para a página do provedor e depois de volta para sua app
        }
        ```

*   **Logout de Usuário (Sign Out)**:

    ```javascript
    async function signOutUser() {
      const { error } = await supabase.auth.signOut();

      if (error) {
        console.error('Erro ao fazer logout:', error.message);
        showStatus('Erro ao fazer logout: ' + error.message, true);
      } else {
        console.log('Usuário deslogado.');
        showStatus('Logout realizado com sucesso!', false);
        // Limpar estado do usuário na UI, redirecionar para página de login
      }
    }
    ```

### 4. Gerenciamento de Sessão e Estado do Usuário

O Supabase gerencia a sessão do usuário automaticamente (usando `localStorage` por padrão). Você pode ouvir eventos de autenticação para atualizar a UI da sua aplicação:

```javascript
supabase.auth.onAuthStateChange((event, session) => {
  console.log(event, session);
  if (event === 'SIGNED_IN') {
    // Usuário logado, atualize a UI
    console.log('Usuário logado:', session.user);
  } else if (event === 'SIGNED_OUT') {
    // Usuário deslogado, atualize a UI
    console.log('Usuário deslogado.');
  }
  // Outros eventos: 'INITIAL_SESSION', 'TOKEN_REFRESHED', 'USER_UPDATED', 'PASSWORD_RECOVERY'
});

// Para obter a sessão atual ao carregar a página
async function getSession() {
  const { data: { session }, error } = await supabase.auth.getSession();
  if (session) {
    console.log('Sessão atual:', session);
    // Faça algo com o usuário logado
  } else {
    console.log('Nenhum usuário logado.');
  }
}

getSession();
```

### 5. Segurança (Row Level Security - RLS)

Com a autenticação, o RLS se torna ainda mais crítico. Você usará as informações do usuário autenticado (via `auth.uid()`, `auth.role()`, etc.) nas suas políticas de RLS para controlar o acesso aos dados no banco de dados. Por exemplo, uma política pode permitir que um usuário veja apenas as tarefas que ele mesmo criou.

### 6. Considerações Adicionais

*   **Tratamento de Erros**: Sempre inclua tratamento de erros (`try...catch`) para as chamadas à API do Supabase.
*   **Validação de Formulários**: Valide os inputs do usuário (email, senha) no frontend antes de enviar para o Supabase.
*   **UI/UX**: Crie uma experiência de usuário clara para os fluxos de cadastro, login, recuperação de senha e logout.
*   **Variáveis de Ambiente**: Para produção, é uma boa prática armazenar suas chaves `SUPABASE_URL` e `SUPABASE_ANON_KEY` como variáveis de ambiente, e não diretamente no código fonte, embora para uma aplicação puramente frontend com a chave `anon public` isso seja menos crítico.

Ao seguir esses passos, você poderá implementar um sistema de autenticação robusto e seguro na sua aplicação utilizando o Supabase.

