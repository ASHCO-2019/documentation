# API :: CurrencyRate

### GET CurrencyRate

Returns currency rates.

Requires *read* access to *Currency* scope.

### PUT CurrencyRate

Updates currency rates. One or multiple currency rates should be specified in a payload.

Requires *edit* access to *Currency* scope.

Payload example:

```json
{
    "EUR": 1.11,
    "UAH": 0.037
}
```
