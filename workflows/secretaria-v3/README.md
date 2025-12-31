# Secretária Virtual v3

Sistema completo de secretária virtual automatizada com IA.

## 📋 Componentes

### Arquivos da Secretária v3
Workflows e configurações principais do sistema de secretária.

### Configuração Base de Dados
Schema PostgreSQL para o sistema de secretária:
- Tabelas de agendamentos
- Gestão de contatos
- Histórico de interações
- Sistema de lembretes

### Configuração Coolify
Setup de deploy e infraestrutura usando Coolify.

### Workflows n8n
Workflows de automação para:
- Agendamento automático
- Lembretes de consultas
- Gestão de leads
- Integração com CRM

## 🚀 Como Implementar

1. **Banco de Dados:**
   ```bash
   psql -U postgres -d your_database -f "Configuração Base de Dados Postgres.sql"
   ```

2. **Workflows n8n:**
   - Importe os arquivos .json da pasta WORKFLOWS/
   - Configure as credenciais necessárias
   - Ative os workflows

3. **Deploy (Coolify):**
   - Siga as instruções em CONFIGURAÇÃO COOLIFY/

## 📝 Documentos Importantes

Ver pasta `Arquivos da Secretária v3` para documentação completa.
