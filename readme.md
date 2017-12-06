# py-tescomobile

A Python library and command line tool for checking your remaining unit
allowances (eg minutes, texts, data).

The user is authorised by sending an SMS to the given phone number containing a
pin, the pin is then exchanged for an auth token used to authenticate all
further requests. Auth tokens do not appear to expire and you can have multiple
active tokens. A method is provided for invalidating tokens.

## API

The ```TescoMobile``` class provides the following methods:

- ```__init__(phone_number, token, user_agent)``` Only requires the phone
number.
- ```send_pin_sms``` Send an auth pin to the user via SMS.
- ```token_pin_exchange(pin)``` Exchange the pin for an auth token for future
requests.
- ```get_usage``` Get used and total values for allowances, billing data etc.
- ```get_invoices``` Get a list of invoices for the account.
- ```logout``` Invalidate the auth token.

Example:

```
>>> from tescomobile import TescoMobile
>>> t = TescoMobile('07XXXXXXXXX')
>>> x = t.send_pin_sms()
>>> x = t.token_pin_exchange(YYYYY)
>>> x['token']
'ZZZZZZZZ-ZZZZ-ZZZZ-ZZZZ-ZZZZZZZZZZZZ'
>>> x = t.get_usage()
>>> x['allowances'][0]
{'allowanceType': 'MIN', 'totalAllowance': 750.0, 'startDate': 1489449600000, 'unlimited': 'NO', 'totalUsed': 32.0, 'totalRemaining': 718.0}
```

## CLi

The CLi app calls the ```get_usage``` method and formats the output.

The phone number and auth token are read from the ```TESCO_PHONENUMBER``` and
```TESCO_TOKEN``` env variables. The script wil call ```send_pin_sms``` and
```token_pin_exchange``` if these are not set.