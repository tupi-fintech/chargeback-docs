Formato da notificação de atualização de transação enviado pelo sistema.

```json
{
	"event": "transaction",
	"payload": {
		"id": string,
		"transactionIdentifier": string,
		"acquirerReferenceNumber": string,
		"terminalIdentifier": string,
		"authorizationDate": string,
		"authorizedAmount": number,
	}
}
```