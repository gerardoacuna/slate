---
title: Moneypool API Documentaion

language_tabs:
  - shell

toc_footers:
  - <a href='http://github.com/tripit/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---

# Introduction

Moneypool API

# Authentication

> To authorize a user before each action, use the Authorization header in the following format:

```shell
curl "https://moneypool.mx/api/"
  -H "Authorization: Token token=fe70b88622cdc3972513e4d1f10c0b7e" \
```

Every call to the Moneypool api requires an authorization token that identifies a logged in user. 

In the **Sessions** section of this documentation is the information on how to login a user and receive it's authorization token.

# Versions

```shell
curl -H "Accept: application/vnd.moneypool.mx+json; version=1" \
      https://moneypool.mx/api/
```

The current api version is version number 1. This needs to be specified in the Accept header.

# Sessions

## Email/Password Login
```shell
curl -H "Accept: application/vnd.moneypool.mx+json; version=1" \
     -H "Content-type: application/json" \
     -X POST -d '{
      "user": {
        "email":"user@example.com",
        "password":"foobar123"
        }
      }' https://moneypool.mx/api/sessions
```
> After login a JSON structured like this is returned:

```json
{"user":
  {"id":1, 
  "name":"John Doe", 
  "email":"user@example.com", 
  "avatar_url":"https://example.com/user_avatar.jpg", 
  "auth_token":"05d07190a444cb8d2bd42af26efcf22b", 
  "bank":"Bank of Mexico",
  "clabe":"123456789123456789",
  "wallet":"1000.0"}
}
```
This endpoint allows a registered user to login with his moneypool credentials.

### HTTP Request

`POST https://moneypool.mx/api/sessions`

### Parameters

Parameter | Type | Description
--------- | ---- | -----------
email | string | User's email
password | string | User's password

<aside class="notice">
The `auth_token` returned in the response json is the token that need to be sent in the Authorization header of every call.
</aside>

## Facebook Login

```shell
curl -H "Accept: application/vnd.moneypool.mx+json; version=1" \
     -H "Content-type: application/json" \
     -X POST -d '{
      "user": {
        "oauth_token":"FACEBOOK_OAUTH_TOKEN"
        }
      }' https://moneypool.mx/api/fb_sessions
```
> After login a JSON structured like this is returned:

```json
{"user":
  {"id":1, 
  "name":"John Doe", 
  "email":"user@example.com", 
  "avatar_url": "https://example.com/user_avatar.jpg", 
  "auth_token":"05d07190a444cb8d2bd42af26efcf22b", 
  "bank":"Bank of Mexico",
  "clabe":"123456789123456789",
  "wallet":"1000.0"}
}
```
This endpoint allows a user to login with facebook authorization.

### HTTP Request

`POST https://moneypool.mx/api/fb_sessions`

### Parameters

Parameter | Type | Description
--------- | ---- | -----------
oauth_token | string | Authorization Token returned by Facebook SDK after useing its login button.

<aside class="notice">
An existing user will login with this method. A new user can use this method to create an account with a single click.
</aside>

## Logout
```shell
curl -H "Accept: application/vnd.moneypool.mx+json; version=1" \
     -H "Content-type: application/json" \
     -H "Authorization: Token token='fe70b88622cdc3972513e4d1f10c0b7e'" \
     -X DELETE
     https://moneypool.mx/api/sessions
```

This endpoint allows an authorized user to logout.

### HTTP Request

`DELETE https://moneypool.mx/api/sessions`

# Users

## Create a User

```shell
curl -H "Accept: application/vnd.moneypool.mx+json; version=1" \
     -H "Content-type: application/json" \
     -X POST -d '{
      "user": {
        "name":"John Doe",
        "email":"john@example.com",
        "password":"foobar123",
        "password_confirmation":"foobar123",
        }
      }' https://moneypool.mx/api/users
```
> After creating a user a JSON structured like this is returned:

```json
{"user":
  {"id":1, 
  "name":"John Doe", 
  "email":"user@example.com", 
  "avatar_url": "", 
  "auth_token":"05d07190a444cb8d2bd42af26efcf22b", 
  "bank":"",
  "clabe":"",
  "wallet":"0.0"}
}
```
This endpoint allows a user to create a moneypool account using his email and personal information.

