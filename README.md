# 🖥️ Sistema de Gestão de Chamados — Service Desk Corporativo

> Projeto de modelagem de sistemas desenvolvido na disciplina **Prototipagem de Sistemas Computacionais** — Bacharelado em Sistemas de Informação (EAD)  
> **Autora:** Bárbara Crisóstomo Navarro

---

## 📋 Índice

- [Sobre o Projeto](#sobre-o-projeto)
- [Problema Identificado](#problema-identificado)
- [Solução Proposta](#solução-proposta)
- [Stakeholders](#stakeholders)
- [Requisitos do Sistema](#requisitos-do-sistema)
- [Modelagem UML](#modelagem-uml)
  - [Diagrama de Casos de Uso](#diagrama-de-casos-de-uso)
  - [Diagrama de Classes](#diagrama-de-classes)
  - [Diagrama de Sequência](#diagrama-de-sequência)
  - [Diagrama de Atividades](#diagrama-de-atividades)
- [Atores do Sistema](#atores-do-sistema)
- [Casos de Uso Principais](#casos-de-uso-principais)
- [Tecnologias e Metodologias](#tecnologias-e-metodologias)
- [Estrutura do Projeto](#estrutura-do-projeto)

---

## 📌 Sobre o Projeto

Este repositório documenta a **modelagem completa de um Sistema de Gestão de Chamados (Service Desk Corporativo)**, desenvolvida ao longo de três experiências práticas da disciplina. O trabalho abrange desde o mapeamento do processo atual (As-Is) em BPMN até a especificação técnica detalhada com diagramas UML, incluindo casos de uso, diagrama de classes, diagrama de sequência e diagrama de atividades.

O projeto segue uma rastreabilidade rigorosa: cada requisito funcional é originado de uma falha identificada no processo atual, garantindo que a solução proposta resolva problemas reais e mensuráveis.

---

## 🚨 Problema Identificado

O processo de suporte técnico interno da organização operava de forma **completamente informal e descentralizada**, com impactos críticos:

| Falha | Descrição | Impacto |
|---|---|---|
| **Registro Volátil** | Chamados abertos via e-mail, WhatsApp ou verbalmente | Histórico irrastreável, perda de solicitações |
| **Loop de Cobranças** | Usuário cobra manualmente sem prazo ou critério | Retrabalho contínuo, desgaste com a TI |
| **Sem Registro Formal** | Técnico recebe demanda sem número, categoria ou data | Chamados esquecidos ou duplicados |
| **Priorização Subjetiva** | Gateway "Urgente?" baseado em julgamento individual | Injustiça, falta de auditoria |
| **Fila Informal** | Chamados gerenciados em memória ou anotações | Redistribuição impossível em alta demanda |
| **Gestão Reativa** | Gestor monitora informalmente sem métricas | Decisões sem dados, gargalos invisíveis |

> 📊 Estimativa: até **30% do tempo dos técnicos** desperdiçado em atividades administrativas manuais que poderiam ser automatizadas.

---

## ✅ Solução Proposta

Um **portal centralizado de Service Desk** com as seguintes funcionalidades principais:

```
RF01 → Portal de abertura de chamados padronizado
RF02 → Motor de priorização automática (Crítico / Alto / Médio / Baixo)
RF03 → Painel de acompanhamento em tempo real
RF04 → Sistema de notificações automáticas (e-mail + alertas internos)
RF05 → Painel gerencial com KPIs e exportação de relatórios
```

---

## 👥 Stakeholders

| Stakeholder | Interesse Principal | Nível de Influência |
|---|---|---|
| Diretoria | KPIs, ROI, redução de custos | Muito Alto |
| Gestor de TI | Centralização, monitoramento de SLA | Alto |
| Analista de Sistemas | Requisitos precisos, protótipos validados | Alto |
| Equipe de Desenvolvimento | Requisitos claros e estáveis | Alto |
| Colaboradores | Interface intuitiva, acompanhamento em tempo real | Médio |
| Técnicos de Suporte | Filas priorizadas, base de conhecimento | Médio |
| Auditoria / Compliance | Logs, rastreabilidade, conformidade | Baixo |
| Fornecedores | Cumprimento de SLAs e contratos | Baixo |

---

## 📐 Requisitos do Sistema

### Requisitos Funcionais

| ID | Funcionalidade | Prioridade |
|---|---|---|
| RF01 | Portal de abertura de chamados com formulário padronizado | 🔴 Alta |
| RF02 | Priorização automática via regras predefinidas | 🔴 Alta |
| RF03 | Acompanhamento de status em tempo real | 🔴 Alta |
| RF04 | Notificações automáticas por e-mail/push | 🟡 Média |
| RF05 | Painel gerencial com indicadores e exportação | 🟡 Média |

### Requisitos Não Funcionais

| ID | Categoria | Descrição |
|---|---|---|
| RNF01 | Performance | Resposta em até **3 segundos** com até 200 usuários simultâneos |
| RNF02 | Segurança | Login obrigatório, perfis distintos, logs de auditoria completos |
| RNF03 | Usabilidade | Interface responsiva, abertura de chamado em até 3 min sem treinamento |

---

## 🗂️ Modelagem UML

### Diagrama de Casos de Uso

O diagrama contempla **12 casos de uso** distribuídos entre 5 atores:

```
Atores:
  ├── Colaborador         → UC01 (Abrir Chamado), UC03 (Acompanhar Status), UC05 (Notificações), UC11 (Anexar Arquivo)
  ├── Técnico de Suporte  → UC04 (Atender Chamado), UC06 (Encerrar e Registrar Solução)
  ├── Gestor de TI        → UC07 (Painel Gerencial), UC09 (Configurar Regras e SLA)
  ├── Administrador       → UC08 (Gerenciar Usuários), UC12 (Consultar Logs)
  └── Sist. Notificações  → UC10 (Enviar Notificação por Evento)

Relacionamentos:
  ├── «include»  → UC01 inclui UC02 (priorização obrigatória)
  ├── «include»  → UC04 inclui UC06 (encerramento formal obrigatório)
  └── «extend»   → UC01 estende UC11 (anexo é opcional)
```

> 📎 Ver diagrama completo em `/diagramas/uml-casos-de-uso.png`

---

### Diagrama de Classes

O modelo estrutural é composto por **5 classes principais**:

```
Usuario (abstrata)
  ├── + idUsuario: String
  ├── + nome, email: String
  ├── - senhaHash: String
  ├── + perfil, status: Enum
  └── + autenticar(), encerrarSessao()

Colaborador ──herda──▶ Usuario
  ├── + setor, cargo: String
  └── + abrirChamado(), verStatus(), salvarRascunho()

TecnicoSuporte ──herda──▶ Usuario
  ├── + especialidades: List
  ├── + cargaTrabalho: Int
  └── + iniciar(), registrarAcao(), transferir(), fechar()

Chamado
  ├── + protocolo: String {id}
  ├── + titulo, descricao: String
  ├── + categoria, urgencia, prioridade, status: Enum
  ├── + dataAbertura, dataFechamento: DateTime
  ├── + solicitante ──▶ Colaborador (1)
  ├── + tecnico ──▶ TecnicoSuporte (0..1)
  └── + validarCampos(), classificarPrioridade(), mudarStatus()

HistoricoChamado ──composição──▶ Chamado (1..*)
  ├── + dataHora: DateTime
  ├── + autor: Usuario
  ├── + statusDe, statusPara: Enum
  └── + gravarAcao()

Anexo ──agregação──▶ Chamado (0..*)
  ├── + nomeArquivo, tipoArquivo: String
  ├── + tamanho: Long
  └── + validarFormato(), validarTamanho()
```

> 📎 Ver diagrama completo em `/diagramas/uml-classes.png`

---

### Diagrama de Sequência

Modela o **UC01 — Abrir Chamado** com 20 interações entre 5 lifelines:

```
COL: Colaborador  →  INT: Interface Service Desk
                  →  CTL: Controlador Chamado
                  →  MOT: Motor de Priorização (UC02)
                  →  BD:  Banco de Dados
                  →  NOT: Sistema de Notificações
```

**Fluxo principal resumido:**
1. Colaborador acessa o portal e seleciona "Abrir Chamado"
2. Sistema valida sessão e carrega formulário com setor pré-preenchido
3. Colaborador preenche e submete o formulário
4. Sistema valida campos e verifica duplicidade (≥ 85% nas últimas 24h)
5. Sistema gera protocolo único e persiste o chamado
6. Motor de priorização classifica automaticamente (Crítico/Alto/Médio/Baixo)
7. Sistema dispara notificação e exibe confirmação com protocolo

> 📎 Ver diagrama completo em `/diagramas/uml-sequencia-uc01.png`

---

### Diagrama de Atividades

Fluxo lógico completo do **UC01** com **20 nós de decisão**, distribuídos em 3 raias:

```
┌─────────────┬───────────────────────────────┬──────────────────────────────┐
│ Colaborador │           Sistema             │   Sub-sistemas e Externos    │
├─────────────┼───────────────────────────────┼──────────────────────────────┤
│ Acessa      │ Valida sessão e permissões    │                              │
│ portal      │ Exibe formulário pré-preenc.  │                              │
│ Preenche    │ [Deseja anexar?]              │ UC11: valida arquivo         │
│ formulário  │ Valida todos os campos        │                              │
│ Submete     │ Verifica duplicidade (24h)    │                              │
│             │ Gera protocolo único          │                              │
│             │ Persiste chamado              │                              │
│             │ [Persistência OK?]            │ UC02: classifica prioridade  │
│             │ Atribui à fila do técnico     │ Sist. Notif.: dispara alerta │
│ Vê          │ Exibe tela de confirmação     │                              │
│ confirmação │                               │                              │
└─────────────┴───────────────────────────────┴──────────────────────────────┘
```

> 📎 Ver diagrama completo em `/diagramas/uml-atividades-uc01.png`

---

## 🎭 Atores do Sistema

| Ator | Tipo | Responsabilidade |
|---|---|---|
| **Colaborador** | Primário | Abre chamados, acompanha status, responde a técnicos |
| **Técnico de Suporte** | Primário | Gerencia fila, executa atendimentos, documenta soluções |
| **Gestor de TI** | Primário | Monitora KPIs, configura SLAs, redistribui cargas |
| **Administrador** | Primário | Gerencia usuários/permissões, monitora logs de auditoria |
| **Sist. Notificações** | Secundário (sistema) | Dispara alertas automáticos por gatilhos de eventos |

---

## 📂 Casos de Uso Principais

### UC01 — Abrir Chamado
> **Ator principal:** Colaborador

**Pré-condições:** sessão ativa, cadastro válido, sistema operacional disponível, regras de SLA configuradas.

**Fluxo principal:** 15 passos — acesso → preenchimento → validação → geração de protocolo → priorização → notificação → confirmação.

**Fluxos alternativos:**
- A1: Anexar evidências ao chamado
- A2: Salvar rascunho (válido por 7 dias)
- A3: Cancelar abertura
- A4: Tratamento de chamado duplicado (≥ 85% similaridade nas últimas 24h)

**Fluxos de exceção:**
- E1: Campo inválido
- E2: Falha técnica (rollback + retentativas)
- E3: Sessão expirada (rascunho automático)
- E4: Notificação indisponível (fila de retentativa)
- E5: Sem permissão de acesso

---

### UC04 — Atender Chamado
> **Ator principal:** Técnico de Suporte

**Pré-condições:** fila disponível com chamados priorizados, regras de SLA ativas.

**Fluxo principal:** 15 passos — seleção na fila → início formal → análise → execução → registro → encerramento via UC06.

**Fluxos alternativos:**
- A1: Solicitar informação complementar ao colaborador (pausa de SLA)
- A2: Reatribuição para outro técnico/especialidade
- A3: Pausa por dependência externa
- A4: Sugestão automática da base de conhecimento (similaridade ≥ 80%)

**Fluxos de exceção:**
- E1: Conflito de acesso simultâneo à fila
- E2: Falha de registro (rollback)
- E3: Estouro de SLA (marcação para métricas)
- E4: Sessão expirada (rascunho vinculado ao chamado)
- E5: Falha de notificação (fila de retentativa)

---

## 🛠️ Tecnologias e Metodologias

- **Notação BPMN 2.0** — Mapeamento do processo As-Is com raias e gateways
- **UML 2.5** — Diagramas de Casos de Uso, Classes, Sequência e Atividades
- **Análise de Stakeholders** — Mapeamento de interesses e nível de influência
- **Levantamento de Requisitos** — Funcionais (RF01–RF05) e Não Funcionais (RNF01–RNF03)
- **Rastreabilidade completa** — Cada requisito rastreado a uma falha identificada no BPMN As-Is

---

## 📁 Estrutura do Projeto

```
📦 service-desk-modelagem
 ┣ 📂 diagramas/
 ┃ ┣ 📄 bpmn-as-is.png
 ┃ ┣ 📄 uml-casos-de-uso.png
 ┃ ┣ 📄 uml-classes.png
 ┃ ┣ 📄 uml-sequencia-uc01.png
 ┃ ┗ 📄 uml-atividades-uc01.png
 ┣ 📂 documentacao/
 ┃ ┣ 📄 Projeto1-Mapeando-Processo.pdf
 ┃ ┣ 📄 Atividade2-Casos-de-Uso.pdf
 ┃ ┗ 📄 Atividade3-Diagramas-UML.pdf
 ┗ 📄 README.md
```

---

## 📊 Rastreabilidade Requisitos × Gargalos

| Gargalo (BPMN As-Is) | Requisito que resolve |
|---|---|
| Registro Volátil — sem canal formal | RF01 — Portal de abertura padronizado |
| Gateway "Urgente?" subjetivo | RF02 — Motor de priorização automática |
| Loop de cobrança manual | RF03 — Painel de acompanhamento em tempo real |
| Ausência de notificações | RF04 — Notificações automáticas |
| "Verifica status informalmente" (Gestor) | RF05 — Painel gerencial com KPIs |

---

<div align="center">

**Bárbara Crisóstomo Navarro**  
Bacharelado em Sistemas de Informação — EAD | 2026  
Disciplina: Prototipagem de Sistemas Computacionais

</div>
