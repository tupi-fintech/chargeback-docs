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

Documentação do fluxo de enriquecimento de dados entre sistema e cliente:

| Arquivo | Descrição | Fluxo de Dados |
|---------|-----------|----------------|
| [`1.TRANSACTION.md`](./1.Enrichment/1.TRANSACTION.md) | 💳 **Dados de Transação**<br/>📤 `TransactionEvent`: Enviado para o cliente<br/>📥 `TransactionResponse`: Recebido via API | **Event** → Cliente<br/>**Response** ← Cliente |
| [`2.MERCHANT.md`](./1.Enrichment/2.MERCHANT.md) | 🏪 **Dados do Estabelecimento Comercial**<br/>📤 `MerchantEvent`: Enviado para o cliente<br/>📥 `MerchantResponse`: Recebido via API | **Event** → Cliente<br/>**Response** ← Cliente |

### 📢 2. Notifications (Notificações de Sistema)

![System Notifications](./images/system-notifications.png)

Documentação das notificações de status e ciclo de vida dos chargebacks:

| Arquivo | Descrição | Tipo de Evento |
|---------|-----------|----------------|
| [`3.STATUS.md`](./2.Notifications/3.STATUS.md) | 📊 **Notificações de Status** - Atualizações de status do processo de chargeback | `status` |
| [`4.CYCLE.md`](./2.Notifications/4.CYCLE.md) | 🔄 **Notificações de Ciclo** - Mudanças de ciclo (chargeback, pré-arbitragem, arbitragem) | `cycle` |

## 🚀 Como Usar Esta Documentação

1. **Para Desenvolvedores**: 
   - Consulte os arquivos de **Events** para implementar recebimento de notificações
   - Consulte os arquivos de **Responses** para implementar APIs de retorno de dados
   - Use os formatos especificados para garantir integração correta

2. **Para Analistas**: 
   - Use a documentação para compreender os fluxos bidirecionais de dados
   - Entenda quando o sistema solicita dados (Events) vs quando recebe dados (Responses)

3. **Para Suporte**: 
   - Utilize como referência para troubleshooting de integrações
   - Identifique se problemas estão no envio de Events ou recebimento de Responses

## 📋 Tipos de Comunicação Disponíveis

### 🔄 Enriquecimento de Dados (Events ↔ Responses)

| Tipo | Enviamos (Event) | Recebemos (Response) | Documentação |
|------|------------------|---------------------|--------------|
| `transaction` | Solicita dados de transação | Dados completos da transação | [TRANSACTION.md](./1.Enrichment/1.TRANSACTION.md) |
| `merchant` | Solicita dados do EC | Dados completos do estabelecimento | [MERCHANT.md](./1.Enrichment/2.MERCHANT.md) |

### 📢 Notificações Unidirecionais (Events)

| Tipo | Enviamos (Event) | Propósito | Documentação |
|------|------------------|-----------|--------------|
| `status` | Atualização de status | Informar mudanças de status | [STATUS.md](./2.Notifications/3.STATUS.md) |
| `cycle` | Mudança de ciclo | Informar alterações de ciclo | [CYCLE.md](./2.Notifications/4.CYCLE.md) |

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
- **PostgreSQL**: Banco de dados PostgreSQL compatível (versão 12+ recomendada)

### ☸️ Requisitos do Kubernetes
- **Pods**: 2 pods mínimo para alta disponibilidade
- **Versão do Kubernetes**: 1.20+ recomendado
- **Rede**: Controlador de ingress configurado para acesso externo

### 🔒 Requisitos de Segurança
- **TLS**: Certificados SSL/TLS para endpoints HTTPS
- **Autenticação**: Keycloak como solução de gerenciamento de identidade e acesso
  - Consulte os [requisitos do Keycloak](https://www.keycloak.org/high-availability/concepts-memory-and-cpu-sizing)

## 📞 Suporte

Para dúvidas ou sugestões sobre esta documentação, entre em contato com a equipe de desenvolvimento.

---

<div align="center">

**📄 Documentação mantida pela equipe Tupi Fintech**

*Última atualização: Setembro 2025*

</div>