### HTTP Request

`POST https://moneypool.mx/api/fb_sessions`

### Parameters

Parameter | Type |  Description
--------- | ---- | ------------
name | string | 
email | string | Must be a valid email address format.
password | string | Must be at least 6 digits long.
password_confirmation | string | It must match the password parameter.

<aside class="notice">
  All parameters are required.
</aside>

## Get a User

```shell
curl -H "Accept: application/vnd.moneypool.mx+json; version=1" \
     -H "Content-type: application/json" \
     -H "Authorization: Token token='fe70b88622cdc3972513e4d1f10c0b7e'" \
     -X GET
     https://moneypool.mx/api/users/1
```
> Response:

```json
{"user":
  {"id":1, 
  "name":"John Doe", 
  "email":"user@example.com", 
  "avatar_url": "https://example.com/user_avatar.jpg", 
  "auth_token":"05d07190a444cb8d2bd42af26efcf22b", 
  "bank":"Bank of Mexico",
  "clabe":"123456789123456789",
  "wallet":"1000.0"}
}
```
This endpoint retrieves a specific user's information.

### HTTP Request

`GET https://moneypool.mx/api/users/:user_id`

## Update a User

```shell
curl -H "Accept: application/vnd.moneypool.mx+json; version=1" \
     -H "Content-type: application/json" \
     -H "Authorization: Token token='fe70b88622cdc3972513e4d1f10c0b7e'" \
     -X PATCH -d '{
      "user": {
        "name":"John Doe",
        "email":"john@example.com",
        "password":"foobar123",
        "password_confirmation":"foobar123",
        "avatar":<image file>,
        "clabe":"123456789123456789",
        "bank":"Bank of Mexico"
        }
      }' https://moneypool.mx/api/users/1
```
> After updating a user a JSON structured like this is returned:

```json
{"user":
  {"id":1, 
  "name":"John Doe", 
  "email":"user@example.com", 
  "avatar_url": "https://example.com/user_avatar.jpg", 
  "auth_token":"05d07190a444cb8d2bd42af26efcf22b", 
  "bank":"Bank of Mexico",
  "clabe":"123456789123456789",
  "wallet":"0.0"}
}
```
This endpoint updates a user's information.

### HTTP Request

`PATCH https://moneypool.mx/api/users/:user_id`

### Parameters

These are the parameters that can be updated with this endpoint. 

Parameter | Type | Description
--------- | ---- | ------------
name | string | 
email | string | Must be a valid email address format.
password | string | Must be at least 6 digits long.
password_confirmation | It must match the password parameter.
avatar | |
clabe | string | User's bank account **clabe** number, 18 digit long.
bank | string | | The name of the user's bank.

<aside class="notice">
 Parameters not sent on the request won't be updated. 
</aside>


## Reset Password

#Payment Requests

## Payment Request Object

> Example Object

```json
{
  "id": 1,
  "created_at": "2014-11-13T23:54:15.105-06:00",
  "amount": "108.5",
  "description": "Pizza and Sodas",
  "party": "Andy Peterson",
  "party_email": "andy@mail.com",
  "type": "to_user",
  "rejected": false
}
```

Payment request objects are used to let a friend know that money is owed to the friend that sent the request. As an authorized user, payment requests of type `from_user` mean that the authorized user owes money to another user (**party**). `to_user` payment requests mean that the authorized user sent a payment request to another user and wants to received a payment for it.

When listing Payment Requests, pool invitations are included as well (`pool` type).
### Attributes

Name | Type | Description
---- | ---- | -----------
type | string | `pool` , `from_user`, `to_user`
id | integer | Id of payment request or pool
created_at | date |Date of object creation
party | string | Depending on the type of payment request, this may be the **name** of another moneypool user that the payment request was sent to (`to_user` type) or was received from (`from_user` type). It can also contain the **title** of the pool the user has been invited to and has not payed yet (`pool` type).
party_email | string | The email of the user to which the payment was sent to or received from. `pool` type payment requests will not have a party_email
amount | string | Depending on the `type`, amount of MXN pesos as a decimal string that the authorized user owes to the party user or that the party user owes to the authorized user. When type is `pool`, this represents the minimum amount the authorized user has to pay to the pool he's been invited to.
rejected | boolean | This shows if a payment request has been rejected by the user it was sent to. Only for `to_user`.
liability_id | integer | The id of a pool invitation. Only for `pool`.

