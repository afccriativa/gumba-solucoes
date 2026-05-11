# ERP_MAP.md

## Visão Geral

Este documento representa a memória estrutural inicial do ERP em desenvolvimento.

O objetivo é:
- reduzir dependência da memória humana;
- facilitar manutenção;
- permitir evolução segura;
- melhorar auditoria técnica;
- preparar modularização futura;
- ajudar IA e futuros colaboradores a compreenderem o sistema.

---

# Estrutura Geral do ERP

O ERP possui uma arquitetura atualmente centralizada no ficheiro principal `index.html`, contendo:

- Interface visual;
- Componentes de UI;
- Estados de aplicação;
- Fluxos operacionais;
- Regras de negócio;
- Integração com Supabase;
- Responsividade;
- Modais;
- Dashboard;
- KPIs;
- Gestão financeira;
- Gestão de stock;
- Relatórios.

---

# Módulos Identificados

## 1. Autenticação

### Responsabilidade
Controlar acesso ao sistema.

### Funcionalidades identificadas
- Login;
- Sessão de utilizador;
- Controle de acesso.

### Dependências
- Supabase Auth;
- Estados globais da aplicação.

### Riscos atuais
- Fluxo ainda aparentemente centralizado;
- Possível mistura entre autenticação e UI.

---

## 2. Dashboard

### Responsabilidade
Apresentar visão geral operacional do ERP.

### Funcionalidades identificadas
- KPIs;
- Indicadores financeiros;
- Resumos operacionais;
- Cartões informativos.

### Componentes encontrados
- KPI Cards;
- Badges;
- Cards estatísticos.

### Dependências
- Vendas;
- Caixa;
- Stock;
- Relatórios.

### Observação estrutural
O dashboard já demonstra padrão visual consistente.

---

## 3. Gestão de Produtos

### Responsabilidade
Gerir produtos e inventário.

### Funcionalidades identificadas
- Cadastro de produtos;
- Visualização de produtos;
- Cards de produtos;
- Controlo visual de stock.

### Dependências
- Stock;
- Vendas;
- Relatórios.

### Riscos operacionais
- Possível inconsistência de stock sem regras explícitas;
- Necessidade futura de histórico de movimentação.

---

## 4. Gestão de Stock

### Responsabilidade
Controlar disponibilidade de produtos.

### Fluxo identificado
Produto → Stock → Venda → Atualização de inventário.

### Dependências
- Produtos;
- Vendas;
- Relatórios.

### Regras ainda não documentadas
- Stock mínimo;
- Bloqueio de stock negativo;
- Ajustes manuais;
- Histórico de movimentações.

### Riscos atuais
- Regras de integridade ainda não explícitas.

---

## 5. Vendas

### Responsabilidade
Registar operações comerciais.

### Funcionalidades identificadas
- Criação de vendas;
- Processamento operacional;
- Integração financeira;
- Atualização visual.

### Dependências
- Produtos;
- Stock;
- Caixa;
- Cobranças.

### Fluxo operacional identificado
1. Seleção de produto;
2. Validação;
3. Registo da venda;
4. Atualização visual;
5. Atualização financeira;
6. Atualização de stock.

### Riscos críticos
- Regras financeiras ainda implícitas;
- Possível acoplamento excessivo;
- Necessidade futura de rollback transacional.

---

## 6. Caixa / Financeiro

### Responsabilidade
Gerir movimentações financeiras.

### Funcionalidades identificadas
- Indicadores financeiros;
- Entradas e saídas;
- KPIs financeiros;
- Estados financeiros.

### Dependências
- Vendas;
- Cobranças;
- Relatórios.

### Pontos críticos
- Necessidade de rastreabilidade;
- Necessidade de logs financeiros;
- Necessidade de histórico imutável.

---

## 7. Cobranças

### Responsabilidade
Gerir pagamentos pendentes.

### Funcionalidades inferidas
- Controlo de dívidas;
- Estado pendente;
- Integração com vendas.

### Dependências
- Vendas;
- Financeiro.

