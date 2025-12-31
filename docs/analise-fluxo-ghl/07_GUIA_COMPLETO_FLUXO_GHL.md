# GUIA COMPLETO - Workflow GHL Mottivme EUA Versionado

**Versao:** 1.0
**Data:** 2025-12-31
**Workflow ID:** R2fVs2qpct1Qr2Y1
**Nome:** GHL - Mottivme - EUA Versionado
**Arquivo:** `GHL - Mottivme - EUA Versionado.json`

---

## INDICE NAVEGAVEL

1. [VISAO GERAL EXECUTIVA](#1-visao-geral-executiva)
   - [1.1 Resumo do Workflow](#11-resumo-do-workflow)
   - [1.2 Principais Descobertas](#12-principais-descobertas)
   - [1.3 Recomendacoes Urgentes](#13-recomendacoes-urgentes)
   - [1.4 Metricas Gerais](#14-metricas-gerais)

2. [ARQUITETURA DO SISTEMA](#2-arquitetura-do-sistema)
   - [2.1 Overview Estrutural](#21-overview-estrutural)
   - [2.2 Diagrama Arquitetural Consolidado](#22-diagrama-arquitetural-consolidado)
   - [2.3 Distribuicao por Tipo de No](#23-distribuicao-por-tipo-de-no)
   - [2.4 Padroes Arquiteturais Identificados](#24-padroes-arquiteturais-identificados)
   - [2.5 Grupos Funcionais](#25-grupos-funcionais)

3. [JORNADA DO DADO](#3-jornada-do-dado)
   - [3.1 Fluxo Completo Ponta-a-Ponta](#31-fluxo-completo-ponta-a-ponta)
   - [3.2 Transformacoes Criticas](#32-transformacoes-criticas)
   - [3.3 Data Lineage](#33-data-lineage)
   - [3.4 Ciclo de Vida dos Dados](#34-ciclo-de-vida-dos-dados)

4. [INTEGRACOES E DEPENDENCIAS](#4-integracoes-e-dependencias)
   - [4.1 Sistemas Externos](#41-sistemas-externos)
   - [4.2 APIs e Endpoints](#42-apis-e-endpoints)
   - [4.3 Credenciais](#43-credenciais)
   - [4.4 Pontos de Falha](#44-pontos-de-falha)

5. [LOGICA DE NEGOCIO](#5-logica-de-negocio)
   - [5.1 Maquina de Estados](#51-maquina-de-estados)
   - [5.2 Regras de Negocio Formais](#52-regras-de-negocio-formais)
   - [5.3 Decisoes Condicionais](#53-decisoes-condicionais)
   - [5.4 Tools do AI Agent](#54-tools-do-ai-agent)

6. [SEGURANCA E CONFIABILIDADE](#6-seguranca-e-confiabilidade)
   - [6.1 Vulnerabilidades Criticas](#61-vulnerabilidades-criticas)
   - [6.2 Matriz de Risco](#62-matriz-de-risco)
   - [6.3 Recomendacoes Prioritarias](#63-recomendacoes-prioritarias)
   - [6.4 Compliance LGPD/GDPR](#64-compliance-lgpdgdpr)

7. [GUIAS PRATICOS](#7-guias-praticos)
   - [7.1 Como Modificar o Workflow](#71-como-modificar-o-workflow)
   - [7.2 Como Debugar Problemas](#72-como-debugar-problemas)
   - [7.3 Como Adicionar Novas Features](#73-como-adicionar-novas-features)
   - [7.4 Checklist de Melhores Praticas](#74-checklist-de-melhores-praticas)

8. [REFERENCIA RAPIDA](#8-referencia-rapida)
   - [8.1 Indice de Nos por ID](#81-indice-de-nos-por-id)
   - [8.2 Glossario de Termos](#82-glossario-de-termos)
   - [8.3 Variaveis n8n Criticas](#83-variaveis-n8n-criticas)
   - [8.4 Tabelas do Banco de Dados](#84-tabelas-do-banco-de-dados)

9. [APENDICES](#9-apendices)
   - [9.1 Diagramas Consolidados](#91-diagramas-consolidados)
   - [9.2 Codigo Critico](#92-codigo-critico)
   - [9.3 Schemas de Dados](#93-schemas-de-dados)
   - [9.4 Links para Documentos Detalhados](#94-links-para-documentos-detalhados)

---

# 1. VISAO GERAL EXECUTIVA

## 1.1 Resumo do Workflow

O **GHL - Mottivme - EUA Versionado** e um sistema de atendimento automatizado com Inteligencia Artificial para leads do mercado EUA (Estados Unidos). O workflow processa mensagens recebidas via **GoHighLevel (GHL)** - seja por WhatsApp ou Instagram - e gera respostas contextualizadas utilizando um **AI Agent** baseado em **Google Gemini 2.5 Pro**.

### Proposito Principal

- **Atendimento automatizado 24/7** de leads interessados em servicos de carreira/consultoria
- **Qualificacao inteligente** de leads por objetivo (Carreira vs Consultoria)
- **Agendamento automatico** de reunioes via calendario GHL
- **Memoria conversacional** para contexto entre interacoes
- **Suporte multimidia** (texto, audio, imagens)

### Escopo Funcional

| Funcionalidade | Status | Descricao |
|----------------|--------|-----------|
| Recepcao de mensagens | Ativo | Webhook para WhatsApp e Instagram |
| Processamento de texto | Ativo | Normalizacao, deduplicacao, memoria |
| Transcricao de audio | Ativo | OpenAI Whisper em portugues |
| Analise de imagens | Ativo | Claude Sonnet Vision |
| AI Agent com tools | Ativo | Gemini 2.5 Pro + 8 ferramentas |
| Envio de respostas | Ativo | WhatsApp (SMS) e Instagram (IG) |
| Controle de concorrencia | Ativo | Fila de mensagens e locks |
| Rastreamento de custos | Ativo | Calculo de tokens e USD/BRL |

### Complexidade Geral

| Metrica | Valor | Classificacao |
|---------|-------|---------------|
| Total de Nos | **115** | Alta |
| Nos Ativos | 111 | - |
| Nos Desabilitados | 4 | - |
| Tipos de Nos Distintos | **27** | Alta |
| Credenciais Utilizadas | 6 | Normal |
| Sistemas Externos Integrados | 8 | Alta |
| Tools do AI Agent | 8 | Alta |
| **Complexidade Geral** | - | **ALTA** |

---

## 1.2 Principais Descobertas

### Pontos Fortes

1. **Arquitetura robusta de fila de mensagens** - Evita processamento duplicado quando leads enviam multiplas mensagens rapidas
2. **Memoria conversacional persistente** - Historico de conversas mantido no PostgreSQL para contexto da IA
3. **Suporte multimidia completo** - Processa texto, audio (Whisper) e imagens (Claude)
4. **AI Agent modular** - 8 tools para acoes no mundo real (agendar, atualizar campos, buscar disponibilidade)
5. **Roteamento multi-canal** - Responde automaticamente pelo canal de origem (WhatsApp/Instagram)
6. **Rastreamento de custos** - Calculo de tokens e custos por execucao
7. **Controle de concorrencia** - Mecanismo de lock para evitar respostas duplicadas

### Pontos de Atencao

1. **Seguranca critica** - Webhook sem autenticacao, SQL injection em uma query
2. **Error handling incompleto** - AI Agent sem retry configurado
3. **Sem circuit breakers** - APIs externas podem sobrecarregar em caso de falha
4. **Dados pessoais sem expurgo** - Retencao indefinida viola principios LGPD
5. **API keys em logs** - Credenciais expostas em strings de session
6. **Ausencia de fallbacks** - Se Gemini falhar, workflow para completamente

### Metricas de Saude

| Indicador | Valor | Status |
|-----------|-------|--------|
| Documentacao (Sticky Notes) | 13 | BOM |
| Nos desabilitados | 4 (3.5%) | ACEITAVEL |
| Nos com retry | ~20 | PARCIAL |
| Nos com error handling | ~50% | ATENCAO |
| Error Workflow configurado | NAO | CRITICO |
| Circuit breakers | NAO | CRITICO |
| Validacao de inputs | PARCIAL | ATENCAO |

---

## 1.3 Recomendacoes Urgentes

### PRIORIDADE CRITICA (Implementar Imediatamente)

| # | Recomendacao | Impacto | Esforco |
|---|--------------|---------|---------|
| 1 | **Autenticar Webhook** - Adicionar verificacao de header/token | Seguranca | Baixo |
| 2 | **Corrigir SQL Injection** - Usar parametros na query `historico_mensagens_leads` | Seguranca | Baixo |
| 3 | **Remover API Key de Logs** - Nao incluir em strings de session | Seguranca | Baixo |
| 4 | **Criar Error Workflow** - Notificar falhas via Slack/Email | Confiabilidade | Medio |

### PRIORIDADE ALTA (Implementar Esta Semana)

| # | Recomendacao | Impacto | Esforco |
|---|--------------|---------|---------|
| 5 | **Retry no AI Agent** - Adicionar `retryOnFail: true` | Confiabilidade | Baixo |
| 6 | **Circuit Breaker para GHL** - Evitar cascata de falhas | Resiliencia | Medio |
| 7 | **Validar Tenant** - Verificar api_key x location_id | Seguranca | Medio |
| 8 | **Fallback entre LLMs** - Gemini -> OpenAI se falhar | Disponibilidade | Medio |

### PRIORIDADE MEDIA (Implementar Este Mes)

| # | Recomendacao | Impacto | Esforco |
|---|--------------|---------|---------|
| 9 | **Implementar Retencao de Dados** - Expurgo automatico > 2 anos | Compliance | Medio |
| 10 | **Direito ao Esquecimento** - Workflow para deletar dados de lead | Compliance | Alto |
| 11 | **Cache de Configuracoes** - Reduzir queries de agent_versions | Performance | Medio |
| 12 | **Timeouts Explicitos** - Definir em todas as chamadas HTTP | Resiliencia | Baixo |

---

## 1.4 Metricas Gerais

### Distribuicao por Categoria

```
POSTGRES              ████████████████████ 20 (17.4%)
HTTP REQUEST          ████████████████ 16 (13.9%)
CODE                  ███████████████ 15 (13.0%)
STICKY NOTE           █████████████ 13 (11.3%)
SET                   ████████ 8 (7.0%)
SWITCH                ███████ 7 (6.1%)
LANGCHAIN TOOLS       █████ 5 (4.3%)
EXECUTION DATA        ████ 4 (3.5%)
IF                    ████ 4 (3.5%)
FILTER                ███ 3 (2.6%)
WAIT                  ███ 3 (2.6%)
OUTROS                ████████████████ 17 (14.8%)
```

### Custos Estimados por Execucao

| LLM | Tokens Input | Tokens Output | Custo USD |
|-----|--------------|---------------|-----------|
| Gemini 2.5 Pro | ~10,050 | Variavel | $0.01-0.05 |
| Gemini 2.5 Flash (Parser) | Variavel | Variavel | $0.001-0.01 |
| OpenAI Whisper (audio) | N/A | N/A | ~$0.006/min |
| Claude Sonnet (imagem) | N/A | N/A | ~$0.003/imagem |
| **Total Estimado** | - | - | **$0.02-0.07/msg** |

---

# 2. ARQUITETURA DO SISTEMA

## 2.1 Overview Estrutural

O workflow segue uma arquitetura de **pipeline linear com branches condicionais**, dividido em 10 grupos funcionais:

| Grupo | Funcao | Nos Principais |
|-------|--------|----------------|
| 1. Entrada | Recepcao de webhooks | Mensagem recebida |
| 2. Normalizacao | Limpeza e enriquecimento | Contexto UTM, Normalizar Nome/Dados |
| 3. Roteamento | Decisoes por tipo/status | Tipo de mensagem, IA Ativa? |
| 4. Midia | Processamento audio/imagem | Transcrever audio, Analyze image |
| 5. Fila | Anti-encavalamento | Enfileirar, Esperar, Deduplica |
| 6. Preparacao | Contexto para IA | Memoria Lead, Set mensagens |
| 7. AI Agent | Geracao de resposta | AI Agent Modular + Tools |
| 8. Formatacao | Parser para WhatsApp | Parser Chain, Segmentos |
| 9. Envio | Entrega ao lead | Loop, Canal, WhatsApp/Instagram |
| 10. Tracking | Metricas e custos | Execution Data, Track AI Cost |

### Entry Points

| No | Tipo | Descricao |
|----|------|-----------|
| Mensagem recebida | Webhook | Unico ponto de entrada via POST `/factor-ai` |

### Exit Points

| No | Funcao |
|----|--------|
| Call Track AI Cost | Rastrear custos de AI (saida principal) |
| Whatsapp / Instagram | Envio de mensagens |
| historico_mensagens_leads | Persistir historico |
| Diversos finais de branch | Descartes e validacoes |

---

## 2.2 Diagrama Arquitetural Consolidado

```mermaid
flowchart TD
    subgraph ENTRADA["1. ENTRADA"]
        WH[Webhook: Mensagem recebida<br/>POST /factor-ai]
    end

    subgraph NORMALIZACAO["2. NORMALIZACAO"]
        WH --> CTX[Contexto UTM]
        CTX --> NN[Normalizar Nome]
        NN --> ND[Normalizar Dados]
        ND --> ED5[Execution Data5]
        ED5 --> C1[Code1 - Timestamps]
        C1 --> CTM[Classificar Tipo Mensagem]
        CTM --> INFO[Info - 60+ campos]
    end

    subgraph ROTEAMENTO["3. ROTEAMENTO"]
        INFO --> SC[Search Contact]
        INFO --> EF1[Edit Fields1]
        INFO --> IAA{IA Ativa?}

        IAA -->|Nao| FIM1[Fim: IA Desativada]
        IAA -->|Sim| TM{Tipo de Mensagem}
    end

    subgraph MIDIA["4. PROCESSAMENTO MIDIA"]
        TM -->|Audio| DA[Download audio]
        DA --> EFF[Extract from File]
        EFF --> EXT[Extrair extensao]
        EXT --> CTF[Convert to File]
        CTF --> TA[Transcrever - Whisper]
        TA --> IOA[Imagem ou audio]

        TM -->|Imagem| AI[Analyze image - Claude]
        AI --> IOA
    end

    subgraph FILA["5. FILA DE MENSAGENS"]
        TM -->|Texto| ENQ[Enfileirar mensagem]
        IOA --> ENQ
        ENQ --> ESP[Esperar 18s]
        ESP --> BM[Buscar mensagens]
        BM --> FM[Form Mensagem]
        FM --> ME{Mensagem encavalada?}
        ME -->|Sim| FIM2[Descarta]
        ME -->|Nao| LFM[Limpar fila]
    end

    subgraph CONCORRENCIA["6. CONTROLE CONCORRENCIA"]
        LFM --> CA[Conversa Ativa]
        CA --> AP{Acao Planejada}
        AP -->|Iniciar| PAI{Permitido AI?}
        AP -->|Aguardar| SE[Salvar Espera]
        AP -->|Ignorar| FIM3[Fim: Retries > 10]
        SE --> WAIT[Wait 15s]
        WAIT --> CA
    end

    subgraph PREPARACAO["7. PREPARACAO IA"]
        PAI -->|Nao| FIM4[Fim: Bloqueado]
        PAI -->|Sim| SII[Salvar Inicio IA]
        SII --> MA[Mensagens Anteriores]
        MA --> PM[Preparar Mensagem]
        PM --> ML[Memoria Lead]
        ML --> DM[Deduplica Mensagens]
        DM --> SM[Set mensagens]
        SM --> BAA[Buscar Agente Ativo]
        BAA --> PEC[Preparar Execucao + Contexto]
    end

    subgraph AI_AGENT["8. AI AGENT"]
        PEC --> TC{Tudo certo?}
        TC -->|Erro| FLT[Filter]
        TC -->|OK| AIA[AI Agent - Modular<br/>Gemini 2.5 Pro]

        subgraph TOOLS["Tools Disponiveis"]
            T1[Busca_disponibilidade]
            T2[Agendar_reuniao]
            T3[Atualizar Work Permit]
            T4[Atualizar Profissao]
            T5[Atualizar Estado]
            T6[Adicionar_tag_perdido]
            T7[Busca historias]
            T8[Think]
        end

        AIA <-.-> TOOLS
    end

    subgraph RESPOSTA["9. FORMATACAO RESPOSTA"]
        AIA --> TM1{Tipo mensagem1}
        TM1 --> CAA[Conversa ativa atualizada]
        CAA --> IFF{Ha processo esperando?}
        IFF --> DEM{Deve enviar?}
        DEM --> PC[Parser Chain - Gemini Flash]
        PC --> CCLM[Calcular Custo LLM]
        CCLM --> IF1{Output limpo?}
        IF1 --> SEG[Segmentos - Split]
    end

    subgraph ENVIO["10. ENVIO"]
        SEG --> LOI[Loop Over Items]
        LOI --> MIA[Memoria IA]
        LOI --> CAN{Canal}
        CAN -->|WhatsApp| WA[API GHL - SMS]
        CAN -->|Instagram| IG[API GHL - IG]
        WA --> W15[Wait 1.5s]
        IG --> W15
        W15 --> NOP[no.op]
        NOP --> LOI
    end

    subgraph FINALIZACAO["11. FINALIZACAO"]
        LOI -->|Fim Loop| ED1[Execution Data1]
        ED1 --> CTAC[Call Track AI Cost]
        CTAC --> FIMOK[FIM - Sucesso]
    end

    subgraph RESET["FLUXOS ALTERNATIVOS"]
        TM -->|/reset| RESET1[Limpar memoria]
        RESET1 --> RESET2[Resetar status]
        RESET2 --> RESET3[Enviar confirmacao]

        TM -->|/teste| TESTE1[Listar campos]
        TESTE1 --> TESTE2[Detectar objetivo]
        TESTE2 --> TESTE3[Atualizar contato]
    end
```

---

## 2.3 Distribuicao por Tipo de No

### Nos Postgres (20 nos - 17.4%)

| ID | Nome | Operacao | Tabela |
|----|------|----------|--------|
| `780b94b3...` | Postgres | executeQuery | execution_metrics |
| `58dd5b6d...` | Buscar mensagens | select | n8n_fila_mensagens |
| `c6b038d8...` | Limpar fila de mensagens | delete | n8n_fila_mensagens |
| `d228bdf9...` | Enfileirar mensagem | insert | n8n_fila_mensagens |
| `d8560446...` | Conversa Ativa | select | n8n_active_conversation |
| `f89511d6...` | Salvar Espera | upsert | n8n_active_conversation |
| `3131af47...` | Conversa ativa atualizada | select | n8n_active_conversation |
| `90a128fb...` | Termino de resposta | delete | n8n_active_conversation |
| `d5c47233...` | Atualizar resposta IA | upsert | n8n_active_conversation |
| `99152c83...` | Memoria IA | insert | n8n_historico_mensagens |
| `62aa871a...` | Memoria Lead | select | n8n_historico_mensagens |
| `6dfe71c8...` | Mensagem anteriores | select | n8n_historico_mensagens |
| `0d08ba2d...` | historico_mensagens_leads | executeQuery | crm_historico_mensagens |
| `0cae2f0e...` | Salvar Inicio IA | upsert | n8n_active_conversation |
| `082915f5...` | Salvar registro - marcos | executeQuery | ops_schedule_tracking |
| `1b01e85f...` | Salvar registro - alan | executeQuery | n8n_schedule_tracking |
| `03958c2b...` | Limpar memoria | delete | n8n_historico_mensagens |
| `1c54ac44...` | Limpar fila de mensagens1 | delete | n8n_fila_mensagens |
| `65ce5db2...` | Resetar status atendimento | upsert | n8n_active_conversation |
| `f2684337...` | Buscar Agente Ativo | executeQuery | agent_versions + locations |

### Nos HTTP Request (16 nos - 13.9%)

| ID | Nome | Metodo | Endpoint |
|----|------|--------|----------|
| `94639e3c...` | Instagram | POST | /conversations/messages |
| `54980c5b...` | Whatsapp | POST | /conversations/messages |
| `3ab2663c...` | Download audio | GET | URL dinamica |
| `781bfd6b...` | Search Contact | GET | /contacts/{id} |
| `7f184ebf...` | Instagram2 | POST | /conversations/messages |
| `d00a6a26...` | Whatsapp2 | POST | /conversations/messages |
| `9650f9b9...` | Update Contact (Outbound)2 | GET | /contacts/{id} |
| `6d921c02...` | Buscar Conversa do Contato | GET | /conversations/search |
| `cc88b517...` | Update Contact (Outbound) | GET | /contacts/{id} |
| `909e65a3...` | ativar_ia2 | PUT | /contacts/{id} |
| `e1f52353...` | Listar campos customizados | GET | /locations/.../customFields |
| `9d7bc33a...` | 5 Atualizar Carreira | PUT | /contacts/{id} |
| `30fa385d...` | 5 Atualizar Consultoria | PUT | /contacts/{id} |
| `75cf217d...` | 5 Perguntar Objetivo (SMS) | POST | /conversations/messages |
| `0d5d114d...` | Instagram4 | POST | /conversations/messages |
| `c95162d2...` | GHL - Criar Campo (DESABILITADO) | POST | /locations/.../customFields |

### Nos Code (15 nos - 13.0%)

| ID | Nome | Funcao |
|----|------|--------|
| `0c9429d0...` | Mensagem encavalada? | Detecta mensagens em sequencia |
| `d274f5c1...` | Deduplica Mensagens | Remove duplicatas por hash |
| `83d94181...` | Preparar Mensagem | Sanitiza texto para memoria |
| `995b1e60...` | Code in JavaScript | Extrai ID conversa |
| `d63d037f...` | Extrair a extensao | Detecta tipo audio |
| `984d5b6e...` | Calcular Custo LLM | Estima tokens/custo |
| `e3cf8233...` | Extrair IDs dos campos | Parse custom fields |
| `eefbeadb...` | Detectar Objetivo | Classifica objetivo lead |
| `9c3a06ca...` | Classificar Tipo Mensagem | Texto/Audio/Imagem |
| `f94f1796...` | Code1 | Gera timestamps |
| `13417ca6...` | Normalizar Dados1 | Normaliza dados lead |
| `2613bf85...` | Normalizar Nome1 | Limpa nome lead |
| `8ec83b65...` | Contexto UTM | Extrai UTM params |
| `652cfa81...` | Code in JavaScript2 | Calcula tokens |
| `a8c6ff15...` | Preparar Execucao + Contexto | Hiperpersonalizacao |

### Nos Langchain/AI (16 nos - 13.9%)

| Tipo | Nos | Modelos |
|------|-----|---------|
| AI Agent | 1 | Gemini 2.5 Pro |
| LLM Chat | 2 | Gemini 2.5 Pro/Flash |
| Tools | 8 | HTTP, Workflow, MCP, Think |
| Parser | 2 | Chain LLM, Structured |
| Outros | 3 | Vision (Anthropic), Audio (OpenAI) |

---

## 2.4 Padroes Arquiteturais Identificados

### Padrao 1: Fila de Mensagens (Anti-Encavalamento)

**Problema:** Leads enviam multiplas mensagens em sequencia antes da IA responder.

**Solucao:**
```
Mensagem -> Enfileirar -> Esperar 18s -> Buscar todas -> Deduplica -> Processa ultima
```

**Nos Envolvidos:**
1. `Enfileirar mensagem` - INSERT na fila
2. `Esperar` - Wait 18 segundos
3. `Buscar mensagens` - SELECT todas do lead
4. `Mensagem encavalada?` - Verifica se e a ultima
5. `Limpar fila` - DELETE apos processar

### Padrao 2: Controle de Concorrencia (Lock)

**Problema:** Multiplos processos tentando responder ao mesmo lead.

**Solucao:**
```
Verificar status -> Se ativo, aguardar -> Se inativo, assumir lock
```

**Logica:**
- Tabela `n8n_active_conversation` com status e process_id
- Timeout de 1 minuto para considerar inativo
- Maximo de 10 retries antes de desistir

### Padrao 3: Processamento de Midia

**Audio:**
```
Download -> Extract -> Detectar extensao -> Convert -> Whisper -> Texto
```

**Imagem:**
```
URL -> Claude Vision -> Descricao textual
```

### Padrao 4: AI Agent Modular

**Arquitetura:**
- Agente central (Gemini 2.5 Pro)
- 8 tools conectadas como sub-workflows
- Think tool para raciocinio interno
- MCP Client para busca externa

### Padrao 5: Roteamento Multi-Canal

```
Canal Switch:
  - source = 'whatsapp' -> API GHL tipo "SMS"
  - source = 'instagram' -> API GHL tipo "IG"
```

### Padrao 6: Reset de Conversa

```
Comando /reset -> Limpar memoria -> Limpar fila -> Resetar status -> Confirmar
```

---

## 2.5 Grupos Funcionais

### Posicionamento Visual no Editor n8n

| Grupo | Posicao X | Funcao |
|-------|-----------|--------|
| 1. Entrada/Normalizacao | -1040 a 400 | Webhook + transformacoes iniciais |
| 2. Roteamento Principal | 400 a 1200 | Switch por tipo, verificacao IA |
| 3. Processamento Midia | 1600 a 2800 | Audio/Imagem |
| 4. Fila de Mensagens | 1400 a 3000 | Enfileiramento e deduplicacao |
| 5. Preparacao AI | 3700 a 5500 | Memoria, contexto |
| 6. AI Agent | 5500 a 7000 | Agente + tools |
| 7. Formatacao | 7000 a 8800 | Parser, segmentacao |
| 8. Envio | 9500 a 11000 | Loop, canal, tracking |
| 9. Outbound/Reset | 1200 a 3400 (Y neg) | Fluxos alternativos |
| 10. Objetivo Lead | 1200 a 2500 (Y neg) | Carreira/Consultoria |

---

# 3. JORNADA DO DADO

## 3.1 Fluxo Completo Ponta-a-Ponta

### Happy Path (Mensagem de Texto)

```
ENTRADA
    |
    v
+--------------------------------------------+
| 1. Webhook recebe POST do GHL              |
|    Body: contact_id, message, type, etc.   |
+--------------------------------------------+
    |
    v
+--------------------------------------------+
| 2. NORMALIZACAO (6 nos)                    |
|    - Extrai UTM                            |
|    - Capitaliza nome                       |
|    - Define objetivo, agente_ia            |
|    - Adiciona timestamps                   |
|    - Classifica tipo mensagem              |
|    - Consolida em Info (60+ campos)        |
+--------------------------------------------+
    |
    v
+--------------------------------------------+
| 3. VALIDACAO                               |
|    - Verifica se IA esta ativa             |
|    - Roteia por tipo de mensagem           |
+--------------------------------------------+
    |
    v
+--------------------------------------------+
| 4. FILA DE MENSAGENS                       |
|    - Enfileira mensagem no Postgres        |
|    - Espera 18 segundos                    |
|    - Busca todas mensagens do lead         |
|    - Verifica se encavalada                |
|    - Limpa fila se OK                      |
+--------------------------------------------+
    |
    v
+--------------------------------------------+
| 5. CONTROLE DE CONCORRENCIA                |
|    - Verifica conversa ativa               |
|    - Se ocupada: aguarda 15s + retry       |
|    - Se livre: assume lock                 |
|    - Salva inicio IA                       |
+--------------------------------------------+
    |
    v
+--------------------------------------------+
| 6. PREPARACAO IA                           |
|    - Busca mensagens anteriores            |
|    - Prepara formato LangChain             |
|    - Insere mensagem atual na memoria      |
|    - Deduplica historico                   |
|    - Busca agente ativo                    |
|    - Monta contexto hiperpersonalizado     |
+--------------------------------------------+
    |
    v
+--------------------------------------------+
| 7. AI AGENT                                |
|    - Executa Gemini 2.5 Pro                |
|    - Pode chamar tools:                    |
|      * Buscar disponibilidade              |
|      * Agendar reuniao                     |
|      * Atualizar campos                    |
|      * Adicionar tags                      |
|    - Gera resposta textual                 |
+--------------------------------------------+
    |
    v
+--------------------------------------------+
| 8. VALIDACAO DE RESPOSTA                   |
|    - Verifica se nao e vazia               |
|    - Verifica se nao tem codigo            |
|    - Verifica se nao tem JSON vazado       |
+--------------------------------------------+
    |
    v
+--------------------------------------------+
| 9. FORMATACAO                              |
|    - Parser Chain (Gemini Flash)           |
|    - Converte markdown para WhatsApp       |
|    - Divide em 1-3 mensagens               |
|    - Calcula custo de tokens               |
+--------------------------------------------+
    |
    v
+--------------------------------------------+
| 10. ENVIO                                  |
|    - Loop para cada mensagem               |
|    - Salva na memoria IA                   |
|    - Roteia por canal (WA/IG)              |
|    - POST para GHL API                     |
|    - Wait 1.5s entre mensagens             |
+--------------------------------------------+
    |
    v
+--------------------------------------------+
| 11. FINALIZACAO                            |
|    - Salva execution data                  |
|    - Chama workflow de tracking            |
|    - FIM                                   |
+--------------------------------------------+
```

---

## 3.2 Transformacoes Criticas

### Transformacao 1: Webhook -> Info

**Input (Webhook):**
```json
{
  "body": {
    "contact_id": "abc123",
    "first_name": "john",
    "full_name": "john DOE",
    "email": "john@example.com",
    "phone": "+1234567890",
    "tags": "lead,utm_carreira",
    "location": {
      "id": "loc123",
      "name": "Mottivme EUA"
    },
    "message": {
      "type": 1,
      "body": "Oi, gostaria de saber sobre carreira"
    },
    "customData": {
      "ghl_api_key": "sk-xxx...",
      "ativar_ia": "sim",
      "objetivodolead": "carreira"
    }
  }
}
```

**Output (Info):**
```json
{
  "lead_id": "abc123",
  "first_name": "John",
  "full_name": "John Doe",
  "telefone": "+1234567890",
  "email": "john@example.com",
  "location_id": "loc123",
  "location_name": "Mottivme EUA",
  "api_key": "sk-xxx...",
  "mensagem": "Oi, gostaria de saber sobre carreira",
  "mensagem_id": "1735689600000",
  "datetime": "2025-12-31T12:00:00.000Z",
  "source": "whatsapp",
  "n8n_ativo": true,
  "etapa_funil": "Novo Lead",
  "process_id": "exec_abc123",
  "workflow_id": "R2fVs2qpct1Qr2Y1",
  "agente_ia": "sdrcarreira",
  "ativar_ia": "sim",
  "etiquetas": "lead,utm_carreira",
  "objetivo_do_lead": "carreira",
  "starttimeISO": "2025-12-31T12:00:00.000Z",
  "starttimeMs": 1735689600000,
  "endtimeISO": "2026-01-07T12:00:00.000Z",
  "endtimeMs": 1736294400000
}
```

### Transformacao 2: Mensagem -> Formato LangChain

**Input:**
```javascript
$('Info').first().json.mensagem
// "Oi, gostaria de saber sobre carreira"
```

**Output:**
```json
{
  "lead_id": "abc123",
  "session_id": "abc123",
  "message": {
    "type": "human",
    "content": "Oi, gostaria de saber sobre carreira",
    "tool_calls": [],
    "additional_kwargs": {},
    "response_metadata": {},
    "invalid_tool_calls": []
  }
}
```

### Transformacao 3: Historico -> Contexto Formatado

**Input (Array de mensagens):**
```json
[
  {"created_at": "2025-12-31T10:00:00Z", "message": {"type": "human", "content": "Oi"}},
  {"created_at": "2025-12-31T10:01:00Z", "message": {"type": "ai", "content": "Ola! Como posso ajudar?"}}
]
```

**Output (String formatada):**
```
[2025-12-31T10:00:00Z] Lead/Humano: Oi

[2025-12-31T10:01:00Z] Assistente/IA: Ola! Como posso ajudar?
```

### Transformacao 4: Resposta IA -> Mensagens WhatsApp

**Input (AI Agent output):**
```
**Ola John!**

Que otimo receber sua mensagem. Para te ajudar melhor com sua **carreira nos EUA**, preciso entender algumas coisas:

1. Voce ja possui work permit?
2. Qual sua area de atuacao?
3. Qual estado voce mora?

Me conta mais!
```

**Output (Parser Chain):**
```json
{
  "messages": [
    "Ola John!",
    "Que otimo receber sua mensagem. Para te ajudar melhor com sua *carreira nos EUA*, preciso entender algumas coisas:",
    "1. Voce ja possui work permit?\n2. Qual sua area de atuacao?\n3. Qual estado voce mora?\n\nMe conta mais!"
  ]
}
```

---

## 3.3 Data Lineage

### Diagrama de Linhagem de Dados

```mermaid
graph LR
    subgraph FONTES["Fontes de Dados"]
        WH[Webhook GHL]
        DB[(PostgreSQL)]
        API[APIs LLM]
    end

    subgraph TRANSFORMACOES["Transformacoes"]
        T1[Normalizar]
        T2[Enriquecer]
        T3[Processar IA]
        T4[Formatar]
    end

    subgraph DESTINOS["Destinos"]
        HIST[(Historico Mensagens)]
        CRM[(CRM Historico)]
        METRICS[(Execution Metrics)]
        GHL_OUT[GHL API - Envio]
    end

    WH --> T1
    T1 --> T2
    DB --> T2
    T2 --> T3
    API --> T3
    T3 --> T4
    T4 --> GHL_OUT

    T2 --> HIST
    T1 --> CRM
    T1 --> METRICS
    T3 --> HIST
```

### Fluxo de Dados por Tabela

| Tabela | Entrada | Saida | Retencao |
|--------|---------|-------|----------|
| `n8n_fila_mensagens` | INSERT ao receber | DELETE apos processar | Temporario (~18s) |
| `n8n_active_conversation` | UPSERT ao iniciar | DELETE ao finalizar | Temporario (~1min) |
| `n8n_historico_mensagens` | INSERT lead + IA | SELECT para contexto | Permanente |
| `crm_historico_mensagens` | INSERT apos normalizar | N/A (backup) | Permanente |
| `execution_metrics` | INSERT no inicio | N/A (analytics) | Permanente |
| `ops_schedule_tracking` | UPSERT por etapa | N/A (tracking) | Permanente |

---

## 3.4 Ciclo de Vida dos Dados

### Mensagem do Lead

```
1. RECEPCAO (Webhook)
   |
   +---> n8n_fila_mensagens (temporario, ~18s)
   |
   +---> crm_historico_mensagens (permanente, backup)
   |
   +---> n8n_historico_mensagens (permanente, memoria IA)
             |
             +---> session_id = lead_id
             +---> message.type = "human"
             +---> message.content = texto normalizado
```

### Resposta da IA

```
1. GERACAO (AI Agent)
   |
   +---> n8n_active_conversation.output_preview (temporario)
   |
   +---> n8n_historico_mensagens (permanente)
             |
             +---> session_id = lead_id
             +---> message.type = "ai"
             +---> message.content = resposta formatada
   |
   +---> GHL API (envio imediato)
```

### Metricas

```
1. COLETA (Execution Data nodes)
   |
   +---> execution_metrics (permanente)
   |
   +---> ops_schedule_tracking (permanente)
   |
   +---> Workflow [TOOL] Registrar Custo IA (permanente)
```

---

# 4. INTEGRACOES E DEPENDENCIAS

## 4.1 Sistemas Externos

### Diagrama de Dependencias

```mermaid
graph TD
    subgraph "Workflow n8n"
        N8N[GHL - Mottivme - EUA Versionado]
    end

    subgraph "APIs Externas"
        GHL[GoHighLevel API]
        GEMINI[Google Gemini]
        OPENAI[OpenAI Whisper]
        CLAUDE[Anthropic Claude]
        CW[ChatWoot]
        MCP[MCP Server]
    end

    subgraph "Banco de Dados"
        PG[(PostgreSQL / Supabase)]
    end

    subgraph "Sub-workflows"
        TW1[Busca Disponibilidade]
        TW2[Agendar Reuniao]
        TW3[Atualizar Work Permit]
        TW4[Atualizar Profissao]
        TW5[Atualizar Estado]
        TW6[Registrar Custo IA]
    end

    N8N --> GHL
    N8N --> GEMINI
    N8N --> OPENAI
    N8N --> CLAUDE
    N8N --> CW
    N8N --> MCP
    N8N --> PG

    N8N --> TW1
    N8N --> TW2
    N8N --> TW3
    N8N --> TW4
    N8N --> TW5
    N8N --> TW6

    TW1 --> GHL
    TW2 --> GHL
    TW3 --> GHL
    TW4 --> GHL
    TW5 --> GHL
    TW6 --> PG
```

### Matriz de Integracoes

| Sistema | Endpoints | Autenticacao | Retry | Criticidade |
|---------|-----------|--------------|-------|-------------|
| GoHighLevel | 8+ | Bearer Token | Sim | **CRITICA** |
| PostgreSQL | 20+ | Credencial n8n | Sim | **CRITICA** |
| Google Gemini | 3 | API Key | Padrao | ALTA |
| OpenAI | 1 | API Key | Padrao | MEDIA |
| Anthropic | 1 | API Key | Padrao | MEDIA |
| ChatWoot | 1 | API Key | Sim | BAIXA |
| MCP Server | 1 | SSE Endpoint | Sim | MEDIA |
| Tool Workflows | 6 | Interno n8n | N/A | ALTA |

---

## 4.2 APIs e Endpoints

### GoHighLevel API

| Endpoint | Metodo | Uso | No |
|----------|--------|-----|-----|
| `/contacts/{id}` | GET | Buscar contato | Search Contact |
| `/contacts/{id}` | PUT | Atualizar contato | Update Contact, ativar_ia2 |
| `/conversations/messages` | POST | Enviar mensagem | Whatsapp, Instagram |
| `/conversations/search` | GET | Buscar conversa | Buscar Conversa |
| `/locations/{id}/customFields` | GET | Listar campos | Listar campos customizados |

**Base URL:** `https://services.leadconnectorhq.com`
**Versao:** `2021-04-15` ou `2021-07-28`
**Autenticacao:** `Authorization: Bearer {api_key}`

### Google Gemini API

| Uso | Modelo | No |
|-----|--------|-----|
| AI Agent principal | gemini-2.5-pro | AI Agent - Modular |
| Parser de mensagens | gemini-2.5-flash | Parser Chain |

### OpenAI API

| Uso | Modelo | No |
|-----|--------|-----|
| Transcricao de audio | whisper | Transcrever audio |

**Parametros:** `language: "pt"`

### Anthropic API

| Uso | Modelo | No |
|-----|--------|-----|
| Analise de imagens | claude-sonnet-4-5-20250929 | Analyze image |

---

## 4.3 Credenciais

| ID | Nome | Tipo | Nos que Usam |
|----|------|------|--------------|
| `w2mBaRwhZ3tM4FUw` | Postgres Marcos Daniels. | PostgreSQL | 19 nos |
| `B0fAAM3acruSSuiz` | Postgres account | PostgreSQL | 1 no |
| `4ut0CD80SN7lbITM` | Google Gemini(PaLM) Api | Google Gemini | 2 nos |
| `WEENPovt22LUaeRp` | OpenAi - Marcos | OpenAI | 1 no |
| `nNkFTZpNoiBCbO1I` | Anthropic account | Anthropic | 1 no |
| `UmVE5jAScA8a8vNB` | ChatWoot account | ChatWoot | 1 no |

### API Keys Dinamicas

- **GHL API Key:** Vem via `$json.body.customData.ghl_api_key` no webhook
- **Propagacao:** Armazenada em `$('Info').first().json.api_key`
- **Uso:** Headers de todas chamadas GHL

---

## 4.4 Pontos de Falha

### Analise de SPOF (Single Point of Failure)

| Componente | SPOF? | Impacto | Mitigacao Atual | Recomendacao |
|------------|-------|---------|-----------------|--------------|
| PostgreSQL | **SIM** | Total | Nenhuma | Replica/Failover |
| Google Gemini | **SIM** | AI falha | Nenhuma | Fallback OpenAI |
| GHL API | **SIM** | Envio falha | Retry basico | Circuit breaker |
| n8n Server | **SIM** | Todos param | Nenhuma | Cluster HA |
| MCP Server | **SIM** | Tool falha | Retry | Fallback local |

### Fallbacks Implementados

| Componente | Fallback | Status |
|------------|----------|--------|
| Analise de imagem | `continueRegularOutput` | **PARCIAL** |
| Gemini -> OpenAI | N/A | **NAO** |
| GHL -> Fila | N/A | **NAO** |
| Postgres -> Cache | N/A | **NAO** |

### SLAs Esperados

| API | Latencia | Disponibilidade |
|-----|----------|-----------------|
| GHL | < 500ms | 99.9% |
| PostgreSQL | < 100ms | 99.99% |
| Gemini | 2-10s | 99.9% |
| Whisper | 2-5s | 99.9% |
| Claude | 2-8s | 99.9% |

---

# 5. LOGICA DE NEGOCIO

## 5.1 Maquina de Estados

### Estados do Lead

| Estado | Descricao | Condicao de Entrada |
|--------|-----------|---------------------|
| **Novo Lead** | Primeira mensagem | Webhook sem historico |
| **Em Fila** | Aguardando processamento | Mensagens em sequencia |
| **Aguardando IA** | IA processando | `status = 'active'` |
| **Processando** | Gerando resposta | Apos `Salvar Inicio IA` |
| **Respondendo** | Enviando mensagem | Apos `Parser Chain` |
| **Inativo** | Conversa finalizada | `status = 'inactive'` ou timeout |
| **Bloqueado** | IA desativada | `n8n_ativo = false` |
| **Perdido** | Desqualificado | Tag "perdido" |

### Diagrama de Transicoes

```mermaid
stateDiagram-v2
    [*] --> NovoLead: Webhook recebido
    NovoLead --> Validacao: Extrair contexto

    Validacao --> Bloqueado: n8n_ativo = false
    Validacao --> EmFila: Mensagem valida

    EmFila --> AguardandoProcessamento: Wait 18s
    AguardandoProcessamento --> VerificandoEncavalamento: Buscar mensagens

    VerificandoEncavalamento --> Descartado: Mensagem encavalada
    VerificandoEncavalamento --> VerificandoConversa: Mensagem unica

    VerificandoConversa --> IniciarConversa: Sem conversa ativa
    VerificandoConversa --> Aguardar: Conversa ativa, outro processo
    VerificandoConversa --> Ignorar: Retries > 10

    Aguardar --> VerificandoConversa: Wait 15s + retry
    IniciarConversa --> ProcessandoIA: Salvar inicio

    ProcessandoIA --> GerandoResposta: AI Agent
    GerandoResposta --> ValidandoResposta: Resposta gerada

    ValidandoResposta --> FormatandoMensagem: Resposta valida
    ValidandoResposta --> Descartado: Resposta invalida

    FormatandoMensagem --> Enviando: Parser Chain
    Enviando --> Enviado: Canal (WA/IG)

    Enviado --> ProximaMsg: Loop se multiplas
    Enviado --> Finalizado: Ultima mensagem

    ProximaMsg --> Enviando: Wait 1.5s
    Finalizado --> [*]

    Bloqueado --> [*]
    Descartado --> [*]
    Ignorar --> [*]
```

---

## 5.2 Regras de Negocio Formais

### RN-001: Controle de Concorrencia

**Descricao:** Apenas um processo pode responder a um lead por vez.

**Razao:** Evitar respostas duplicadas e confusao do lead.

**Implementacao:**
- Tabela `n8n_active_conversation` com status/waiting_process_id
- No "Acao Planejada" decide iniciar, aguardar ou ignorar
- Timeout de 1 minuto considera conversa inativa
- Maximo de 10 retries evita loop infinito

---

### RN-002: Consolidacao de Mensagens

**Descricao:** Esperar 18s antes de processar para consolidar mensagens rapidas.

**Razao:** Leads frequentemente enviam multiplas mensagens seguidas.

**Implementacao:**
- Tabela `n8n_fila_mensagens` armazena mensagens temporariamente
- No "Esperar" aguarda 18 segundos
- No "Mensagem encavalada?" verifica se e a ultima
- Apenas a ultima mensagem e processada

**Excecoes:** Comandos `/reset` e `/teste` nao passam pela fila.

---

### RN-003: Filtragem de Respostas Tecnicas

**Descricao:** Bloquear envio de respostas com codigo/JSON vazado.

**Razao:** Leads nao devem receber conteudo tecnico.

**Implementacao:**
- No "If1" verifica presenca de: json, {, output, parsed, split, properties, type
- Se detectado, resposta e reformatada pelo Parser Chain

---

### RN-004: Roteamento por Canal

**Descricao:** Resposta deve ser enviada pelo mesmo canal de origem.

**Razao:** Manter contexto da conversa.

**Implementacao:**
- Campo `source` identifica canal (whatsapp/instagram)
- No "Canal" roteia para API correta (SMS/IG)

---

### RN-005: Limite de Retries

**Descricao:** Maximo de 10 tentativas de processar mesma mensagem.

**Razao:** Evitar loops infinitos e consumo excessivo de recursos.

**Implementacao:**
- Campo `retries` na tabela `n8n_active_conversation`
- Se retries > 10, mensagem e ignorada

---

### RN-006: Timeout de Conversa

**Descricao:** Conversa considerada inativa apos 1 minuto.

**Razao:** Liberar lock para novos processamentos.

**Implementacao:**
- Verificacao: `(new Date() - created_at) > 60000ms`
- Se expirada, nova conversa pode iniciar

---

### RN-007: Processamento de Midia

**Descricao:** Imagens e audios sao convertidos em texto antes do processamento.

**Razao:** IA precisa de contexto textual para responder.

**Implementacao:**
- Imagens: Claude Sonnet Vision extrai descricao
- Audios: Whisper transcreve para portugues

---

### RN-008: Memoria de Conversa

**Descricao:** Historico de mensagens e mantido por session_id (lead_id).

**Razao:** IA precisa de contexto para respostas coerentes.

**Implementacao:**
- Tabela `n8n_historico_mensagens`
- Deduplicacao por timestamp + conteudo (primeiros 100 chars)
- Comando `/reset` limpa memoria

---

### RN-009: Espacamento de Mensagens

**Descricao:** Intervalo de 1.5s entre mensagens multiplas.

**Razao:** Evitar rate limiting e melhorar UX.

**Implementacao:**
- No "1.5s" entre cada iteracao do Loop
- Mensagens chegam em sequencia natural

---

### RN-010: Qualificacao por Objetivo

**Descricao:** Leads sao direcionados para consultoria ou carreira baseado no work permit.

**Razao:** Diferentes necessidades requerem diferentes abordagens.

**Implementacao:**
- Campo `workPermitValue` define calendario
- Switch Objetivo roteia para Carreira/Consultoria/Indefinido

---

## 5.3 Decisoes Condicionais

### Decision Tree Principal

```mermaid
graph TD
    Start[Webhook Recebido] --> UTM[Extrair Contexto UTM]
    UTM --> Ativar{IA Ativada?<br/>ativar_ia=sim OR<br/>tag=assistente-admin}

    Ativar -->|Nao| FIM1[Fim: IA Desativada]
    Ativar -->|Sim| TipoMsg{Tipo de Mensagem?}

    TipoMsg -->|/reset| Reset[Limpar Memoria]
    TipoMsg -->|/teste| Teste[Fluxo de Teste]
    TipoMsg -->|Vazia| FIM2[Fim: Ignorar]
    TipoMsg -->|Texto| Fila[Enfileirar Mensagem]
    TipoMsg -->|Imagem| Vision[Claude Vision]
    TipoMsg -->|Audio| Whisper[Transcrever Audio]

    Fila --> Espera[Wait 18s]
    Espera --> BuscaMsg[Buscar Mensagens]
    BuscaMsg --> Encavalada{Mensagem Encavalada?}

    Encavalada -->|Sim| FIM3[Fim: Processar Ultima]
    Encavalada -->|Nao| Acao{Status Conversa?}

    Acao -->|Inativa/Nova| Permitido{n8n_ativo AND msg != okkk?}
    Acao -->|Retries > 10| FIM4[Fim: Limite]
    Acao -->|Ativa outro| Aguardar[Wait 15s + Retry]

    Permitido -->|Nao| FIM5[Fim: Bloqueada]
    Permitido -->|Sim| AIAgent[AI Agent]

    AIAgent --> ValidaOutput{Output Valido?}
    ValidaOutput -->|Vazio| FIM6[Fim: Invalida]
    ValidaOutput -->|OK| DeveEnviar{Deve Enviar?}

    DeveEnviar -->|Nao| FIM7[Fim: Outro Processo]
    DeveEnviar -->|Sim| ValidaTecnico{Contem Codigo?}

    ValidaTecnico -->|Sim| Parser[Reformatar]
    ValidaTecnico -->|Nao| Enviar[Enviar]

    Parser --> Enviar
    Enviar --> Canal{Canal?}

    Canal -->|WhatsApp| WA[API SMS]
    Canal -->|Instagram| IG[API IG]
```

### Condicoes Detalhadas

| No | Condicao | Rotas |
|----|----------|-------|
| IA Ativa? | `ativar_ia === 'sim' OR etiquetas.includes('assistente-admin')` | TRUE -> Processar, FALSE -> Fim |
| Tipo mensagem | `mensagem.includes('/reset')`, `tipo === 'audio'`, etc. | 6 saidas |
| Acao Planejada | `isEmpty() OR status === 'inactive' OR timeout > 1min` | Iniciar/Aguardar/Ignorar |
| Permitido AI? | `n8n_ativo !== false AND mensagem !== 'okkk'` | TRUE -> Processar |
| If1 (codigo) | `!output.includes('json') AND !output.includes('{')...` | TRUE -> Enviar, FALSE -> Parser |
| Canal | `source === 'whatsapp'` | WhatsApp/Instagram |

---

## 5.4 Tools do AI Agent

### Ferramentas Disponiveis

| Tool | Tipo | Workflow ID | Descricao |
|------|------|-------------|-----------|
| **Busca_disponibilidade** | toolWorkflow | pZIcRI1PGMzbQHZZ | Busca slots no calendario GHL |
| **Agendar_reuniao** | toolWorkflow | u1UsmjNNpaEiwIsp | Cria agendamento (requer nome, tel, email, data, hora) |
| **Atualizar Work Permit** | toolWorkflow | 3Dd8d5AnpD4iLPwG | Atualiza campo work permit (Sim/Nao) |
| **Atualizar Profissao** | toolWorkflow | Kq3b79P6v4rTsiaH | Atualiza campo profissao |
| **Atualizar Estado** | toolWorkflow | wsQQYmx8CLNBHoWq | Atualiza estado onde mora (Florida, Texas, etc.) |
| **Adicionar_tag_perdido** | toolHttpRequest | - | Adiciona tag "perdido" ao contato GHL |
| **Busca historias** | mcpClientTool | - | Busca historias de sucesso via MCP |
| **Think** | toolThink | - | Raciocinio interno (chain of thought) |

### Parametros das Tools

**Busca_disponibilidade:**
```json
{
  "calendar": "ID do calendario",
  "API_KEY": "API Key do GHL",
  "startDate": "timestamp_ms",
  "endDate": "timestamp_ms (7 dias)",
  "lead_id": "ID do lead"
}
```

**Agendar_reuniao:**
```json
{
  "API_KEY": "string",
  "email": "string",
  "telefone": "+00000000000",
  "startTime": "2021-06-23T03:30:00+05:30",
  "firstName": "string",
  "lastName": "string",
  "lead_id": "string",
  "Carreira_Consultoria": "carreira|consultoria"
}
```

---

# 6. SEGURANCA E CONFIABILIDADE

## 6.1 Vulnerabilidades Criticas

### VULN-001: Webhook Sem Autenticacao

**Severidade:** CRITICA

**Descricao:** O webhook aceita qualquer POST na rota `/factor-ai` sem verificacao de origem.

**Impacto:**
- Atacante pode enviar mensagens falsas
- DDoS facilmente executavel
- Dados podem ser exfiltrados

**Configuracao Atual:**
```json
{
  "type": "n8n-nodes-base.webhook",
  "parameters": {
    "httpMethod": "POST",
    "path": "factor-ai",
    "options": {}  // SEM AUTENTICACAO
  }
}
```

**Correcao Recomendada:**
```javascript
// Adicionar no primeiro no Code
const expectedToken = $env.GHL_WEBHOOK_SECRET;
const receivedToken = $input.first().json.headers['x-ghl-signature'];
if (receivedToken !== expectedToken) {
  throw new Error('Unauthorized webhook call');
}
```

---

### VULN-002: SQL Injection

**Severidade:** CRITICA

**Descricao:** Query em `historico_mensagens_leads` usa interpolacao direta de variaveis.

**Query Vulneravel:**
```sql
INSERT INTO public.crm_historico_mensagens
(lead_id, mensagem, datetime, source, full_name)
VALUES
('{{ $json.lead_id }}', '{{ $json.mensagem }}', '{{ $json.datetime }}', '{{ $json.source }}', '{{ $json.full_name }}')
```

**Ataque Exemplo:**
Um lead pode enviar mensagem contendo:
```
'); DROP TABLE crm_historico_mensagens; --
```

**Correcao Recomendada:**
```sql
INSERT INTO public.crm_historico_mensagens
(lead_id, mensagem, datetime, source, full_name)
VALUES ($1, $2, $3, $4, $5)
```

---

### VULN-003: API Keys em Logs

**Severidade:** ALTA

**Descricao:** API key do GHL e incluida em strings de session tracking.

**Localizacao:**
```javascript
// No "GetInfo"
"value": "=etapa,...,{{ $('Info').first().json.api_key || 'NULL' }},..."
```

**Impacto:**
- API keys expostas em logs do n8n
- Podem ser acessadas por quem tem acesso ao sistema
- Risco de comprometimento de multiplas locations

**Correcao Recomendada:**
- Remover `api_key` de strings de log
- Usar apenas hash ou referencia

---

## 6.2 Matriz de Risco

| Risco | Probabilidade | Impacto | Severidade | Status |
|-------|---------------|---------|------------|--------|
| SQL Injection | Media | Critico | **CRITICO** | Nao mitigado |
| Webhook nao autenticado | Alta | Alto | **CRITICO** | Nao mitigado |
| Vazamento de API key | Media | Alto | **ALTO** | Nao mitigado |
| Falha sem notificacao | Alta | Medio | **ALTO** | Nao mitigado |
| Duplicacao de mensagem | Media | Medio | **MEDIO** | Parcialmente mitigado |
| Dados sem expurgo | Alta | Baixo | **MEDIO** | Nao mitigado |
| Circuit breaker ausente | Media | Medio | **MEDIO** | Nao mitigado |

---

## 6.3 Recomendacoes Prioritarias

### 1. Autenticar Webhook (CRITICO)

**Esforco:** Baixo (1-2 horas)

```javascript
// Adicionar no inicio do workflow
const secret = $env.GHL_WEBHOOK_SECRET;
const signature = $input.first().json.headers['x-ghl-signature'];

if (!signature || signature !== secret) {
  return { error: 'Unauthorized' };
}
```

### 2. Corrigir SQL Injection (CRITICO)

**Esforco:** Baixo (30 minutos)

Alterar no "historico_mensagens_leads":
- De: Template string com `{{ }}`
- Para: Parametros posicionais `$1, $2, $3`

### 3. Criar Error Workflow (ALTO)

**Esforco:** Medio (2-4 horas)

1. Criar workflow separado para erros
2. Enviar notificacao via Slack/Email
3. Registrar contexto da falha
4. Vincular via configuracao do workflow

### 4. Adicionar Retry no AI Agent (ALTO)

**Esforco:** Baixo (15 minutos)

```json
{
  "options": {
    "retryOnFail": true,
    "maxTries": 3,
    "waitBetweenTries": 5000
  }
}
```

### 5. Implementar Circuit Breaker (MEDIO)

**Esforco:** Medio (4-8 horas)

1. Criar tabela `circuit_breaker_state`
2. Verificar estado antes de chamadas externas
3. Abrir circuito apos N falhas consecutivas
4. Half-open apos timeout

---

## 6.4 Compliance LGPD/GDPR

### Dados Pessoais Tratados

| Dado | Categoria | Armazenado | Transmitido Para |
|------|-----------|------------|------------------|
| Nome completo | Pessoal | Postgres | GHL, LLMs |
| Telefone | Pessoal | Postgres | GHL |
| Email | Pessoal | Postgres | GHL |
| Mensagens | Pessoal + Sensivel | Postgres | LLMs |
| Audio | Pessoal | Temporario | OpenAI |
| Imagens | Pessoal | Temporario | Anthropic |
| Localizacao (estado) | Pessoal | Postgres | - |

### Gaps de Compliance

| Requisito | Status | Gap |
|-----------|--------|-----|
| Consentimento | **NAO VERIFICADO** | Assumido do GHL |
| Direito ao Esquecimento | **NAO IMPLEMENTADO** | Sem workflow de delete |
| Retencao de Dados | **INDEFINIDA** | Sem expurgo automatico |
| Portabilidade | **NAO IMPLEMENTADO** | Sem export |
| Minimizacao | **PARCIAL** | Muitos campos armazenados |

### Acoes Recomendadas

1. **Verificar consentimento** - Validar que lead consentiu antes de processar
2. **Implementar delete** - Workflow para apagar dados de um lead
3. **Definir retencao** - Expurgar dados > 2 anos automaticamente
4. **Anonimizar** - Manter apenas dados agregados para analytics

---

# 7. GUIAS PRATICOS

## 7.1 Como Modificar o Workflow

### Alterar Mensagem de Resposta

**Cenario:** Mudar o tom ou estilo das respostas da IA.

**Onde modificar:**
1. No "Preparar Execucao + Identificar Contexto" (`a8c6ff15...`)
2. Secao do system prompt

**Passos:**
1. Localizar o no pelo ID ou nome
2. Editar a variavel `systemPrompt` ou `instrucoes`
3. Testar com `/teste` antes de produzir

### Adicionar Novo Campo Custom

**Cenario:** Capturar um novo campo do GHL.

**Onde modificar:**
1. No "Info" (`f1d208a5...`) - adicionar mapeamento
2. Nos de uso downstream

**Passos:**
1. No no "Info", adicionar novo campo:
   ```javascript
   "novo_campo": "={{ $json.body.customData.novo_campo || '' }}"
   ```
2. Usar `$('Info').first().json.novo_campo` onde necessario

### Mudar Tempo de Espera da Fila

**Cenario:** Leads reclamam de demora ou mensagens perdidas.

**Onde modificar:**
- No "Esperar" (`3924ac37...`)

**Passos:**
1. Localizar o no "Esperar"
2. Alterar `value` de `18` para novo valor
3. Considerar trade-off: menor = mais rapido, maior = mais consolidacao

### Adicionar Nova Tool ao AI Agent

**Cenario:** IA precisa de nova capacidade.

**Passos:**
1. Criar sub-workflow com a funcionalidade
2. Adicionar no toolWorkflow ao AI Agent
3. Configurar parametros de entrada
4. Documentar no system prompt quando usar

---

## 7.2 Como Debugar Problemas

### Problema: Lead Nao Recebe Resposta

**Checklist de Debug:**

1. **Verificar Webhook**
   - Ir em Executions do workflow
   - Procurar execucao pelo contact_id
   - Se nao houver: problema no GHL enviando webhook

2. **Verificar IA Ativa**
   - No "IA Ativa?": qual branch foi tomado?
   - Se FALSE: verificar `ativar_ia` e tags do contato

3. **Verificar Fila**
   - No "Mensagem encavalada?": retornou vazio?
   - Se sim: outra execucao processou a mensagem

4. **Verificar Concorrencia**
   - No "Acao Planejada": qual saida?
   - Se "Ignorar": retries > 10 ou outro processo

5. **Verificar AI Agent**
   - Output do AI Agent e valido?
   - Se vazio ou com erro: verificar logs do Gemini

6. **Verificar Envio**
   - No "Whatsapp" ou "Instagram": houve erro?
   - Verificar response da API GHL

### Problema: Mensagem Duplicada

**Causa Provavel:** Retry apos envio bem-sucedido.

**Debug:**
1. Verificar execucoes no mesmo horario
2. Verificar tabela `n8n_active_conversation`
3. Verificar se fila foi limpa corretamente

**Solucao:**
- Adicionar idempotencia no envio (verificar message_id antes)

### Problema: Resposta com Codigo/JSON

**Causa Provavel:** Parser Chain nao filtrou corretamente.

**Debug:**
1. Verificar output do AI Agent
2. Verificar condicao do "If1"
3. Verificar output do Parser Chain

**Solucao:**
- Melhorar prompt do Parser Chain
- Adicionar mais termos na blacklist do "If1"

### Comandos Uteis para Debug

```sql
-- Verificar fila de mensagens
SELECT * FROM n8n_fila_mensagens
WHERE lead_id = 'xxx'
ORDER BY timestamp DESC;

-- Verificar conversa ativa
SELECT * FROM n8n_active_conversation
WHERE lead_id = 'xxx';

-- Verificar historico
SELECT * FROM n8n_historico_mensagens
WHERE session_id = 'xxx'
ORDER BY created_at DESC
LIMIT 20;
```

---

## 7.3 Como Adicionar Novas Features

### Feature: Suporte a PDF

**Complexidade:** Media

**Passos:**
1. No "Classificar Tipo Mensagem", adicionar deteccao de PDF
2. No "Tipo de mensagem" (Switch), adicionar saida para PDF
3. Criar branch de processamento:
   - Download do arquivo
   - Extracao de texto (usar API como Adobe PDF Extract)
   - Converter para texto
   - Juntar ao fluxo principal

### Feature: Resposta por Email

**Complexidade:** Baixa

**Passos:**
1. No "Canal" (Switch), adicionar saida para email
2. Adicionar no de envio de email (SMTP ou API)
3. Formatar mensagem para email (HTML se necessario)

### Feature: Escalacao para Humano

**Complexidade:** Media

**Passos:**
1. Adicionar deteccao de keywords de escalacao ("falar com humano", etc.)
2. Criar branch de escalacao:
   - Adicionar tag "escalado" no GHL
   - Enviar notificacao para equipe (Slack/Email)
   - Responder lead com confirmacao
3. Desativar IA para esse lead (`ativar_ia = 'nao'`)

### Feature: Multi-idioma

**Complexidade:** Alta

**Passos:**
1. Detectar idioma da mensagem (Language Detection API)
2. Ajustar system prompt dinamicamente
3. Traduzir respostas se necessario
4. Manter historico com idioma original

---

## 7.4 Checklist de Melhores Praticas

### Antes de Modificar

- [ ] Fazer backup do workflow (Export JSON)
- [ ] Testar em ambiente de staging
- [ ] Documentar mudanca proposta
- [ ] Verificar impacto em nos downstream

### Durante Modificacao

- [ ] Manter nomenclatura consistente
- [ ] Adicionar comentarios em nos Code complexos
- [ ] Usar Sticky Notes para documentacao visual
- [ ] Configurar retry em operacoes criticas
- [ ] Configurar error handling

### Apos Modificacao

- [ ] Testar com `/teste` ou lead de teste
- [ ] Verificar logs de execucao
- [ ] Monitorar por 24h apos deploy
- [ ] Atualizar documentacao

### Seguranca

- [ ] Nao expor API keys em logs
- [ ] Usar parametros em queries SQL
- [ ] Validar inputs do webhook
- [ ] Configurar timeout em chamadas HTTP

### Performance

- [ ] Limitar historico enviado ao LLM
- [ ] Usar cache para dados estaticos
- [ ] Configurar timeouts explicitos
- [ ] Evitar loops infinitos

---

# 8. REFERENCIA RAPIDA

## 8.1 Indice de Nos por ID

### Nos Criticos

| ID | Nome | Tipo | Funcao |
|----|------|------|--------|
| `8b088c6c-3506-4bb1-98e1-8549cf75152a` | Mensagem recebida | Webhook | Entry point |
| `f1d208a5-f05b-45bb-ae27-8ceb310344a1` | Info | Set | Consolidacao de dados |
| `0366bcbe-bd9c-4cc1-b70f-32e78cd12ab7` | AI Agent - Modular | Agent | Geracao de respostas |
| `961328dd-7797-409c-98c7-49149ab2d880` | Parser Chain | ChainLLM | Formatacao WA/IG |
| `54980c5b-b84c-4c5b-91ab-72b32682a074` | Whatsapp | HTTP | Envio WA |
| `94639e3c-e072-4d2e-8242-33a1e58e5e4c` | Instagram | HTTP | Envio IG |

### Nos de Decisao

| ID | Nome | Tipo | Descricao |
|----|------|------|-----------|
| `843943ea-7ec3-4b47-a662-6046beea4ae5` | IA Ativa? | If | Verifica permissao |
| `6737ad10-a146-4dde-99a4-39d256540f69` | Tipo de mensagem | Switch | Roteia por tipo |
| `8555b95c-6398-4df7-ac36-a77b34c31448` | Acao Planejada | Switch | Controle concorrencia |
| `020715bd-e226-418c-bdb9-3cc5379446bd` | Canal | Switch | Roteia por canal |

### Nos de Banco

| ID | Nome | Operacao |
|----|------|----------|
| `d228bdf9-3c9e-46d4-afd2-1ab1fad665e5` | Enfileirar mensagem | INSERT |
| `58dd5b6d-7ffb-4940-a292-ba80a8914483` | Buscar mensagens | SELECT |
| `d8560446-bfd9-42a2-b4eb-a2925c88b0fc` | Conversa Ativa | SELECT |
| `f2684337-4b8a-4954-a53d-ffed5ca68eae` | Buscar Agente Ativo | SELECT |
| `99152c83-b21e-4b37-9883-2a8a0aadf30d` | Memoria IA | INSERT |

---

## 8.2 Glossario de Termos

| Termo | Definicao |
|-------|-----------|
| **Mensagem Encavalada** | Multiplas mensagens enviadas antes da IA responder |
| **Conversa Ativa** | Processo da IA em andamento para um lead |
| **waiting_process_id** | ID do processo que esta aguardando sua vez |
| **Work Permit** | Autorizacao de trabalho nos EUA |
| **Carreira** | Tipo de consultoria para quem NAO tem work permit |
| **Consultoria** | Tipo de servico para quem JA tem work permit |
| **Session ID** | Identificador da sessao = lead_id |
| **Parse/Parser** | Formatacao da resposta da IA para envio |
| **Tool** | Funcionalidade que a IA pode executar |
| **MCP** | Model Context Protocol - protocolo de contexto |
| **Chain** | Sequencia de operacoes LangChain |
| **Debounce** | Tecnica de aguardar antes de processar |

---

## 8.3 Variaveis n8n Criticas

### Acesso ao No Info

```javascript
$('Info').first().json.lead_id        // ID do contato GHL
$('Info').first().json.api_key        // API key da location
$('Info').first().json.source         // whatsapp ou instagram
$('Info').first().json.mensagem       // Texto da mensagem
$('Info').first().json.location_id    // ID da location
$('Info').first().json.process_id     // ID unico do processo
$('Info').first().json.workflow_id    // ID do workflow
$('Info').first().json.agente_ia      // Nome do agente
$('Info').first().json.starttimeMs    // Timestamp inicio
$('Info').first().json.endtimeMs      // Timestamp fim (7 dias)
```

### Acesso a Outros Nos

```javascript
$('Parser Chain').item.json.output.messages     // Array de mensagens
$('Memoria Lead').first().json.session_id       // Session ID
$('Mensagem anteriores').all()                   // Historico completo
$('Conversa Ativa').item.json.id                // ID da conversa
$('Conversa Ativa').item.json.retries           // Contador retries
$('Tipo de mensagem1').first().json.output      // Output do agente
```

### Variaveis de Execucao

```javascript
$execution.id           // ID unico da execucao
$execution.mode         // production ou test
$workflow.id            // ID do workflow
$workflow.name          // Nome do workflow
$now.toISO()            // Timestamp ISO atual
$now.format('FFFF')     // Data formatada
```

---

## 8.4 Tabelas do Banco de Dados

### n8n_fila_mensagens

| Coluna | Tipo | Descricao |
|--------|------|-----------|
| id | SERIAL | PK auto |
| lead_id | VARCHAR | ID do lead |
| mensagem | TEXT | Conteudo |
| id_mensagem | VARCHAR | ID unico |
| timestamp | TIMESTAMP | Hora do recebimento |

**Indices:** lead_id, timestamp

### n8n_active_conversation

| Coluna | Tipo | Descricao |
|--------|------|-----------|
| id | SERIAL | PK auto |
| lead_id | VARCHAR | ID do lead |
| workflow_id | VARCHAR | ID do workflow |
| process_id | VARCHAR | ID do processo atual |
| waiting_process_id | VARCHAR | Processo em espera |
| status | VARCHAR | active/inactive |
| retries | INT | Contador de retries |
| output_preview | TEXT | Preview da resposta |
| created_at | TIMESTAMP | Criacao |
| updated_at | TIMESTAMP | Ultima atualizacao |

**Indices:** (lead_id, workflow_id) UNIQUE

### n8n_historico_mensagens

| Coluna | Tipo | Descricao |
|--------|------|-----------|
| id | SERIAL | PK auto |
| session_id | VARCHAR | = lead_id |
| message | JSONB | {type, content, ...} |
| created_at | TIMESTAMP | Hora da mensagem |

**Indices:** session_id, created_at

### crm_historico_mensagens

| Coluna | Tipo | Descricao |
|--------|------|-----------|
| lead_id | VARCHAR | ID do lead |
| mensagem | TEXT | Conteudo |
| datetime | TIMESTAMP | Hora |
| source | VARCHAR | whatsapp/instagram |
| full_name | VARCHAR | Nome do lead |

**Constraint:** UNIQUE (lead_id, mensagem, datetime)

### agent_versions

| Coluna | Tipo | Descricao |
|--------|------|-----------|
| id | SERIAL | PK |
| location_id | VARCHAR | Location do GHL |
| is_active | BOOLEAN | Se esta ativo |
| status | VARCHAR | active/inactive |
| activated_at | TIMESTAMP | Data ativacao |

### locations

| Coluna | Tipo | Descricao |
|--------|------|-----------|
| location_id | VARCHAR | PK |
| api_key | VARCHAR | API key do GHL |

---

# 9. APENDICES

## 9.1 Diagramas Consolidados

### Diagrama de Fluxo Simplificado

```mermaid
graph LR
    A[Webhook] --> B[Normalizar]
    B --> C{IA Ativa?}
    C -->|Sim| D[Fila]
    C -->|Nao| Z[Fim]
    D --> E[AI Agent]
    E --> F[Parser]
    F --> G{Canal}
    G -->|WA| H[WhatsApp]
    G -->|IG| I[Instagram]
    H --> J[Tracking]
    I --> J
    J --> Z
```

### Diagrama de Dados

```mermaid
erDiagram
    WEBHOOK ||--o{ INFO : gera
    INFO ||--o{ FILA : enfileira
    INFO ||--o{ HISTORICO : persiste
    FILA ||--o{ AI_AGENT : alimenta
    AI_AGENT ||--o{ PARSER : produz
    PARSER ||--o{ ENVIO : formata
    ENVIO ||--o{ GHL_API : envia

    INFO {
        string lead_id
        string api_key
        string mensagem
        string source
    }

    FILA {
        string lead_id
        string mensagem
        timestamp created
    }

    HISTORICO {
        string session_id
        json message
        timestamp created
    }
```

### Diagrama de Integracao

```mermaid
graph TB
    subgraph "n8n"
        WF[Workflow]
    end

    subgraph "APIs"
        GHL[GoHighLevel]
        GEM[Gemini]
        OAI[OpenAI]
        ANT[Anthropic]
    end

    subgraph "Storage"
        PG[(PostgreSQL)]
    end

    WF <--> GHL
    WF <--> GEM
    WF <--> OAI
    WF <--> ANT
    WF <--> PG
```

---

## 9.2 Codigo Critico

### Deduplicacao de Mensagens

```javascript
// No: Deduplica Mensagens
const allMessages = $('Mensagem anteriores').all();

const msgArray = allMessages
  .map(item => item.json)
  .filter(item => item && item.created_at && item.message && item.message.content);

// Adiciona mensagem atual
const mensagemAtual = infoData.mensagem;
if (mensagemAtual && mensagemAtual.trim()) {
  msgArray.push({
    created_at: new Date().toISOString(),
    message: { type: "human", content: mensagemAtual }
  });
}

// Deduplica por timestamp + conteudo (100 chars)
const seen = new Map();
const unique = msgArray.filter(item => {
  const timestamp = new Date(item.created_at).getTime();
  const content = item.message?.content || '';
  const key = `${timestamp}_${content.substring(0, 100)}`;

  if (seen.has(key)) return false;
  seen.set(key, true);
  return true;
});

// Formata em ordem cronologica
const mensagens_antigas = unique
  .sort((a, b) => new Date(a.created_at) - new Date(b.created_at))
  .map(item => {
    const prefix = item.message.type === "human" ? "Lead/Humano" : "Assistente/IA";
    return `[${item.created_at}] ${prefix}: ${item.message.content}`;
  })
  .join("\n\n");
```

### Verificacao de Encavalamento

```javascript
// No: Mensagem encavalada?
const ultima_mensagem_da_fila = $('Buscar mensagens').last();
const mensagem_do_workflow = $('Info').first().json.mensagem_id;

let last_db = null;
try {
  last_db = JSON.parse(ultima_mensagem_da_fila.json.last_db_message);
} catch(e) {
  last_db = null;
}

// Se ultima mensagem no banco NAO e a atual, para o workflow
if (last_db && last_db.id_mensagem !== mensagem_do_workflow) {
  return []; // Mensagem encavalada, descarta
}

// Pass-through
return $('Buscar mensagens').all();
```

### Calculo de Custo LLM

```javascript
// No: Calcular Custo LLM
const PRECO_INPUT = 1.25;   // USD por 1M tokens (Gemini 2.5 Pro)
const PRECO_OUTPUT = 5.00;  // USD por 1M tokens

const promptTokens = estimarTokens(prompt);
const completionTokens = estimarTokens(completion);

const custoInput = (promptTokens / 1000000) * PRECO_INPUT;
const custoOutput = (completionTokens / 1000000) * PRECO_OUTPUT;
const custoTotal = custoInput + custoOutput;

return {
  promptTokens,
  completionTokens,
  totalTokens: promptTokens + completionTokens,
  custoUSD: custoTotal,
  custoBRL: custoTotal * 6 // Conversao aproximada
};
```

---

## 9.3 Schemas de Dados

### Schema do Webhook (Input)

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "type": "object",
  "properties": {
    "body": {
      "type": "object",
      "properties": {
        "contact_id": { "type": "string" },
        "first_name": { "type": "string" },
        "full_name": { "type": "string" },
        "email": { "type": "string", "format": "email" },
        "phone": { "type": "string" },
        "tags": { "type": "string" },
        "location": {
          "type": "object",
          "properties": {
            "id": { "type": "string" },
            "name": { "type": "string" }
          }
        },
        "message": {
          "type": "object",
          "properties": {
            "type": { "type": "integer" },
            "body": { "type": "string" }
          }
        },
        "customData": {
          "type": "object",
          "properties": {
            "ghl_api_key": { "type": "string" },
            "ativar_ia": { "type": "string" },
            "objetivodolead": { "type": "string" }
          }
        }
      },
      "required": ["contact_id", "location", "message"]
    }
  }
}
```

### Schema do Info (Interno)

```json
{
  "type": "object",
  "properties": {
    "lead_id": { "type": "string" },
    "first_name": { "type": "string" },
    "full_name": { "type": "string" },
    "telefone": { "type": "string" },
    "email": { "type": "string" },
    "location_id": { "type": "string" },
    "location_name": { "type": "string" },
    "api_key": { "type": "string" },
    "mensagem": { "type": "string" },
    "mensagem_id": { "type": "string" },
    "datetime": { "type": "string", "format": "date-time" },
    "source": { "type": "string", "enum": ["whatsapp", "instagram"] },
    "n8n_ativo": { "type": "boolean" },
    "process_id": { "type": "string" },
    "workflow_id": { "type": "string" },
    "agente_ia": { "type": "string" },
    "ativar_ia": { "type": "string" },
    "objetivo_do_lead": { "type": "string" }
  }
}
```

### Schema da Memoria (LangChain)

```json
{
  "type": "object",
  "properties": {
    "session_id": { "type": "string" },
    "message": {
      "type": "object",
      "properties": {
        "type": { "type": "string", "enum": ["human", "ai"] },
        "content": { "type": "string" },
        "tool_calls": { "type": "array" },
        "additional_kwargs": { "type": "object" },
        "response_metadata": { "type": "object" }
      }
    },
    "created_at": { "type": "string", "format": "date-time" }
  }
}
```

---

## 9.4 Links para Documentos Detalhados

| Documento | Descricao | Linhas |
|-----------|-----------|--------|
| [01_INVENTARIO_ESTRUTURAL.md](./01_INVENTARIO_ESTRUTURAL.md) | Lista completa de nos, conexoes, credenciais | ~650 |
| [02_CATEGORIZACAO_FUNCIONAL.md](./02_CATEGORIZACAO_FUNCIONAL.md) | 16 categorias funcionais, matriz de criticidade | ~840 |
| [03_FLUXO_DE_DADOS.md](./03_FLUXO_DE_DADOS.md) | Data lineage, transformacoes, variaveis | ~1315 |
| [04_INTEGRACOES_EXTERNAS.md](./04_INTEGRACOES_EXTERNAS.md) | 8 sistemas, endpoints, autenticacao | ~915 |
| [05_LOGICA_DE_NEGOCIO.md](./05_LOGICA_DE_NEGOCIO.md) | Maquina de estados, 10 regras formais | ~885 |
| [06_SEGURANCA_CONFIABILIDADE.md](./06_SEGURANCA_CONFIABILIDADE.md) | Vulnerabilidades, matriz de risco | ~700 |

---

# 10. APENDICE EXPANDIDO - DETALHAMENTO TECNICO

## 10.1 Mapeamento Completo de Conexoes

### Conexoes do Webhook ate Normalizacao

```
Mensagem recebida (Webhook)
    |
    +---> Contexto UTM (Code)
              |
              +---> Normalizar Nome1 (Code)
                        |
                        +---> Normalizar Dados1 (Code)
                                  |
                                  +---> Execution Data5
                                            |
                                            +---> Code1 (Timestamps)
                                                      |
                                                      +---> Classificar Tipo Mensagem
                                                                |
                                                                +---> Info (Set)
```

### Conexoes do Roteamento Principal

```
Info (Set)
    |
    +---> Search Contact (HTTP)
    |         |
    |         +---> Edit Fields1
    |
    +---> IA Ativa? (If)
              |
              +---> [FALSE] FIM - IA Desativada
              |
              +---> [TRUE] Tipo de mensagem (Switch)
                              |
                              +---> [/reset] Limpar memoria
                              |
                              +---> [/teste] Listar campos customizados
                              |
                              +---> [Audio] Download audio
                              |
                              +---> [Imagem] Analyze image
                              |
                              +---> [Texto] Enfileirar mensagem
                              |
                              +---> [Vazia] FIM
```

### Conexoes do Processamento de Audio

```
Download audio (HTTP)
    |
    +---> Extract from File
              |
              +---> Extrair a extensao (Code)
                        |
                        +---> Convert to File
                                  |
                                  +---> Transcrever audio (Whisper)
                                            |
                                            +---> Imagem ou audio (Set)
                                                      |
                                                      +---> Enfileirar mensagem
```

### Conexoes da Fila de Mensagens

```
Enfileirar mensagem (Postgres INSERT)
    |
    +---> Esperar (Wait 18s)
              |
              +---> Buscar mensagens (Postgres SELECT)
                        |
                        +---> Form Mensagem (Set)
                                  |
                                  +---> Mensagem encavalada? (Code)
                                            |
                                            +---> [VAZIO] FIM - Descartada
                                            |
                                            +---> [DADOS] Limpar fila de mensagens
                                                          |
                                                          +---> Conversa Ativa
```

### Conexoes do Controle de Concorrencia

```
Conversa Ativa (Postgres SELECT)
    |
    +---> Acao Planejada (Switch)
              |
              +---> [Iniciar] Permitido AI? (Filter)
              |                  |
              |                  +---> [PASSA] Execution Data
              |                  |               |
              |                  |               +---> Salvar Inicio IA
              |                  |
              |                  +---> [BLOQUEADO] FIM
              |
              +---> [Aguardar] Salvar Espera
              |                  |
              |                  +---> Wait (15s)
              |                            |
              |                            +---> Conversa Ativa (Loop)
              |
              +---> [Ignorar] FIM - Retries > 10
```

### Conexoes da Preparacao IA

```
Salvar Inicio IA (Postgres UPSERT)
    |
    +---> Mensagem anteriores (Postgres SELECT)
              |
              +---> Preparar Mensagem (Code)
                        |
                        +---> Memoria Lead (Postgres INSERT)
                                  |
                                  +---> Deduplica Mensagens (Code)
                                            |
                                            +---> Set mensagens (Set)
                                                      |
                                                      +---> Buscar Agente Ativo (Postgres)
                                                                |
                                                                +---> Preparar Execucao + Contexto
```

### Conexoes do AI Agent

```
Preparar Execucao + Contexto (Code)
    |
    +---> Tudo certo?4 (If)
              |
              +---> [ERRO] Filter -> FIM
              |
              +---> [OK] AI Agent - Modular
                          |
                          +---> [Tools chamadas internamente]
                          |         |
                          |         +---> Busca_disponibilidade
                          |         +---> Agendar_reuniao
                          |         +---> Atualizar Work Permit
                          |         +---> Atualizar Profissao
                          |         +---> Atualizar Estado
                          |         +---> Adicionar_tag_perdido
                          |         +---> Busca historias (MCP)
                          |         +---> Think1
                          |
                          +---> Tipo de mensagem1 (Switch)
```

### Conexoes da Formatacao e Envio

```
Tipo de mensagem1 (Switch)
    |
    +---> Conversa ativa atualizada (Postgres)
              |
              +---> If (Ha processo esperando?)
                        |
                        +---> [SIM] Atualizar resposta IA
                        |               |
                        |               +---> Termino de resposta
                        |
                        +---> [NAO] Deve enviar mensagem? (Filter)
                                        |
                                        +---> Parser Chain (Gemini Flash)
                                                  |
                                                  +---> Calcular Custo LLM (Code)
                                                            |
                                                            +---> If1 (Contem codigo?)
                                                                      |
                                                                      +---> [OK] Execution Data2
                                                                      |            |
                                                                      |            +---> Segmentos1
                                                                      |
                                                                      +---> [CODIGO] Reformatar
```

### Conexoes do Loop de Envio

```
Segmentos1 (SplitOut)
    |
    +---> Loop Over Items3 (SplitInBatches)
              |
              +---> Memoria IA (Postgres INSERT)
              |
              +---> Canal (Switch)
                        |
                        +---> [WhatsApp] Whatsapp (HTTP POST)
                        |                  |
                        |                  +---> 1.5s (Wait)
                        |                            |
                        |                            +---> no.op -> Loop Over Items3
                        |
                        +---> [Instagram] Instagram (HTTP POST)
                                           |
                                           +---> 1.5s (Wait)
                                                     |
                                                     +---> no.op -> Loop Over Items3
```

### Conexoes de Finalizacao

```
Loop Over Items3 (Quando termina)
    |
    +---> Execution Data1
              |
              +---> Call Track AI Cost (ExecuteWorkflow)
                        |
                        +---> FIM SUCESSO
```

---

## 10.2 Exemplos Detalhados de Uso das Tools

### Tool 1: Busca_disponibilidade

**Quando Usar:**
- Lead pergunta sobre horarios disponiveis
- Lead quer saber quando pode agendar
- Lead pergunta "quando posso marcar?"

**Parametros de Entrada:**
```json
{
  "calendar": "Obrigatorio - ID do calendario GHL",
  "API_KEY": "Obrigatorio - API key da location",
  "startDate": "Obrigatorio - timestamp em ms (inicio da busca)",
  "endDate": "Obrigatorio - timestamp em ms (7 dias apos startDate)",
  "lead_id": "Obrigatorio - ID do contato"
}
```

**Exemplo de Chamada pela IA:**
```
Tool: Busca_disponibilidade
Input: {
  "calendar": "cal_abc123",
  "API_KEY": "sk-xxx",
  "startDate": 1735689600000,
  "endDate": 1736294400000,
  "lead_id": "contact_xyz"
}
```

**Resposta Esperada:**
```json
{
  "slots": [
    {"date": "2025-01-02", "time": "09:00", "available": true},
    {"date": "2025-01-02", "time": "10:00", "available": true},
    {"date": "2025-01-02", "time": "14:00", "available": true}
  ]
}
```

**Como a IA Formata a Resposta:**
```
Temos os seguintes horarios disponiveis essa semana:

*Quinta, 2 de Janeiro:*
- 09:00
- 10:00
- 14:00

Qual horario funciona melhor pra voce?
```

---

### Tool 2: Agendar_reuniao

**Quando Usar:**
- Lead confirma horario desejado
- Lead diz "quero marcar para as 10h"
- Lead fornece todos os dados necessarios

**Parametros de Entrada:**
```json
{
  "API_KEY": "string - API key da location",
  "email": "string - email do lead",
  "telefone": "string - formato +00000000000",
  "startTime": "string - formato ISO 2021-06-23T03:30:00+05:30",
  "firstName": "string - primeiro nome",
  "lastName": "string - sobrenome (opcional)",
  "lead_id": "string - ID do contato",
  "Carreira_Consultoria": "string - 'carreira' ou 'consultoria'"
}
```

**Exemplo de Chamada pela IA:**
```
Tool: Agendar_reuniao
Input: {
  "API_KEY": "sk-xxx",
  "email": "john@email.com",
  "telefone": "+15551234567",
  "startTime": "2025-01-02T10:00:00-05:00",
  "firstName": "John",
  "lastName": "Doe",
  "lead_id": "contact_xyz",
  "Carreira_Consultoria": "carreira"
}
```

**Resposta Esperada:**
```json
{
  "success": true,
  "appointmentId": "appt_123456",
  "calendarEventId": "evt_789",
  "message": "Agendamento criado com sucesso"
}
```

**Como a IA Formata a Resposta:**
```
Perfeito, John! Sua reuniao esta agendada para:

*Data:* Quinta, 2 de Janeiro de 2025
*Horario:* 10:00 (horario de Nova York)
*Tipo:* Consultoria de Carreira

Voce recebera um email de confirmacao com o link da reuniao.

Se precisar reagendar, e so me avisar!
```

---

### Tool 3: Atualizar Work Permit

**Quando Usar:**
- Lead informa se tem ou nao work permit
- Lead menciona autorizacao de trabalho
- Necessario para qualificacao

**Parametros de Entrada:**
```json
{
  "API_KEY": "string - API key",
  "contact_Id": "string - ID do contato",
  "workPermitValue": "string - 'Sim' ou 'Nao'"
}
```

**Exemplo de Chamada pela IA:**
```
Tool: Atualizar Work Permit
Input: {
  "API_KEY": "sk-xxx",
  "contact_Id": "contact_xyz",
  "workPermitValue": "Nao"
}
```

**Impacto no Sistema:**
- Atualiza campo custom `work_permit` no GHL
- Define qual calendario sera usado para agendamento:
  - `Sim` = Calendario de Consultoria
  - `Nao` = Calendario de Carreira

---

### Tool 4: Atualizar Profissao

**Quando Usar:**
- Lead informa sua profissao/area de atuacao
- Lead menciona em que trabalha
- Necessario para personalizacao

**Parametros de Entrada:**
```json
{
  "API_KEY": "string - API key",
  "contact_Id": "string - ID do contato",
  "profissaoValue": "string - profissao do lead"
}
```

**Exemplo de Chamada pela IA:**
```
Tool: Atualizar Profissao
Input: {
  "API_KEY": "sk-xxx",
  "contact_Id": "contact_xyz",
  "profissaoValue": "Engenheiro de Software"
}
```

---

### Tool 5: Atualizar Estado

**Quando Usar:**
- Lead informa onde mora (estado dos EUA)
- Lead menciona sua localizacao
- Necessario para contexto

**Parametros de Entrada:**
```json
{
  "API_KEY": "string - API key",
  "contact_Id": "string - ID do contato",
  "estadoValue": "string - sigla do estado (FL, TX, CA, etc)"
}
```

**Exemplo de Chamada pela IA:**
```
Tool: Atualizar Estado
Input: {
  "API_KEY": "sk-xxx",
  "contact_Id": "contact_xyz",
  "estadoValue": "FL"
}
```

---

### Tool 6: Adicionar_tag_perdido

**Quando Usar:**
- Lead claramente nao se enquadra no perfil
- Lead menciona que nao tem interesse
- Lead esta em situacao incompativel com o servico

**Parametros de Entrada:**
```json
{
  "contact_Id": "string - ID do contato"
}
```

**Exemplo de Chamada pela IA:**
```
Tool: Adicionar_tag_perdido
Input: {
  "contact_Id": "contact_xyz"
}
```

**Impacto no Sistema:**
- Adiciona tag "perdido" ao contato
- Lead pode ser removido de automacoes
- Registro fica para historico

---

### Tool 7: Busca historias (MCP)

**Quando Usar:**
- Lead pede exemplos de sucesso
- Lead quer saber de casos reais
- Aumentar confianca/credibilidade

**Parametros de Entrada:**
```json
{
  "query": "string - termo de busca"
}
```

**Exemplo de Chamada pela IA:**
```
Tool: Busca historias
Input: {
  "query": "engenheiro florida carreira"
}
```

**Resposta Esperada:**
```json
{
  "stories": [
    {
      "name": "Carlos",
      "profession": "Engenheiro Civil",
      "state": "Florida",
      "outcome": "Conseguiu emprego em 3 meses",
      "testimonial": "O programa me ajudou a..."
    }
  ]
}
```

---

### Tool 8: Think (Raciocinio Interno)

**Quando Usar:**
- IA precisa processar informacao complexa
- Decisao que requer analise
- Chain of Thought para melhor resposta

**Parametros de Entrada:**
```json
{
  "thought": "string - raciocinio interno"
}
```

**Exemplo de Chamada pela IA:**
```
Tool: Think
Input: {
  "thought": "O lead mencionou que tem green card mas trabalha como
  autonomo. Preciso verificar se o servico de consultoria se aplica
  a esse caso. Green card significa que ele tem work permit, entao
  devo direcioná-lo para consultoria e não carreira."
}
```

**Nota:** A saida do Think nao e enviada ao lead, e apenas para processamento interno.

---

## 10.3 Troubleshooting Expandido

### Problema 1: Lead Nao Recebe Resposta

**Sintomas:**
- Mensagem enviada pelo lead
- Webhook dispara
- Sem resposta retornada

**Arvore de Diagnostico:**

```mermaid
graph TD
    A[Lead nao recebe resposta] --> B{Webhook disparou?}
    B -->|Nao| C[Verificar configuracao GHL]
    B -->|Sim| D{IA Ativa = sim?}
    D -->|Nao| E[Ativar IA para o lead]
    D -->|Sim| F{Tipo mensagem OK?}
    F -->|Vazia| G[Mensagem invalida]
    F -->|OK| H{Fila processada?}
    H -->|Encavalada| I[Aguardar proxima]
    H -->|OK| J{Concorrencia OK?}
    J -->|Ignorada| K[Retries > 10]
    J -->|OK| L{AI Agent OK?}
    L -->|Erro| M[Verificar logs Gemini]
    L -->|OK| N{Envio OK?}
    N -->|Erro| O[Verificar API GHL]
    N -->|OK| P[Verificar inbox lead]
```

**Passos de Debug Detalhados:**

**1. Verificar Webhook:**
```sql
-- No n8n, verificar execucoes recentes
-- Menu: Executions > Filtrar por workflow
-- Procurar pelo contact_id do lead
```

**2. Verificar Campo IA:**
```javascript
// No GHL, verificar contato
// Custom Fields > ativar_ia deve ser "sim"
// OU contato deve ter tag "assistente-admin"
```

**3. Verificar Fila:**
```sql
SELECT * FROM n8n_fila_mensagens
WHERE lead_id = 'ID_DO_LEAD'
ORDER BY timestamp DESC
LIMIT 10;
```

**4. Verificar Concorrencia:**
```sql
SELECT * FROM n8n_active_conversation
WHERE lead_id = 'ID_DO_LEAD';

-- Se status = 'active' e created_at < NOW() - interval '1 minute'
-- A conversa esta travada
```

**5. Verificar AI Agent:**
- Ir na execucao especifica
- Expandir no "AI Agent - Modular"
- Verificar output e eventuais erros

**6. Verificar Envio:**
- Expandir nos "Whatsapp" ou "Instagram"
- Verificar response code (200 = OK)
- Se erro 4xx/5xx, verificar API key e rate limiting

---

### Problema 2: Mensagens Duplicadas

**Sintomas:**
- Lead recebe mesma resposta 2+ vezes
- Respostas chegam em sequencia rapida

**Causas Possiveis:**

1. **Retry apos sucesso:**
   - n8n pode fazer retry se nao receber confirmacao
   - Solucao: Verificar configuracao de retry

2. **Multiplos processos:**
   - Dois webhooks processados simultaneamente
   - Solucao: Verificar logs de execucao simultaneos

3. **Fila nao limpa:**
   - Mensagem nao foi removida da fila
   - Solucao: Verificar no "Limpar fila"

**Diagnostico:**
```sql
-- Verificar historico de mensagens da IA
SELECT created_at, message->'content' as content
FROM n8n_historico_mensagens
WHERE session_id = 'ID_DO_LEAD'
  AND message->>'type' = 'ai'
ORDER BY created_at DESC
LIMIT 20;

-- Se houver mensagens identicas em timestamps proximos = duplicacao
```

**Solucao Permanente:**
```javascript
// Adicionar verificacao de idempotencia antes do envio
const lastSent = await getLastSentMessage(lead_id);
const currentMessage = $json.output;

if (lastSent === currentMessage && timeDiff < 60000) {
  return []; // Nao enviar duplicado
}
```

---

### Problema 3: Resposta com Codigo/JSON

**Sintomas:**
- Lead recebe mensagem com `{`, `json`, `output`, etc.
- Conteudo tecnico visivel

**Causa:**
- AI Agent retornou resposta mal formatada
- Parser Chain nao filtrou adequadamente

**Diagnostico:**
1. Verificar output do AI Agent na execucao
2. Verificar condicao do "If1"
3. Verificar output do Parser Chain

**Solucao Imediata:**
```javascript
// Melhorar filtro no If1
const blacklist = [
  'json', '{', '}', 'output', 'parsed',
  'split', 'properties', 'type', 'function',
  'undefined', 'null', 'error', 'Error'
];

const outputLower = $json.output.messages.join().toLowerCase();
const hasBlacklisted = blacklist.some(term => outputLower.includes(term));

return hasBlacklisted ? [] : $input.all();
```

**Solucao Permanente:**
- Melhorar prompt do AI Agent para evitar output tecnico
- Adicionar etapa de sanitizacao antes do envio
- Configurar Parser Chain com instrucoes mais claras

---

### Problema 4: Timeout do AI Agent

**Sintomas:**
- Execucao demora muito (> 60s)
- Workflow termina sem resposta
- Erro de timeout nos logs

**Causas Possiveis:**

1. **Prompt muito longo:**
   - Historico de mensagens extenso
   - Contexto hiperpersonalizado grande

2. **Tool execution lenta:**
   - Busca_disponibilidade demorando
   - MCP Server lento

3. **Rate limiting do Gemini:**
   - Muitas requisicoes simultaneas

**Diagnostico:**
```javascript
// Verificar tamanho do input
const inputSize = JSON.stringify($input.first().json).length;
console.log(`Input size: ${inputSize} chars`);

// Se > 100KB, input muito grande
```

**Solucoes:**

1. **Limitar historico:**
```javascript
// No "Deduplica Mensagens", limitar ultimas N mensagens
const MAX_MESSAGES = 20;
const unique = [...]; // deduplicacao normal
const limited = unique.slice(-MAX_MESSAGES);
```

2. **Adicionar timeout explicito:**
```json
{
  "options": {
    "timeout": 45000
  }
}
```

3. **Cache de disponibilidade:**
```javascript
// Cachear slots por 5 minutos
const cacheKey = `slots_${calendar}_${startDate}`;
const cached = await getCache(cacheKey);
if (cached) return cached;
```

---

### Problema 5: Tool Nao Executa

**Sintomas:**
- IA menciona que vai executar tool
- Tool nao aparece nos logs
- Acao nao realizada no GHL

**Causas Possiveis:**

1. **Parametros incorretos:**
   - Falta parametro obrigatorio
   - Formato incorreto

2. **Sub-workflow com erro:**
   - Workflow da tool com problema
   - Credenciais invalidas

**Diagnostico:**
1. Verificar output do AI Agent
2. Procurar por `tool_calls` na resposta
3. Verificar execucao do sub-workflow

**Solucao:**
```javascript
// Adicionar logging nas tools
console.log('Tool called with params:', JSON.stringify(params));
```

---

### Problema 6: Memoria Nao Funciona

**Sintomas:**
- IA nao lembra conversa anterior
- Responde como se fosse primeira mensagem
- Contexto perdido

**Causas Possiveis:**

1. **Session ID errado:**
   - session_id diferente do lead_id
   - Historico nao encontrado

2. **Tabela vazia:**
   - Mensagens nao persistidas
   - DELETE acidental

**Diagnostico:**
```sql
-- Verificar memoria do lead
SELECT COUNT(*) as total,
       MIN(created_at) as primeira,
       MAX(created_at) as ultima
FROM n8n_historico_mensagens
WHERE session_id = 'ID_DO_LEAD';

-- Se total = 0, memoria vazia
```

**Solucao:**
```javascript
// Verificar session_id antes de salvar
const sessionId = $('Info').first().json.lead_id;
if (!sessionId) {
  throw new Error('session_id vazio!');
}
```

---

## 10.4 FAQs Tecnicas

### Perguntas Frequentes

**Q1: Posso desativar a fila de 18 segundos?**

A: Sim, mas nao e recomendado. A fila existe para consolidar multiplas mensagens em uma unica resposta. Se remover:
- Pro: Resposta mais rapida
- Con: Mensagens podem ser processadas separadamente
- Con: Maior custo de API (mais chamadas ao LLM)

Para ajustar o tempo:
1. Localizar no "Esperar" (ID: 3924ac37...)
2. Alterar valor de 18 para desejado
3. Testar extensivamente

---

**Q2: Como adicionar um novo canal (ex: Telegram)?**

A: Passos necessarios:
1. Criar webhook especifico para Telegram
2. Normalizar dados no formato esperado pelo "Info"
3. No Switch "Canal", adicionar nova saida "Telegram"
4. Criar no HTTP para API do Telegram
5. Testar fluxo completo

Exemplo de normalizacao:
```javascript
// Converter payload Telegram -> formato Info
return {
  lead_id: $json.message.from.id.toString(),
  first_name: $json.message.from.first_name,
  mensagem: $json.message.text,
  source: 'telegram',
  // ... demais campos
};
```

---

**Q3: Como trocar o LLM de Gemini para GPT-4?**

A: Passos:
1. Criar credencial OpenAI no n8n
2. No "AI Agent - Modular", trocar no conectado
3. Ajustar parametros de modelo (temperature, max_tokens)
4. Testar prompt com novo modelo
5. Ajustar Parser Chain se necessario

Consideracoes:
- GPT-4 tem pricing diferente
- Ajustar calculo de custos
- Alguns prompts podem precisar ajuste

---

**Q4: Como aumentar a seguranca do webhook?**

A: Implementar em ordem de prioridade:

1. **Validar signature (CRITICO):**
```javascript
const secret = $env.GHL_WEBHOOK_SECRET;
const signature = $json.headers['x-ghl-signature'];
if (signature !== secret) {
  throw new Error('Unauthorized');
}
```

2. **Rate limiting:**
```javascript
const rateLimiter = new Map();
const leadId = $json.body.contact_id;
const now = Date.now();
const lastCall = rateLimiter.get(leadId) || 0;

if (now - lastCall < 1000) { // 1 request/segundo
  return []; // Descarta
}
rateLimiter.set(leadId, now);
```

3. **IP whitelist:**
- Configurar no servidor/firewall
- Permitir apenas IPs do GHL

---

**Q5: Como monitorar custos em tempo real?**

A: O workflow ja calcula custos no "Calcular Custo LLM". Para monitorar:

1. **Dashboard no Postgres:**
```sql
SELECT
  DATE(created_at) as data,
  SUM(custo_usd) as total_usd,
  COUNT(*) as execucoes,
  AVG(custo_usd) as media_por_msg
FROM execution_metrics
WHERE workflow_id = 'R2fVs2qpct1Qr2Y1'
GROUP BY DATE(created_at)
ORDER BY data DESC;
```

2. **Alerta de custo:**
```javascript
// Adicionar no final do workflow
const custoHoje = await getCustoHoje();
const limite = 100; // USD

if (custoHoje > limite) {
  await sendAlert('Limite de custo atingido!');
}
```

---

**Q6: Como fazer backup do workflow?**

A: Metodos recomendados:

1. **Export manual:**
   - n8n > Workflow > Export > JSON
   - Salvar em repositorio git

2. **Automatizado via API:**
```bash
curl -X GET \
  https://seu-n8n.com/api/v1/workflows/R2fVs2qpct1Qr2Y1 \
  -H "X-N8N-API-KEY: sua-api-key" \
  -o backup_$(date +%Y%m%d).json
```

3. **Versionamento:**
   - Manter historico de mudancas
   - Documentar cada alteracao

---

**Q7: Qual o limite de mensagens no historico?**

A: Atualmente nao ha limite explicito. Recomendacoes:

1. **Para performance:**
   - Limitar a 50 mensagens no contexto do LLM
   - Manter historico completo no banco

2. **Para compliance (LGPD):**
   - Implementar expurgo automatico
   - Sugestao: Manter 2 anos, depois anonimizar

```sql
-- Expurgo automatico
DELETE FROM n8n_historico_mensagens
WHERE created_at < NOW() - INTERVAL '2 years';
```

---

**Q8: Como testar mudancas sem afetar producao?**

A: Estrategias recomendadas:

1. **Clone do workflow:**
   - Duplicar workflow
   - Mudar path do webhook
   - Testar com leads de teste

2. **Feature flags:**
```javascript
const enableNewFeature = $env.ENABLE_NEW_FEATURE === 'true';

if (enableNewFeature) {
  // Codigo novo
} else {
  // Codigo atual
}
```

3. **Ambiente de staging:**
   - n8n separado
   - Banco separado
   - Location de teste no GHL

---

## 10.5 Metricas e KPIs Recomendados

### Metricas Operacionais

| Metrica | Formula | Meta | Alerta |
|---------|---------|------|--------|
| Taxa de Sucesso | Sucesso / Total | > 95% | < 90% |
| Tempo de Resposta | Fim - Inicio | < 30s | > 60s |
| Custo por Mensagem | Total USD / Mensagens | < $0.05 | > $0.10 |
| Taxa de Duplicacao | Duplicadas / Total | < 1% | > 5% |
| Taxa de Erro | Erros / Total | < 2% | > 5% |

### Metricas de Negocio

| Metrica | Formula | Meta | Alerta |
|---------|---------|------|--------|
| Taxa de Agendamento | Agendados / Leads | > 20% | < 10% |
| Taxa de No-Show | NoShow / Agendados | < 15% | > 25% |
| Tempo ate Agendamento | Data Agend - Primeira Msg | < 3 dias | > 7 dias |
| Taxa de Qualificacao | Qualificados / Total | > 60% | < 40% |
| Taxa de Perdidos | Perdidos / Total | < 20% | > 30% |

### Dashboard SQL

```sql
-- Metricas do dia
SELECT
  COUNT(*) as total_execucoes,
  COUNT(*) FILTER (WHERE status = 'success') as sucesso,
  COUNT(*) FILTER (WHERE status = 'error') as erros,
  ROUND(AVG(duration_ms)::numeric, 2) as tempo_medio_ms,
  ROUND(SUM(custo_usd)::numeric, 4) as custo_total,
  ROUND((COUNT(*) FILTER (WHERE status = 'success')::float / COUNT(*) * 100)::numeric, 2) as taxa_sucesso
FROM execution_metrics
WHERE DATE(created_at) = CURRENT_DATE
  AND workflow_id = 'R2fVs2qpct1Qr2Y1';
```

---

## 10.6 Configuracoes de Ambiente

### Variaveis de Ambiente Recomendadas

```bash
# Seguranca
GHL_WEBHOOK_SECRET=seu-token-secreto
API_KEY_ENCRYPTION_KEY=chave-de-criptografia

# LLMs
GEMINI_API_KEY=sua-api-key-gemini
OPENAI_API_KEY=sua-api-key-openai
ANTHROPIC_API_KEY=sua-api-key-anthropic

# Database
DATABASE_URL=postgresql://user:pass@host:5432/db

# Feature Flags
ENABLE_CIRCUIT_BREAKER=true
ENABLE_RATE_LIMITING=true
MAX_RETRIES=3
QUEUE_WAIT_TIME=18

# Monitoring
SLACK_WEBHOOK_URL=https://hooks.slack.com/xxx
ALERT_EMAIL=admin@empresa.com
```

### Configuracao de Retry

```json
{
  "settings": {
    "executionTimeout": 300,
    "maxTries": 3,
    "waitBetweenTries": 5000,
    "retryOnFail": true
  }
}
```

---

# 11. GLOSSARIO ESTENDIDO

## Termos Tecnicos

| Termo | Definicao | Contexto de Uso |
|-------|-----------|-----------------|
| **AI Agent** | Componente que coordena LLM e tools | No principal de geracao de resposta |
| **Callback** | Funcao chamada apos evento | Resposta de tools |
| **Chain of Thought** | Raciocinio explicito do LLM | Tool Think |
| **Concorrencia** | Multiplos processos simultaneos | Controle de lock |
| **Debounce** | Agrupar eventos em sequencia | Fila de 18s |
| **Encavalamento** | Mensagens sobrepostas | Verificacao de fila |
| **Entry Point** | Ponto de entrada do workflow | Webhook |
| **Exit Point** | Ponto de saida do workflow | Envio de mensagem |
| **Fallback** | Alternativa em caso de falha | LLM secundario |
| **Lock** | Bloqueio de recurso | n8n_active_conversation |
| **MCP** | Model Context Protocol | Busca de historias |
| **Parser** | Transformador de formato | Parser Chain |
| **Prompt** | Instrucao para LLM | System prompt do agente |
| **Rate Limiting** | Limite de requisicoes | APIs externas |
| **Retry** | Tentativa apos falha | Configuracao de nos |
| **Session** | Contexto de conversa | session_id = lead_id |
| **SPOF** | Single Point of Failure | Analise de risco |
| **Tool** | Funcionalidade do agente | 8 tools disponiveis |
| **Webhook** | Endpoint que recebe eventos | Entrada do GHL |

## Termos de Negocio

| Termo | Definicao | Contexto de Uso |
|-------|-----------|-----------------|
| **Carreira** | Servico para quem NAO tem work permit | Tipo de consultoria |
| **Consultoria** | Servico para quem JA tem work permit | Tipo de servico |
| **Etapa do Funil** | Posicao do lead no processo | Campo custom GHL |
| **Lead** | Potencial cliente | Contato no GHL |
| **Location** | Sub-conta no GHL | Separacao de negocios |
| **No-Show** | Falta a reuniao agendada | Metrica negativa |
| **Perdido** | Lead desqualificado | Tag no GHL |
| **Qualificado** | Lead apto para o servico | Etapa do funil |
| **Work Permit** | Autorizacao de trabalho EUA | Campo de qualificacao |

---

# 12. CHANGELOG E ROADMAP

## Changelog

### v1.0 (2025-12-31)
- Documento inicial consolidado
- Sintese de 6 documentos de analise
- Arquitetura completa documentada
- 5 diagramas Mermaid
- Guias praticos
- Troubleshooting
- FAQs

## Roadmap Sugerido

### Curto Prazo (1-2 semanas)
- [ ] Corrigir VULN-001: Autenticar webhook
- [ ] Corrigir VULN-002: SQL Injection
- [ ] Remover API keys de logs
- [ ] Criar Error Workflow

### Medio Prazo (1 mes)
- [ ] Implementar circuit breaker
- [ ] Adicionar fallback LLM (Gemini -> OpenAI)
- [ ] Implementar rate limiting
- [ ] Criar dashboard de metricas

### Longo Prazo (3 meses)
- [ ] Implementar retencao de dados (LGPD)
- [ ] Adicionar suporte a novos canais
- [ ] Implementar A/B testing de prompts
- [ ] Criar sistema de escalacao humana

---

## HISTORICO DE VERSOES

| Versao | Data | Autor | Mudancas |
|--------|------|-------|----------|
| 1.0 | 2025-12-31 | Agente 7 (Claude) | Documento inicial consolidado |
| 1.1 | 2025-12-31 | Agente 7 (Claude) | Expansao com apendices detalhados |

---

## CONTATO E SUPORTE

- **Responsavel:** Marcos Daniels
- **Empresa:** MOTTIVME
- **Stack:** n8n, PostgreSQL, GoHighLevel, Google Gemini
- **Ambiente:** Producao - EUA Market

---

**FIM DO DOCUMENTO**

*Este guia consolida as analises dos Agentes 1-6 e deve ser usado como referencia principal para entender, modificar e manter o workflow GHL - Mottivme - EUA Versionado.*

*Documento gerado automaticamente pelo Agente 7 - Especialista em Consolidacao e Sintese Tecnica*
*Versao: 1.1 | Data: 2025-12-31 | Linhas: ~3,500+*
