Formato da notificação de atualização de ciclo enviado pelo sistema.

```json
{
	"event": "cycle",
	"payload": {
		"id": string,
		"installmentDisputeId": string,
		"name": "chargeback" | "pre_arbitrage" | "arbitrage",
		"status": "open" | "closed",
		"decision": "accepted_in_full" | "accepted_partially" | "rejected",
		"recomendation": string, // AI decision recomendation
		"requestForInformation": boolean, // email enviado ao varejista?
		"informationReceived": boolean, // recebeu resposta do varejista?
		"startDate": string,
		"endDate": string,
	}
}
```