### Regras ainda ausentes
- Vencimentos;
- Multas;
- Histórico de pagamento;
- Parcelamentos.

---

## 8. Relatórios

### Responsabilidade
Consolidar informações operacionais.

### Funcionalidades identificadas
- Visualização de dados;
- KPIs;
- Resumos;
- Indicadores.

### Dependências
- Todos os módulos operacionais.

### Observações
Este módulo tende a tornar-se um dos mais sensíveis do ERP.

Necessita:
- integridade;
- consistência;
- auditoria;
- padronização de cálculos.

---

## 9. Sistema de Modais

### Responsabilidade
Interações contextuais da interface.

### Funcionalidades identificadas
- Formulários;
- Detalhes;
- Confirmações;
- Interações rápidas.

### Observação estrutural
Sistema visual relativamente consistente.

### Risco atual
Centralização excessiva da lógica de abertura/fecho.

---

## 10. Sistema Visual / Design System

### Responsabilidade
Garantir consistência visual.

### Estruturas identificadas
- Variáveis CSS;
- Componentes reutilizáveis;
- Sistema de badges;
- Cards;
- Responsividade;
- Estados visuais.

### Pontos positivos
- Boa consistência;
- Boa organização visual;
- Forte identidade visual.

### Riscos futuros
- CSS monolítico;
- Crescimento descontrolado;
- Colisão de estilos.

---

# Fluxos Operacionais Identificados

## Fluxo de Venda

1. Utilizador seleciona produto;
2. Sistema valida informações;
3. Venda é registada;
4. Stock é atualizado;
5. Financeiro é atualizado;
6. Dashboard reflete alteração.

---

## Fluxo Financeiro

Venda → Caixa → KPIs → Relatórios

---

## Fluxo de Inventário

Produto → Entrada de stock → Disponibilidade → Venda → Redução de stock

---

# Dependências Críticas

## Vendas depende de:
- Produtos;
- Stock;
- Caixa;
- Cobranças.

---

## Dashboard depende de:
- Vendas;
- Financeiro;
- Stock;
- Relatórios.

---

## Relatórios depende de:
- Integridade de todos os módulos.

---

# Riscos Estruturais Identificados

## 1. Centralização excessiva
Grande parte da lógica encontra-se no mesmo ficheiro.

Impacto:
- manutenção difícil;
- regressões;
- crescimento caótico.

---

## 2. Dependência da memória humana
Muitas regras parecem implícitas.

Impacto:
- risco operacional;
- perda de contexto;
- dificuldade de escalar equipa.

---

## 3. Mistura de responsabilidades
UI, estados, lógica e comportamento coexistem de forma muito próxima.

Impacto:
- acoplamento elevado;
- debugging complexo.

---

## 4. Escalabilidade futura
O crescimento contínuo do `index.html` aumentará:
- risco técnico;
- tempo de manutenção;
- dificuldade de modularização.

---

# Prioridades Técnicas Recomendadas

## Prioridade Alta

### 1. Documentar regras financeiras
Crítico para integridade operacional.

### 2. Separar módulos logicamente
Mesmo antes da separação física.

### 3. Criar mapa de estados
Identificar:
- estados globais;
- dependências;
- eventos.

### 4. Criar nomenclatura oficial
Padronizar:
- funções;
- componentes;
- estados;
- eventos.

---

# Próximos Documentos Recomendados

## BUSINESS_RULES.md
Documentar regras de negócio.

---

## ARCHITECTURE.md
Documentar arquitetura técnica.

---

## CHANGELOG.md
Controlar evolução do sistema.

---

## DATABASE_MAP.md
Documentar estrutura relacional.

---

# Estado Atual do ERP

## Visualmente
Muito bom.

## Funcionalmente
Bom e promissor.

## Estruturalmente
Entrando em fase crítica de organização.

## Documentalmente
Ainda muito dependente da memória do criador.

---

# Objetivo desta documentação

Transformar o ERP de:

"Sistema dependente do criador"

para:

"Sistema compreensível, auditável e sustentável."

