# Changelog - Tarefas Segunda PWA

## Versão 4.0 - Restauração Completa, Renomeação, Exportação e Drag & Drop

### 🔧 **Correções e Novas Funcionalidades**

#### ✅ **Restauração Completa do SQLite**
- **Problema**: Restauração anterior não trazia todas as informações (Minhas Tarefas, Lista de Favoritos, Tarefas Organizadas por Pessoa).
- **Solução**: A lógica de `loadFromSQLite()` e `importDatabase()` foi aprimorada para carregar e restaurar corretamente todas as tabelas do banco de dados (minhas_tarefas, favoritos, tarefas_por_pessoa e accordion_order).
- **Funcionalidade**: Agora, ao restaurar um backup SQLite, todos os dados e a ordem dos colapses são recuperados integralmente.

#### ✅ **Renomeação de Colapses**
- **Funcionalidade**: Possibilidade de renomear os títulos dos colapses gerados a partir do upload do Word (tarefasPorPessoa).
- **Exceções**: Os colapses "Minhas Tarefas" e "Lista de Favoritos" permanecem fixos e não podem ser renomeados.
- **Interface**: Ao clicar no título de um colapse renomeável, ele se torna um campo de texto editável. A alteração é salva automaticamente ao perder o foco ou pressionar Enter.

#### ✅ **Exportação JSON Aprimorada**
- **Funcionalidade**: O botão "Exportar Favoritos (JSON)" foi renomeado para "Exportar Favoritos + Minhas Tarefas (JSON)".
- **Conteúdo**: Agora, o arquivo JSON exportado inclui tanto a `Lista de Favoritos` quanto as `Minhas Tarefas`, facilitando o backup e a portabilidade de dados.

#### ✅ **Drag and Drop para Colapses**
- **Funcionalidade**: Permite reordenar os colapses na interface usando arrastar e soltar.
- **Restrições**: "Minhas Tarefas" e "Lista de Favoritos" são fixos e não podem ser movidos, permanecendo sempre no topo em suas posições originais.
- **Persistência**: A nova ordem dos colapses é salva no SQLite e persistida entre as sessões.

#### ✅ **Drag and Drop para Tarefas**
- **Funcionalidade**: Permite mover tarefas individuais entre diferentes colapses usando arrastar e soltar.
- **Zonas de Drop**: Cada colapse (incluindo "Minhas Tarefas" e "Lista de Favoritos") possui uma "zona de drop" visível durante o arrasto, indicando onde a tarefa pode ser solta.
- **Lógica de Movimentação**: A tarefa é removida do colapse de origem e adicionada ao colapse de destino, com a persistência dos dados atualizada no SQLite.

### 🎨 **Melhorias de Interface**

#### **Estilos para Drag and Drop**
```css
.accordion.dragging, .accordion-content li.dragging {
  opacity: 0.5;
}
.drop-zone {
  min-height: 50px;
  border: 2px dashed #ccc;
  border-radius: 5px;
  margin: 10px 0;
  padding: 10px;
  text-align: center;
  color: #666;
  display: none;
}
.drop-zone.active {
  display: block;
  border-color: #007BFF;
  background-color: #f0f8ff;
}
.drop-zone.hover {
  border-color: #28a745;
  background-color: #f0fff0;
}
```

#### **Estilos para Renomeação de Colapses**
```css
.accordion-header.editable {
  cursor: text;
}
.accordion-header input {
  background: transparent;
  border: none;
  color: white;
  font-weight: bold;
  font-size: inherit;
  width: 100%;
  outline: none;
}
.rename-btn {
  background: transparent;
  border: none;
  color: white;
  cursor: pointer;
  padding: 2px 5px;
  border-radius: 3px;
  font-size: 12px;
}
```

### 💾 **Persistência Melhorada**

#### **Estrutura SQLite Atualizada**
```sql
-- Tabela para a ordem dos acordeões
CREATE TABLE IF NOT EXISTS accordion_order (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  accordion_id TEXT NOT NULL,
  position INTEGER NOT NULL
);
```

#### **Fluxo de Backup/Restauração**
- O backup agora inclui a ordem dos colapses (`accordion_order`).
- A restauração carrega todas as tabelas e a ordem, garantindo a integridade completa dos dados.

### 🧪 **Testes Realizados**

#### **Funcionalidades Testadas**
- ✅ Restauração completa do SQLite (Minhas Tarefas, Favoritos, Tarefas por Pessoa, Ordem dos Colapses).
- ✅ Renomeação de colapses (exceto fixos).
- ✅ Exportação JSON com Minhas Tarefas.
- ✅ Drag and Drop para ordenar colapses.
- ✅ Drag and Drop para mover tarefas entre colapses.
- ✅ Persistência de todas as alterações.

### 📋 **Próximas Melhorias Sugeridas**

#### **Funcionalidades Futuras**
- [ ] Sincronização em nuvem
- [ ] Notificações push
- [ ] Modo escuro
- [ ] Categorias de tarefas
- [ ] Relatórios de produtividade

---

**Data**: 08/07/2025  
**Versão**: 4.0  
**Compatibilidade**: PWA Moderna com SQLite  
**Status**: ✅ Produção