## List Payment Requests

```shell
curl -H "Accept: application/vnd.moneypool.mx+json; version=1" \
     -H "Content-type: application/json" \
     -H "Authorization: Token token=fe70b88622cdc3972513e4d1f10c0b7e" \
     -X GET
     https://moneypool.mx/api/users/1/mixed_payment_requests
```
> A JSON with a list of payment requests is returned:

```json
{
  "payment_requests": [
      {
          "id": 20,
          "created_at": "2015-02-15T18:36:40.437-06:00",
          "type": "pool",
          "party": "John's Birthday",
          "amount": "200.0",
          "liability_id": 81
      },
      {
          "id": 3,
          "created_at": "2014-11-13T23:54:15.105-06:00",
          "amount": "140.0",
          "description": "Pizza and Sodas",
          "party": "Andy Peterson",
          "party_email": "andy@mail.com",
          "type": "to_user",
          "rejected": false
      },
      {
          "id": 1,
          "created_at": "2014-09-11T15:32:57.094-05:00",
          "amount": "50.0",
          "description": "Bus fare",
          "party": "Ashlee Roberts",
          "party_email": "Ashly@mail.com",
          "type": "from_user",
          "rejected": false
      }
  ]
}
```
This enpoint gets all of the user's payment requests: payement requests he **received**, payment requests he **sent** and **pools** that he has been invited to and has not payed yet.

### HTTP Request

`GET https://moneypool.mx/api/users/:user_id/mixed_payment_requests`

## Create a Payment Request

```shell
curl -H "Accept: application/vnd.moneypool.mx+json; version=1" \
     -H "Content-type: application/json" \
     -H "Authorization: Token token=fe70b88622cdc3972513e4d1f10c0b7e" \
     -X POST -d '{
      "amount":150.0,
      "description":"Yesterdays dinner",
      "user_requested_id":2
      }' https://moneypool.mx/api/users/:user_id/payment_requests
```
> After creating a payment request a JSON structured like this is returned:

```json
{
  "payment_request": {
    "id": 8,
    "description": "Yesterday's dinner",
    "amount": "140.0",
    "rejected": false,
    "user_requested": {
        "id": 2,
        "name": "Andy Peterson",
        "email": "andy@mail.com",
        "avatar_url": "https://example.com/user_avatar.jpg"
    }
  }
}
```
This endpoint allows an authorized user to send a payment_request to another existing moneypool user.

### HTTP Request

`POST https://moneypool.mx/api/users/:user_id/payment_requests`

### Parameters

Parameter | Type | Description
--------- | ---- | -----------
user_requested_id | integer | Id of the user from which the payment is being requested.
amount | string | Amount in MXN pesos that the authorized user is requesting. 
description | string | A brief description of why a payment is being requested.


<aside class="notice">
  All parameters are required.
</aside>

<aside class="notice">
  The user the payment request is being sent to (`user_requested_id`) has to be an existing moneypool user.
</aside>

## Reject a Payment Request

```shell
curl -H "Accept: application/vnd.moneypool.mx+json; version=1" \
     -H "Content-type: application/json" \
     -H "Authorization: Token token=fe70b88622cdc3972513e4d1f10c0b7e" \
     -X PATCH
     https://moneypool.mx/api/users/1/payment_requests/1/reject
```

This endpoint allows an authorized user to reject a payment_request that he has received from another user.

### HTTP Request

`PATCH https://moneypool.mx/api/users/:user_id/payment_requests/:payment_request_id/reject`

<aside class="notice">
  Only payment requests the authorized user has received, the ones of the type `from_user`, can be rejected.
</aside>

## Delete a Payment Request

```shell
curl -H "Accept: application/vnd.moneypool.mx+json; version=1" \
     -H "Content-type: application/json" \
     -H "Authorization: Token token=fe70b88622cdc3972513e4d1f10c0b7e" \
     -X DELETE
     https://moneypool.mx/api/users/1/payment_requests/1
```

This endpoint allows an authorized user to reject a payment_request that he has sent to another user.

### HTTP Request

