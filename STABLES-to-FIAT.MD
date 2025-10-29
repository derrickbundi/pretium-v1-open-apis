![My Image](USDT-FIAT.png)

# API DOCUMENTATION
This documentation provides a guide on how to integrate with Pretium's open APIs.

## To Get Started
Reach out our tech team `hello@pretium.africa` for provision of `base_uri`, `x-api-key` and `settlement wallet address`

### Authorization Required
All endpoints require API key authorization. Ensure that you include the `x-api-key` header in every request.

### Exchange Rates
This endpoint retrieves exchange rates and can be used to calculate fiat equivalent for a given stablecoin amount. <br><br>
`POST {{uri}}/v1/exchange-rate`

#### Body Params
```javascript
{
    "currency_code": "KES"
}
```

### Validation
This endpoint is used to validate a merchant code, mobile number, or bank account number. In response, you get the corresponding business name, registered individual name (as provided by MNOs), or bank account details.<br><br>
`POST {{uri}}/v1/validation`

| Params | Countries | Required |
| --- | --- | --- |
| type | KES | Yes |
| shortcode | KES, UGX, CDF, MWK, ETB & GHS | Yes |
| mobile_network | KES, UGX, CDF, MWK, ETB & GHS | Yes |
| account_number | NGN | Yes |
| bank_code | NGN | Yes |

##### Sample
```javascript
{
    "type": "MOBILE",
    "shortcode": "0799770833",
    "mobile_network": "Safaricom"
}
```

- Mobile networks: `Safaricom`, `Airtel`, `MTN`, `AirtelTigo`, `Airtel Money`, `Orange Money`, `MPESA`, `Telcel`, `Telebirr` & `Cbe Birr`
- Type: `PAYBILL`, `BUY_GOODS` & `MOBILE`

<br>

**Note: When validating markets other than Kenya, include the currency code in the `uri`, for example: `{{uri}}/v1/validation/UGX`**

### Pay (Stables-to-FIAT)
This endpoint calls the Pretium payment protocol to verify that the payment has been made to the settlement wallet and release the equivalent fiat.    

`POST {{uri}}/v1/pay`
| Params | Countries | Required |
| --- | --- | --- |
| transaction_hash | All | Yes |
| amount | All | Yes |
| fee | All | No |
| shortcode | KES, UGX, MWK, ETB & GHS | Yes |
| account_number | KES & NGN | Yes |
| type | KES | Yes |
| mobile_network | KES, UGX, MWK, ETB & GHS | Yes |
| account_name | NGN | Yes |
| bank_name | NGN | Yes |
| bank_code | NGN | Yes |
| chain | All | No |
| callback_url | All | No |

##### Sample
```javascript
{
    "transaction_hash":"0x55a572efe1720250e442f38741477a4fc3f7f152e5cd208cc52f8222a1c2a13b",
    "shortcode": "0799770833",
    "amount": "500",
    "fee": "10",
    "type": "MOBILE",
    "chain": "CELO",
    "callback_url": "https://pretium.africa/callback"
}
```

##### Countries Supported
Kenya 🇰🇪, Nigeria🇳🇬, Ghana 🇬🇭, Uganda 🇺🇬, DR Congo 🇨🇩, Malawi 🇲🇼 & Ethiopia 🇪🇹

### Onramp (FIAT-to-Stables)
This endpoint calls the Pretium payment protocol to initiate cash collection from the user, confirm receipt via mobile money or bank transfer, and release the equivalent amount in stablecoins. 

`POST {{url}}/v1/onramp/{currency_code}`
| Params | Countries | Required |
| --- | --- | --- |
| shortcode | KES, CDF, MWK & UGX | Yes |
| amount | All | Yes |
| fee | All | No |
| type | All | No |
| mobile_network | KES, CDF, MWK & UGX | Yes |
| callback_url | All | No |
| chain | All | Yes |
| asset | All | Yes |

##### Sample
```javascript
{
    "shortcode": "0799770833",
    "amount": 100,
    "fee": 10,
    "mobile_network": "Safaricom",
    "chain": "CELO",
    "asset": "USDT",
    "address": "0xE9c7cf1b0f7F68d1236284ebE6b5bc8CA43b11b8",
    "callback_url": "https://pretium.africa"
}
```

> `fee` params is used collect fees from partners' users and credited to your wallet. For example, include a 1,000 `fee` in the API request. Our payment protocol collects the `fee` + `amount` from the customer, credits your wallet with the `fee`, and releases the equivalent value of `amount` to your customer.

 

##### Countries Supported
Kenya 🇰🇪, Uganda 🇺🇬, Malawi 🇲🇼 & DR Congo 🇨🇩

##### Stablecoins supported

| Chain | Asset |
| --- | --- |
| CELO | cUSD, USDC, USDT |
| BASE | USDC |
| TRON | USDT |
| STELLAR | USDC |

##### Limits

| Country | Lower limit | Higher limit|
| --- | --- | --- |
| KES | 20 | 250,000 |
| NGN | 100 | 2,000,000 |
| MWK | 100 | 5,000,000 |
| UGX | 500 | 5,000,000 |
| GHS | 5 | 1,000 |

**Note 1: When calling API `PAY` on other markets apart from Kenya, include the currency code in the `uri`, for example: `{{uri}}/v1/pay/UGX`** <br> <br>
**Note 2: Account number in `KES` is required only when `type` is `PAYBILL`** <br> <br>
**Note 3: The API `ONRAMP` callback URL receives two responses — payment receipt confirmation and stablecoin release confirmation.**

### Banks
This endpoint is used to get list of banks supported in `NGN` market.    

`POST {{uri}}/v1/banks`

### Status
`POST {{uri}}/v1/status`

#### Body Params
```javascript
{
    "transaction_code": "09eb23f8-6a11-4fec-b6bc-168ccf4e254d"
}
```

**Note: When calling this API on other markets than Kenya, include the currency code in the `uri`, for example: `{{uri}}/v1/status/UGX`**

### Supported Blockchain Networks
- Celo
- Base
- Tron
- Stellar

