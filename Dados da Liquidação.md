Formato da notificação de atualização de liquidação enviado pelo sistema.

```json
{
	"event": "settlement",
	"payload": {
	"installments": [
		{
			"id": string,
			"transactionId": string,
			"acquirerReferenceNumber": string,
			"installmentNumber": number,
			"installmentDate": string,
			"amount": number,
			"isMain": boolean,
			"issuerSettlementStatus": "settled" | "scheduled" | "canceled",
			"merchantSettlementStatus": "settled" | "scheduled" | "canceled",
		}
	],
}
```