`DELETE https://moneypool.mx/api/users/:user_id/payment_requests/:payment_request_id`

<aside class="notice">
  Only payment requests the authorized user created, the ones of the type `to_user`, can be deleted.
</aside>

## Delete a Pool invitation

```shell
curl -H "Accept: application/vnd.moneypool.mx+json; version=1" \
     -H "Content-type: application/json" \
     -H "Authorization: Token token=fe70b88622cdc3972513e4d1f10c0b7e" \
     -X DELETE
     https://moneypool.mx/api/users/1/liabilities/2
```

This endpoint allows an authorized user to delete his invitation to a pool that he has not paid yet.

### HTTP Request

`DELETE   https://moneypool.mx/api/users/:user_id/liabilities/:liability_id`

<aside class="notice">
  This is for pool invitations that are included in the payment requests list, the ones of the type `pool` and that have the `liability_id` attribute.
</aside>

# Payments

An authorized user can send money to another user using money from his moneypool wallet or using a credit card.

## List Payments

```shell
curl -H "Accept: application/vnd.moneypool.mx+json; version=1" \
     -H "Content-type: application/json" \
     -H "Authorization: Token token=fe70b88622cdc3972513e4d1f10c0b7e" \
     -X GET
     https://moneypool.mx/api/users/1/history
```
> A JSON with a list of the user's history is returned:

```json
{
  "history": [
    {
      "outcome": {
        "amount": "200.00",
        "created_at": "2015-01-07T00:32:47.390-06:00",
        "description": "",
        "type": "wallet_to_pool",
        "party": "John's Birthday"
      }
    },
    {
      "income": {
        "amount": "100.0",
        "created_at": "2014-12-04T14:45:56.842-06:00",
        "description": "Pizza and beers",
        "type": "card_to_wallet",
        "party": "Alice Smith"
      }
    }
  ]
}
```

The history of an authorized user includes payments he has made and received, specified as outcome or income.

### Attributes

Name | Type | Description
---- | ---- | -----------
Amount | string | Amount in MXN pesos as a decimal string.
Description | string | 
Type | string | Determines the nature of the payment.
Party | string | Name of the User or title of Pool the payment was made or received from.

### Type of Payments

#### Incomes

Type | Description
--- | ---
card_to_wallet | A **transaction** made from another user's credit card into the authorized user's moneypool wallet.
user_to_wallet | A **transfer** made from another user's moneypool wallet into the authorized user's moneypool wallet.
pool_to_wallet | A **transfer** made from a pool's total balance into the authorized user's moneypool wallet.

#### Outcomes

Type | Description
--- | ---
card_to_user | A **transaction** made from the authorized user's credit card to another user's moneypool wallet.
card_to_pool | A **transaction** made from the authorized user's credit card to a pool he was invited to.
cash_to_pool | A **transaction** made by paying at an OXXO convenience with the authorized user's cash to a pool he was invited to.
wallet_to_account | A **transfer** made from the authorized user's moneypool wallet to his own bank account through a withdrawal.
wallet_to_user | A **transfer** made from the authorized user's moneypool wallet to another user's moneypool wallet/
wallet_to_pool | A **transfer** made from the authorized user's moneypool wallet to a pool he was invited to.

### HTTP Request

`GET https://moneypool.mx/api/users/:user_id/history`

## Create a Card Payment

```shell
curl -H "Accept: application/vnd.moneypool.mx+json; version=1" \
     -H "Content-type: application/json" \
     -H "Authorization: Token token=fe70b88622cdc3972513e4d1f10c0b7e" \
     -X POST -d '{
      "payment":{
        "description":"Dinner",
        "payment_method":"card",
        "new_email":"alex@mail.com"},
      "transaction":{
        "amount":100.00,
        "card":"tok_test_visa_4242",
        "name":"Cecelia Schuster"}
      }' https://moneypool.mx/api/users/1/mixed_payments
```

This endpoint creates a payment from the authorized user to another moneypool user or to a pool. This is a credit card transaction. This payment will appear as an outcome in the list of payments.

### HTTP Request

`POST https://moneypool.mx/api/users/:user_id/mixed_payments`

### Parameters

#### Payment

These parameters specify the general information of the payment.

