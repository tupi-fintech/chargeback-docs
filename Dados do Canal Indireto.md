Formato da notificação de dados do Parceiro enviado pelo sistema.

```json
{
	"event": "partner",
	"payload": {
		"id": string,
		"taxId": string,
		"legalName": string,
		"dbaName": string,
		"partnerAcquirerIdentifier": string,
		"address": {
			"country": string,
			"postalCode": string,
			"street": string,
			"number": string,
			"complement": string,
			"neighborhood": string,
			"state": string,
			"city": string,
		},
		"contact": {
			"name": string,
			"email": string,
			"phone": string,
		}
	}
}
```