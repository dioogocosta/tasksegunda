# Tarefas Segunda - PWA

## Descrição
Progressive Web App (PWA) para gerenciamento de tarefas com persistência SQLite local.

## Funcionalidades Implementadas

### ✅ Melhorias Visuais
- **Título**: Alterado para "Tarefas Segunda"
- **Logo**: Logo geométrica discreta ao lado do título
- **Botão de Upload**: Estilizado como os demais botões da interface

### ✅ Funcionalidades CRUD Completas
- **Criar**: Adicionar novas tarefas em "Minhas Tarefas"
- **Ler**: Visualizar todas as tarefas organizadas por seções
- **Atualizar**: Editar título e descrição das tarefas
- **Excluir**: Remover tarefas com confirmação

### ✅ Funcionalidades Avançadas
- **Busca Inteligente**: Filtra todos os colapses incluindo "Minhas Tarefas"
- **Ícone de Lixeira (🗑️)**: Exclusão de tarefas em todas as seções
- **Ícone de Confirmação (✔️)**: Move tarefas dos favoritos para "Minhas Tarefas"
- **Favoritos**: Sistema de estrelas para marcar tarefas importantes

### ✅ PWA (Progressive Web App)
- **Manifest**: Configuração para instalação como app
- **Service Worker**: Cache offline e funcionalidade sem internet
- **Instalação**: Botão automático para instalar no dispositivo
- **Ícones**: Logo otimizada para diferentes tamanhos

### ✅ Persistência SQLite
- **Banco Local**: SQLite WASM para armazenamento robusto
- **Backup/Restore**: Exportar e importar banco de dados
- **Fallback**: localStorage como alternativa se SQLite falhar
- **Sincronização**: Dados persistem entre sessões

## Arquivos Incluídos

```
tarefas_segunda_pwa.zip
├── index.html          # Arquivo principal da aplicação
├── manifest.json       # Configuração PWA
├── sw.js              # Service Worker para cache offline
├── logo_compact.png   # Logo da aplicação
├── sql-wasm.js        # Biblioteca SQLite para JavaScript
└── sql-wasm.wasm      # WebAssembly do SQLite
```

## Instalação Local

### Opção 1: Servidor HTTP Simples (Python)
```bash
# Extrair o ZIP
unzip tarefas_segunda_pwa.zip
cd tarefas_segunda_pwa/

# Iniciar servidor local
python3 -m http.server 8000

# Acessar no navegador
http://localhost:8000
```

### Opção 2: Servidor HTTP Simples (Node.js)
```bash
# Instalar servidor global
npm install -g http-server

# Extrair e navegar
unzip tarefas_segunda_pwa.zip
cd tarefas_segunda_pwa/

# Iniciar servidor
http-server -p 8000

# Acessar no navegador
http://localhost:8000
```

### Opção 3: Live Server (VS Code)
1. Extrair o ZIP em uma pasta
2. Abrir a pasta no VS Code
3. Instalar extensão "Live Server"
4. Clicar com botão direito em `index.html` → "Open with Live Server"

## Instalação Online

### Opção 1: GitHub Pages
1. Criar repositório no GitHub
2. Fazer upload dos arquivos extraídos
3. Ativar GitHub Pages nas configurações
4. Acessar via URL: `https://seuusuario.github.io/nome-do-repo`

### Opção 2: Netlify
1. Acessar [netlify.com](https://netlify.com)
2. Arrastar a pasta extraída para o deploy
3. Receber URL automática
4. Configurar domínio personalizado (opcional)

### Opção 3: Vercel
1. Instalar Vercel CLI: `npm i -g vercel`
2. Na pasta extraída: `vercel`
3. Seguir instruções de deploy
4. Receber URL de produção

## Requisitos Técnicos

### Navegadores Suportados
- ✅ Chrome 57+
- ✅ Firefox 52+
- ✅ Safari 11+
- ✅ Edge 79+

### Funcionalidades Necessárias
- **WebAssembly**: Para SQLite (fallback para localStorage)
- **Service Workers**: Para funcionalidade PWA
- **IndexedDB**: Para cache avançado
- **File API**: Para upload de documentos .docx

## Como Usar

### 1. Primeira Execução
- Abrir a aplicação no navegador
- Aceitar instalação como PWA (opcional)
- Começar a adicionar tarefas

### 2. Adicionar Tarefas
- Clicar em "+ Nova Tarefa" na seção "Minhas Tarefas"
- Preencher título e descrição
- Salvar

### 3. Gerenciar Tarefas
- **Concluir**: Marcar como concluída (fica riscada)
- **Editar**: Modificar título e descrição
- **Excluir**: Remover permanentemente

### 4. Importar Documentos
- Usar botão "Escolher Arquivo" para upload de .docx
- Tarefas serão organizadas por responsável
- Usar estrelas para favoritar
- Usar ✔️ para mover favoritos para "Minhas Tarefas"

### 5. Backup e Restore
- **Backup SQLite**: Exportar banco completo
- **Restaurar SQLite**: Importar backup anterior
- **Exportar JSON/CSV**: Formatos alternativos

### 6. Busca
- Digitar no campo de busca
- Filtra todos os colapses automaticamente
- Destaca termos encontrados

## Funcionalidades PWA

### Instalação
- Botão automático aparece em navegadores compatíveis
- Instala como app nativo no dispositivo
- Funciona offline após primeira visita

### Cache Offline
- Todos os arquivos são armazenados localmente
- Funciona sem internet após carregamento inicial
- Dados persistem entre sessões

### Responsivo
- Interface adaptada para desktop e mobile
- Touch-friendly em dispositivos móveis
- Layout flexível

## Persistência de Dados

### SQLite (Preferencial)
- Banco relacional completo no navegador
- Transações ACID
- Backup/restore nativo
- Performance superior

### localStorage (Fallback)
- Usado se SQLite não estiver disponível
- Compatibilidade máxima
- Funcionalidade completa mantida

## Solução de Problemas

### PWA não instala
- Verificar se está sendo servido via HTTPS ou localhost
- Confirmar que manifest.json está acessível
- Verificar console do navegador para erros

### SQLite não funciona
- Verificar se WebAssembly está habilitado
- Aplicação usa localStorage automaticamente como fallback
- Funcionalidade completa mantida

### Upload de documentos falha
- Verificar se arquivo é .docx válido
- Confirmar que mammoth.js carregou corretamente
- Tentar com documento menor

### Cache não funciona
- Verificar se Service Worker registrou
- Limpar cache do navegador e recarregar
- Verificar console para erros de SW

## Desenvolvimento

### Estrutura do Código
- **HTML**: Interface principal com CSS inline
- **JavaScript**: Lógica da aplicação e SQLite
- **Service Worker**: Cache e funcionalidade offline
- **Manifest**: Configuração PWA

### Personalização
- Cores: Modificar variáveis CSS
- Logo: Substituir `logo_compact.png`
- Funcionalidades: Editar JavaScript no `index.html`

## Suporte

Para dúvidas ou problemas:
1. Verificar console do navegador para erros
2. Confirmar requisitos técnicos
3. Testar em navegador diferente
4. Verificar se está sendo servido corretamente

---

**Versão**: 1.0  
**Compatibilidade**: PWA Moderna  
**Licença**: Uso Livre