Name | Type | Description
---- | ---- | -----------
description | string |
payment_method | string | The payment method, for this payment the value must be `card`
new_email | string | The email of the user that will receive the payment. If the email belongs to a person that does not have a moneypool account, one will be created for him.
pool_id | integer | Id of the pool that the authorized user wants to make a payment to.

#### Transaction
These parameters specify the information needed to charge a credit card.

Name | Type | Description
---- | ---- | -----------
amount | string | A decimal string that specifies the amount in MXN pesos that will be charged to the credit/debit card.
card | string | The card token. Moneypool can only process a credit/debit card if the card has been previously tokenized by Conekta.
name | string | Name of the credit/debit cardholder.

<aside class="notice">
  Payments need to be sent either to a user or to a pool. Only one of the `new_email` and `pool_id` parameters can be present.
</aside>

## Create a Wallet Payment

```shell
curl -H "Accept: application/vnd.moneypool.mx+json; version=1" \
     -H "Content-type: application/json" \
     -H "Authorization: Token token=fe70b88622cdc3972513e4d1f10c0b7e" \
     -X POST -d '{
      "payment":{
        "wallet_amount":"100.00"
        "description":"Dinner",
        "payment_method":"wallet",
        "new_email":"alex@mail.com"}
      }' https://moneypool.mx/api/users/1/mixed_payments
```

This endpoint creates a payment from the authorized user to another moneypool user or a pool. This is a transfer from a moneypool wallet to another. This payment will appear as an outcome in the list of payments.

### HTTP Request

`POST https://moneypool.mx/api/users/:user_id/mixed_payments`

### Parameters

#### Payment

These parameters specify the general information of the payment.

Name | Type | Description
---- | ---- | -----------
wallet_amount | string |  A decimal string that specifies the amount in MXN pesos that will be transfered from the authorized user's wallet to the receiver user. This amount can't be greater than the amount available in the user's wallet.
description | string |
payment_method | string | The payment method, for this payment the value must be `wallet`
new_email | string | The email of the user that will receive the payment. If the email belongs to a person that does not have a moneypool account, one will be created for him.
pool_id | integer | Id of the pool that the authorized user wants to make a payment to.

<aside class="notice">
  Payments need to be sent either to a user or to a pool. Only one of the `new_email` and `pool_id` parameters can be present.
</aside>

## Create a Mixed Payment

```shell
curl -H "Accept: application/vnd.moneypool.mx+json; version=1" \
     -H "Content-type: application/json" \
     -H "Authorization: Token token=fe70b88622cdc3972513e4d1f10c0b7e" \
     -X POST -d '{
      "payment":{
        "wallet_amount":"100.00"
        "description":"Dinner",
        "payment_method":"card",
        "new_email":"alex@mail.com"},
      "transaction":{
        "amount":100.00,
        "card":"tok_test_visa_4242",
        "name":"Cecelia Schuster"}
      }' https://moneypool.mx/api/users/1/mixed_payments
```

This endpoint creates a mixed payment from the authorized user to another moneypool user or a pool. It is used to create a credit/debit card transaction and a wallet transfer at the same time. This payment will appear as two outcome items in the list of payments, one for the credit/debit card transaction and another for the wallet transfer.

### HTTP Request

`POST https://moneypool.mx/api/users/:user_id/mixed_payments`

### Parameters

#### Payment

These parameters specify the general information of the payment.

Name | Type | Description
---- | ---- | -----------
wallet_amount | string |  A decimal string that specifies the amount in MXN pesos that will be transfered from the authorized user's wallet.
description | string |
payment_method | string | The payment method, for this payment the value must be `mixed`
new_email | string | The email of the user that will receive the payment. If the email belongs to a person that does not have a moneypool account, one will be created for him.
pool_id | integer | Id of the pool that the authorized user wants to make a payment to.

#### Transaction
These parameters specify the information needed to charge a credit card.

Name | Type | Description
---- | ---- | -----------
amount | string | A decimal string that specifies the amount in MXN pesos that will be charged to the credit/debit card.
card | string | The card token. Moneypool can only process a credit/debit card if the card has been previously tokenized by Conekta.
name | string | Name of the credit/debit cardholder.

<aside class="notice">
  Payments need to be sent either to a user or to a pool. Only one of the `new_email` and `pool_id` parameters can be present.
</aside>