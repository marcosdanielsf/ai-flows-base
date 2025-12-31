# 🤖 AI Flows Base - MOTTIVME

Base de criação de projetos de fluxos de IA para automação comercial e CRM.

## 📋 Visão Geral

Este repositório contém a base de conhecimento e workflows para projetos de automação com IA, incluindo:
- Fluxos de SDR (Sales Development Representative)
- Automações de secretária virtual
- Pré-vendas e qualificação de leads
- Integração com GoHighLevel, n8n e Supabase

## 🏗️ Estrutura do Projeto

```
ai-flows-base/
├── workflows/              # Workflows n8n e automações
│   ├── secretaria-v3/     # Material Secretária Virtual v3
│   ├── pre-vendas-fss/    # Pré-vendas FSS (Follow-up Sales System)
│   └── secretaria-ghl/    # Secretária adaptada para GoHighLevel
├── docs/                  # Documentação e playbooks
├── scripts/               # Scripts de automação e deploy
├── database/              # Schemas e configurações de banco
└── templates/             # Templates reutilizáveis
```

## 📦 Workflows Disponíveis

### 1. Secretária Virtual v3
- **Localização:** `workflows/secretaria-v3/`
- **Descrição:** Sistema completo de secretária virtual com agendamento, lembretes e gestão de leads
- **Componentes:**
  - Arquivos da Secretária v3
  - Configuração Base de Dados Postgres
  - Configuração Coolify
  - Workflows n8n

### 2. Pré-Vendas FSS
- **Localização:** `workflows/pre-vendas-fss/`
- **Descrição:** Sistema de pré-vendas com scripts, playbooks e materiais de treinamento
- **Componentes:**
  - Play Books completos
  - Scripts de ligação e WhatsApp
  - Matriz de follow-up
  - Speechs e abordagens

### 3. Secretária GoHighLevel
- **Localização:** `workflows/secretaria-ghl/`
- **Descrição:** Adaptação da secretária para integração completa com GoHighLevel
- **Componentes:**
  - Agentes de IA
  - Integrações Supabase/Asaas
  - Gestão de agendamentos
  - Recuperação de leads

## 🚀 Como Usar

### Pré-requisitos
- n8n instalado
- GoHighLevel configurado
- Supabase ou PostgreSQL
- Node.js 18+

### Instalação

1. Clone o repositório:
```bash
git clone https://github.com/[seu-usuario]/ai-flows-base.git
cd ai-flows-base
```

2. Configure as variáveis de ambiente:
```bash
cp .env.example .env
# Edite o .env com suas credenciais
```

3. Importe os workflows no n8n:
```bash
# Importe os arquivos .json da pasta workflows/ no seu n8n
```

## 🔧 Tecnologias

- **n8n** - Orquestração de workflows
- **GoHighLevel** - CRM e automação de marketing
- **Supabase** - Banco de dados e backend
- **PostgreSQL** - Banco de dados relacional
- **Asaas** - Gateway de pagamento
- **Claude AI** - Agentes de IA conversacional

## 📚 Documentação

Cada workflow possui documentação específica:
- [Secretária v3](./workflows/secretaria-v3/README.md)
- [Pré-Vendas FSS](./workflows/pre-vendas-fss/README.md)
- [Secretária GHL](./workflows/secretaria-ghl/README.md)

## 🤝 Contribuindo

Este é um projeto base interno da MOTTIVME. Para contribuir:
1. Crie uma branch feature
2. Faça suas alterações
3. Teste em ambiente de desenvolvimento
4. Abra um Pull Request

## 📝 Licença

Propriedade da MOTTIVME - Uso interno apenas.

## 👥 Autor

**Marcos Daniels** - MOTTIVME
- Stack: Next.js, Supabase, n8n, GoHighLevel, PostgreSQL

## 📞 Suporte

Para dúvidas ou suporte, entre em contato através dos canais internos da MOTTIVME.

---

*Última atualização: 31/12/2025*
