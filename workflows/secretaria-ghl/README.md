# Secretária GoHighLevel

Adaptação da secretária virtual para integração completa com GoHighLevel.

## 📋 Workflows Disponíveis

### Agendamento
- `GHL - Agendamento com Validação Anti-Duplicata.json`
- `Agendar pelo GHL - ATUALIZAR KOMMO.json`
- `[ GHL ] Busca Disponibilidade.json`
- `04.1 Atualizar_Cancelar agendamento_notificar.json`
- `09. Desmarcar e enviar alerta.json`
- `Atualizar Last Appointment GHL.json`

### Agentes de IA
- `08. Agente Assistente Interno copy.json`
- `11. Agente de Lembretes de Agendamento.json`
- `13. Agente de Recuperação de Leads.json`
- `SDR - Orthodontic.json`
- `Atualizar Agente IA GHL.json`

### Integrações
- `06.1 Integração Supabase.json`
- `06.1 Integração Asaas.json`
- `10. Buscar ou criar contato + conversa.json`

### Atualizações de Dados
- `[GHL - APEXPRO] Atualizar Lead.json`
- `Atualizar Campo Profissão GHL.json`
- `Atualizar Estado GHL.json`
- `Atualizar Work Permit GHL.json`
- `Contador de Tentativas de Objeção.json`

### Comunicação
- `07. Quebrar e enviar mensagens.json`
- `12. Gestão de ligações.json`
- `05 - Escalar para humano - SOCIALFY.json`

### Utilitários
- `02. Baixar e enviar arquivo do Google Drive.json`

## 🚀 Implementação

### 1. Pré-requisitos
- GoHighLevel configurado
- n8n instalado
- Supabase (opcional mas recomendado)
- Asaas (para pagamentos)

### 2. Importar Workflows
Importe os workflows na seguinte ordem:

1. **Primeiro:** Integrações base
   - `06.1 Integração Supabase.json`
   - `06.1 Integração Asaas.json`

2. **Segundo:** Gestão de contatos
   - `10. Buscar ou criar contato + conversa.json`
   - Workflows de atualização (Profissão, Estado, etc)

3. **Terceiro:** Agendamento
   - `[ GHL ] Busca Disponibilidade.json`
   - `GHL - Agendamento com Validação Anti-Duplicata.json`

4. **Por último:** Agentes de IA
   - `08. Agente Assistente Interno copy.json`
   - `11. Agente de Lembretes de Agendamento.json`
   - `13. Agente de Recuperação de Leads.json`

### 3. Configuração

Para cada workflow:
1. Abra no n8n
2. Configure as credenciais GHL
3. Configure webhooks
4. Teste individualmente
5. Ative após validação

## 🔧 Customização

Cada workflow pode ser adaptado para:
- Diferentes calendários
- Múltiplos usuários
- Vários tipos de serviço
- Diferentes funis de venda

## 📊 Monitoramento

Monitore através de:
- Dashboard n8n
- Supabase (logs e histórico)
- GoHighLevel (conversas e agendamentos)

## ⚠️ Importante

- Teste sempre em ambiente de desenvolvimento primeiro
- Mantenha backups dos workflows
- Documente customizações
