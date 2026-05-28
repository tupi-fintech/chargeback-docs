# 📋 Documentação do Sistema de Chargeback

[![Status](https://img.shields.io/badge/status-active-brightgreen)]()
[![Version](https://img.shields.io/badge/version-1.0.0-blue)]()
[![Documentation](https://img.shields.io/badge/docs-complete-success)]()

> 📚 Documentação completa das APIs e notificações do sistema de gerenciamento de chargebacks

## 🌐 Versões de Idioma

- 🇺🇸 [English](./README.md)
- 🇪🇸 [Español](./README.es.md)
- 🇧🇷 **Português** (atual)

## 🎯 Visão Geral

Este repositório contém a documentação técnica completa do sistema de chargeback, incluindo especificações de APIs, formatos de notificações e fluxos de dados para integração com sistemas externos.

![File Ingestion](./images/file-ingestion.png)

## 📁 Estrutura da Documentação

### 🔄 1. Enrichment (Enriquecimento de Dados)

![Data Enrichment](./images/data-enrichment.png)

> **Nota:** A área destacada representa componentes que devem ser desenvolvidos pelo cliente. Isso inclui a implementação da lógica de consulta aos dados internos e o envio das respostas para o nosso sistema durante o fluxo de enriquecimento.

Documentação do fluxo de enriquecimento de dados entre sistema e cliente:

| Arquivo | Descrição | Fluxo de Dados |
|---------|-----------|----------------|
| [`1.TRANSACTION.md`](./1.Enrichment/1.TRANSACTION.pt-br.md) | 💳 **Dados de Transação**<br/>📤 `TransactionEvent`: Enviado para o cliente<br/>📥 `TransactionResponse`: Recebido via API | **Event** → Cliente<br/>**Response** ← Cliente |
| [`2.MERCHANT.md`](./1.Enrichment/2.MERCHANT.pt-br.md) | 🏪 **Dados do Estabelecimento Comercial**<br/>📤 `MerchantEvent`: Enviado para o cliente<br/>📥 `MerchantResponse`: Recebido via API | **Event** → Cliente<br/>**Response** ← Cliente |

### 📢 2. Notifications (Notificações de Sistema)

![System Notifications](./images/system-notifications.png)

Documentação das notificações de status e ciclo de vida dos chargebacks:

| Arquivo | Descrição | Tipo de Evento |
|---------|-----------|----------------|
| [`3.STATUS.md`](./2.Notifications/3.STATUS.pt-br.md) | 📊 **Notificações de Status** - Atualizações de status do processo de chargeback | `status` |
| [`4.CYCLE.md`](./2.Notifications/4.CYCLE.pt-br.md) | 🔄 **Notificações de Ciclo** - Mudanças de ciclo (chargeback, pré-arbitragem, arbitragem) | `cycle` |

### 🤖 3. AI (Inteligência Artificial)

![AI Recommendation](./images/ai-recommendation.png)

Documentação da integração com agente de IA para recomendações de chargeback:

| Arquivo | Descrição | Tipo de Dados |
|---------|-----------|---------------|
| [`5.AI.md`](./3.AI/5.AI.pt-br.md) | 🧠 **Dados do Agente de IA** - Dados de entrada enviados para o agente de IA de recomendação de chargeback | API de Entrada |

## 📋 Tipos de Comunicação Disponíveis

### 🔄 Enriquecimento de Dados (Events ↔ Responses)

| Tipo | Enviamos (Event) | Recebemos (Response) | Documentação |
|------|------------------|---------------------|--------------|
| `transaction` | Solicita dados de transação | Dados completos da transação | [TRANSACTION.md](./1.Enrichment/1.TRANSACTION.pt-br.md) |
| `merchant` | Solicita dados do EC | Dados completos do estabelecimento | [MERCHANT.md](./1.Enrichment/2.MERCHANT.pt-br.md) |

### 📢 Notificações Unidirecionais (Events)

| Tipo | Enviamos (Event) | Propósito | Documentação |
|------|------------------|-----------|--------------|
| `status` | Atualização de status | Informar mudanças de status | [STATUS.md](./2.Notifications/3.STATUS.pt-br.md) |
| `cycle` | Mudança de ciclo | Informar alterações de ciclo | [CYCLE.md](./2.Notifications/4.CYCLE.pt-br.md) |

## 🔧 Integração

### 📤📥 Fluxo de Comunicação

O sistema utiliza dois tipos de comunicação:

#### 📤 **Events (Eventos)** - Enviados pelo Sistema
Notificações que **enviamos para o cliente** quando precisamos de dados adicionais:
- Contêm identificadores mínimos necessários
- Solicitam enriquecimento de dados específicos
- São enviados via webhook/notificação

#### 📥 **Responses (Respostas)** - Recebidas via API  
Dados completos que **recebemos do cliente** via API para atualizar nosso sistema:
- Contêm todos os dados detalhados solicitados
- São enviados pelo cliente através de chamadas API
- Atualizam as informações no nosso sistema

### 🌐 Endpoint Único de Webhook

Utilizamos um único endpoint de webhook para todos os eventos da plataforma (`transaction`, `merchant`, `status`, `cycle`, `settlement`, `voucher`).

O tipo do evento é identificado pelo campo `event` no payload.

### 🔒 Segurança da Comunicação (Global)

Como camada de segurança para eventos e respostas, adotamos:

- IP whitelisting (apenas IPs autorizados podem enviar/receber chamadas);
- Assinatura HMAC SHA-256 no header `x-webhook-signature`.

Headers esperados (exemplo):

```http
content-type: application/json
user-agent: Chargeback-API-Webhook/1.0
x-webhook-signature: sha256=<assinatura_hmac_hex>
```
### Estrutura Base dos Eventos

Todos os eventos enviados seguem uma estrutura base comum:

```typescript
type BaseEvent = {
    event: string;
    payload: {
        contractDisputeId: string;
        // ... identificadores específicos por tipo de evento
    };
}
```

### Estrutura Base das Respostas

As respostas recebidas via API contêm dados completos:

```typescript
type BaseResponse = {
    // Dados completos e detalhados do objeto solicitado
    // Estrutura varia conforme o tipo de dados
}
```

### Identificadores Principais

- **`contractDisputeId`**: Identificador único do contrato de disputa
- **`transactionIdentifier`**: Identificador da transação
- **`acquirerReferenceNumber`**: Número de referência da adquirente
- **`helpdeskCaseIdentifier`**: Identificador do caso no helpdesk

## ⚙️ Requisitos Mínimos

Para executar o sistema de chargeback, certifique-se de que seu ambiente atenda aos seguintes requisitos mínimos:

### 🖥️ Requisitos de Hardware
- **CPU**: 1 vCPU mínimo
- **RAM**: 512MB mínimo
- **Armazenamento**: Armazenamento de objetos compatível com S3 (tamanho depende do volume do cliente)

### 🗄️ Requisitos de Banco de Dados
- **PostgreSQL**: Banco de dados PostgreSQL compatível (versão 17+ recomendada)

### ☸️ Requisitos do Kubernetes
- **Pods**: 2 pods mínimo para alta disponibilidade
- **Versão do Kubernetes**: 1.20+ recomendado
- **Rede**: Controlador de ingress configurado para acesso externo

### 📊 Requisitos de Relatórios e BI
- **Metabase**: Plataforma de Business Intelligence para criação de gráficos e dashboards
  - Necessário para visualização de dados e relatórios
  - Consulte os [requisitos do Metabase](https://www.metabase.com/docs/latest/installation-and-operation/installing-metabase)

### 🔒 Requisitos de Segurança
- **TLS**: Certificados SSL/TLS para endpoints HTTPS
- **Autenticação**: Keycloak como solução de gerenciamento de identidade e acesso
  - Consulte os [requisitos do Keycloak](https://www.keycloak.org/high-availability/concepts-memory-and-cpu-sizing)

## 📞 Suporte

Para dúvidas ou sugestões sobre esta documentação, entre em contato com a equipe de desenvolvimento.

---

<div align="center">

**📄 Documentação mantida pela equipe Tupi Fintech**

*Última atualização: Abril 2026*

</div>
