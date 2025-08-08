# 📋 Documentación del Sistema de Contracargo

[![Status](https://img.shields.io/badge/status-active-brightgreen)]()
[![Version](https://img.shields.io/badge/version-1.0.0-blue)]()
[![Documentation](https://img.shields.io/badge/docs-complete-success)]()

> 📚 Documentación completa de las APIs y notificaciones del sistema de gestión de contracargos

## 🌐 Versiones de Idioma

- 🇺🇸 [English](./README.md)
- 🇪🇸 **Español** (actual)
- 🇧🇷 [Português](./README.pt-br.md)

## 🎯 Descripción General

Este repositorio contiene la documentación técnica completa del sistema de contracargo, incluyendo especificaciones de APIs, formatos de notificaciones y flujos de datos para integración con sistemas externos.

## 📁 Estructura de la Documentación

### 🔄 1. Enrichment (Enriquecimiento de Datos)

Documentación del flujo de enriquecimiento de datos entre sistema y cliente:

| Archivo | Descripción | Flujo de Datos |
|---------|-------------|----------------|
| [`1.TRANSACTION.md`](./1.Enrichment/1.TRANSACTION.md) | 💳 **Datos de Transacción**<br/>📤 `TransactionEvent`: Enviado al cliente<br/>📥 `TransactionResponse`: Recibido vía API | **Event** → Cliente<br/>**Response** ← Cliente |
| [`2.MERCHANT.md`](./1.Enrichment/2.MERCHANT.md) | 🏪 **Datos del Comercio**<br/>📤 `MerchantEvent`: Enviado al cliente<br/>📥 `MerchantResponse`: Recibido vía API | **Event** → Cliente<br/>**Response** ← Cliente |

### 📢 2. Notifications (Notificaciones del Sistema)

Documentación de las notificaciones de estado y ciclo de vida de los contracargos:

| Archivo | Descripción | Tipo de Evento |
|---------|-------------|----------------|
| [`3.STATUS.md`](./2.Notifications/3.STATUS.md) | 📊 **Notificaciones de Estado** - Actualizaciones de estado del proceso de contracargo | `status` |
| [`4.CYCLE.md`](./2.Notifications/4.CYCLE.md) | 🔄 **Notificaciones de Ciclo** - Cambios de ciclo (contracargo, pre-arbitraje, arbitraje) | `cycle` |

## 🚀 Cómo Usar Esta Documentación

1. **Para Desarrolladores**: 
   - Consulte los archivos de **Events** para implementar recepción de notificaciones
   - Consulte los archivos de **Responses** para implementar APIs de retorno de datos
   - Use los formatos especificados para garantizar una integración correcta

2. **Para Analistas**: 
   - Use la documentación para comprender los flujos bidireccionales de datos
   - Entienda cuándo el sistema solicita datos (Events) vs cuándo recibe datos (Responses)

3. **Para Soporte**: 
   - Utilice como referencia para resolución de problemas de integraciones
   - Identifique si los problemas están en el envío de Events o recepción de Responses

## 📋 Tipos de Comunicación Disponibles

### 🔄 Enriquecimiento de Datos (Events ↔ Responses)

| Tipo | Enviamos (Event) | Recibimos (Response) | Documentación |
|------|------------------|---------------------|---------------|
| `transaction` | Solicita datos de transacción | Datos completos de transacción | [TRANSACTION.md](./1.Enrichment/1.TRANSACTION.md) |
| `merchant` | Solicita datos del comercio | Datos completos del comercio | [MERCHANT.md](./1.Enrichment/2.MERCHANT.md) |

### 📢 Notificaciones Unidireccionales (Events)

| Tipo | Enviamos (Event) | Propósito | Documentación |
|------|------------------|-----------|---------------|
| `status` | Actualización de estado | Informar cambios de estado | [STATUS.md](./2.Notifications/3.STATUS.md) |
| `cycle` | Cambio de ciclo | Informar alteraciones de ciclo | [CYCLE.md](./2.Notifications/4.CYCLE.md) |

## 🔧 Integración

### 📤📥 Flujo de Comunicación

El sistema utiliza dos tipos de comunicación:

#### 📤 **Events (Eventos)** - Enviados por el Sistema
Notificaciones que **enviamos al cliente** cuando necesitamos datos adicionales:
- Contienen identificadores mínimos necesarios
- Solicitan enriquecimiento de datos específicos
- Se envían vía webhook/notificación

#### 📥 **Responses (Respuestas)** - Recibidas vía API  
Datos completos que **recibimos del cliente** vía API para actualizar nuestro sistema:
- Contienen todos los datos detallados solicitados
- Son enviados por el cliente a través de llamadas API
- Actualizan la información en nuestro sistema

### Estructura Base de los Eventos

Todos los eventos enviados siguen una estructura base común:

```typescript
type BaseEvent = {
    event: string;
    payload: {
        contractDisputeId: string;
        // ... identificadores específicos por tipo de evento
    };
}
```

### Estructura Base de las Respuestas

Las respuestas recibidas vía API contienen datos completos:

```typescript
type BaseResponse = {
    // Datos completos y detallados del objeto solicitado
    // La estructura varía según el tipo de datos
}
```

### Identificadores Principales

- **`contractDisputeId`**: Identificador único del contrato de disputa
- **`transactionIdentifier`**: Identificador de transacción
- **`acquirerReferenceNumber`**: Número de referencia del adquirente
- **`helpdeskCaseIdentifier`**: Identificador del caso de helpdesk

## 📞 Soporte

Para preguntas o sugerencias sobre esta documentación, contacte al equipo de desarrollo.

---

<div align="center">

**📄 Documentación mantenida por el equipo Tupi Fintech**

*Última actualización: Agosto 2025*

</div>
