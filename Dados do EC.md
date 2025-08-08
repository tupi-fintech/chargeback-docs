Formato da notificação de dados do EC enviado pelo sistema.

```json
{
	"event": "merchant",
	"payload": {
		"id": string,
		"taxId": string,
		"legalName": string,
		"dbaName": string,
		"acquirerMerchantIdentifier": string,
		"economicActivityCode": string,
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