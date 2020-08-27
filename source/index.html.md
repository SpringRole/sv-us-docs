---
title: SpringVerify API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell: cURL

toc_footers:
  - <a href='https://stage.us.springverify.com'>Head to SpringVerify to know more.</a>


includes:


search: true
---


# Introduction

Welcome to the SpringVerify API!

We currently have two environments, development and production. Both the environments have separate databases. During development base url of development environment must be used.

We have language bindings in cURL. You can view code examples in the dark area to the right.

## Defining internal objects

USER ROLES | RESPONSE
--------- | ------- 
Employee|We define employee as an entity which is being verified on the platform.
User| We define user as an company admin.
Company| We define company as an entity having group of users (Admins).
Super Admin	| We define Super Admin as springVerify-US team.

## Environment Urls

**Development** : https://api-stage.us.springverify.com

**Acceptance** : https://api-acceptance.us.springverify.com

**Production** : https://api.us.springverify.com

<aside class="notice">
Currently we have two types of coupons (open/closed). Please contact info@springverify.com regarding coupon code for your company.
</aside>

## Authentication

Authenticating in SpringVerify is done on the bases of JSON Web Tokens. Once registered, you will receive a token which can be used for subsequent API requests. 

<aside class="notice">
You must replace <code>JWT_TOKEN</code> in the examples below with your personal Token.
</aside>

## Status Mapping

### System Statuses

STATUS | VALUE
------ | -----
0| PENDING
1| VERIFIED
2| FAILED
3| MANUAL REVIEW

### Super Admin Statuses

STATUS | VALUE
------ | -----
0| PENDING
1| VERIFIED
2| FAILED
3| UNABLE TO VERIFY


# Company API Flow

## SignUp

```shell
curl --location --request POST 'https://api.us.springverify.com/auth/signup' \
--header 'Content-Type: application/json' \
--data-raw '{
    "first_name": "John",
    "last_name": "Wick",
    "phone": "999999999",
    "email": "john@wick.com",
    "password": "daaad6e5604e8e17bd9f108d91e26afe6281dac8fda0091040a7a6d7bd9b43b5",
    "confirm_password":"daaad6e5604e8e17bd9f108d91e26afe6281dac8fda0091040a7a6d7bd9b43b5",
    "domain": "wick.com"
}'
```

> Success Response

```json
{
    "success": true,
    "data": {
        "message": "Account created successfully."
    }
}
```

> Error Response

```json
{
    "success": true,
    "data": {
        "message": "Account with this email address already exists."
    }
}
```

This API is used to register a company ADMIN. Once registered an verification email is sent to the registered email.

<aside class="notice">
    Password and Confirm Password fields should be hashed using SHA256 before hand. 
</aside>

### Query Parameters

Parameter | Type | Description
--------- | ------- | -----------
email|string|Email registered on SpringVerify.
password|string|Password registered on SpringVerify.
confirm_password|string|Password confirmation.
first_name|string|First name of the Admin.
last_name|string|Last name of the Admin.
phone|string|Phone number of the user.
domain|string|Domain of the company. Must be same as the email domain.

<aside class="success">
Congrats on getting started with SpringVerify!
</aside>


## Verify Company Admin Email

```shell
curl --location --request GET 'https://api.us.springverify.com/auth/verify?token=TOKEN_SENT_IN_EMAIL'
```

> Success Response

```json
{
    "success": true,
    "data": {
        "message": "Activated."
    }
}
```

> Error Response

```json
{
    "success": true,
    "data": {
        "message": "Account not found."
    }
}
```

This API is used to confirm email a company ADMIN. Once confirmed the company will get created. 


## Resend Verify Company Admin Email

```shell
curl --location --request POST 'https://api.us.springverify.com/auth/resend-email' \
--header 'Content-Type: application/json' \
--data-raw '{
    "email":"john@wick.com"
}'
```
> Success Response

```json
{
    "success": true,
    "data": {
        "message": "Email Sent."
    }
}

```

> Error Response

```json
{
    "message": "user already verified"
}
```

This API is used to resend confirm email a company ADMIN. 


## Login

```shell
curl --location --request POST 'https://api.us.springverify.com/auth/login' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer JWT_TOKEN' \
--data-raw '{
    "email": "john@wick.com",
    "password": "daaad6e5604e8e17bd9f108d91e26afe6281dac8fda0091040a7a6d7bd9b43b5",
    "role":"admin"
}'
```

> Success Response

```json
{
    "token": "JWT_TOKEN",
    "profile": {
        "first_name": "John",
        "last_name": "Wick",
        "email": "john@wick.com",
        "domain": "wick.com",
        "company": {
            "id": 44,
            "name": "SmartBrains",
            "address": "901, Downtown Manhattan",
            "city": "NY",
            "state": "NY",
            "zipcode": "91319",
            "tax_id_number": "1234567890",
            "s3_logo": "link"
        }
    }
}
```

> Error Response:

```json
{
    "errors": [
        {
            "value": "",
            "msg": "Not a valid role",
            "param": "role",
            "location": "body"
        }
    ]
}
```

Aim is to generate a JWT token , which will be used for all further API calls. The Token in the response will be valid for one hour. 

Client needs to send JWT token in the header successfully Call other APIs.

<aside class="notice">
    Password should be hashed using SHA256 before hand. 
</aside>

### URL Parameters

Parameter | Type | Description
--------- | ------- | -----------
email|string|Email registered on SpringVerify.
password|string|Password registered on SpringVerify.
role|string|It is the role of the login entity (use ‘admin’ for company login).

## Forgot Password

```shell
curl --location --request POST 'https://api.us.springverify.com/auth/forgot-password' \
--header 'Content-Type: application/json' \
--data-raw '{
    "email":"johndoe@gmail.com"
}'
```

> Success Response

```json
{
    "success": true,
    "data": "success"
}
```

## Reset Password

```shell
curl --location --request POST 'https://api.us.springverify.com/profile/reset-password' \
--header 'Authorization: Bearer JWT_TOKEN' \
--header 'Content-Type: application/json' \
--data-raw '{
    "password":"17f80754644d33ac685b0842a402229adbb43fc9312f7bdf36ba24237a1f1ffb"
}'
```

> Success Response

```json
{
    "success": true,
    "data": "success"
}
```

## Register Your Company

```shell
curl --location --request POST 'https://api.us.springverify.com/company' \
--header 'Authorization: Bearer JWT_TOKEN' \
--header 'Content-Type: application/json' \
--data-raw '{
    "name": "SmartBrains",
    "address": "901, Downtown Manhattan",
    "city": "NY",
    "state": "NY",
    "zipcode": "91319",
    "tax_id_number":"1234567890"
}'
```

> Success Response:

```json
{
    "success": true,
    "data": {
        "message": "Company updated successfully"
    }
}
```

This API is used to register a company. JWT token will be used for authentication.

### URL Parameters

Parameter | Type | Description
--------- | ------- | -----------
name|string|Name of the registered company.
address|string|Address of the company.
city|string|City of the company.
state|string|State of the registered company.
zipcode|string|Zipcode of the company.
tax_id_number|string|Tax number of the company.

## Upload Logo

```shell
curl --location --request POST 'https://api.us.springverify.com/company/upload-logo' \
--header 'Authorization: Bearer JWT_TOKEN' \
--form 'logo=@/path/to/file'
```

> Success Response

```json
{
    "success": true,
    "data": "https://spring-verify-us.s3.amazonaws.com/company/wick.com/logo"
}
```

Set the company logo using this API

### URL Parameters

Parameter | Type | Description
--------- | ------- | -----------
logo| file | Raw File of the logo. 

## Get Company Details

```shell
curl --location --request GET 'https://api.us.springverify.com/company' \
--header 'Authorization: Bearer JWT_TOKEN'
```

> Success Response

```json
{
    "success": true,
    "data": {
        "id": 44,
        "created_by": "john@wick.com",
        "name": "SmartBrains",
        "address": "901, Downtown Manhattan",
        "city": "NY",
        "state": "NY",
        "zipcode": "91319",
        "tax_id_number": "1234567890",
        "credits": 0,
        "domain": "wick.com",
        "employment_limit": null,
        "education_limit": null,
        "license_limit": null,
        "civilcourt_limit": null,
        "dl_limit": null,
        "s3_logo": "https://spring-verify-us.s3.amazonaws.com/company/wick.com/logo",
        "created_at": "2020-08-24T10:20:20.000Z",
        "updated_at": "2020-08-24T10:31:49.000Z",
        "employee_invite_groups": []
    }
}
```

Get company details using the JWT_TOKEN.


## Get Company Profile

```shell
curl --location --request GET 'https://api.us.springverify.com/profile' \
--header 'Authorization: Bearer JWT_TOKEN'
```

> Success Response

```json
{
    "success": true,
    "data": {
        "first_name": "John",
        "last_name": "Wick",
        "phone": "999999999",
        "email": "john@wick.com",
        "domain": "wick.com",
        "stripe_id": null,
        "company": {
            "name": "SmartBrains",
            "address": "901, Downtown Manhattan",
            "city": "NY",
            "state": "NY",
            "zipcode": "91319",
            "tax_id_number": "1234567890",
            "s3_logo": "https://spring-verify-us.s3.amazonaws.com/company/wick.com/logo"
        },
        "count": [
            {
                "count": 0,
                "type": null
            },
            {
                "count": 0,
                "type": "FAILED"
            },
            {
                "count": 0,
                "type": "PENDING"
            },
            {
                "count": 0,
                "type": "VERIFIED"
            }
        ]
    }
}
```

Get company profile using the JWT_TOKEN.


## Get Available Packages

```shell
curl --location --request GET 'http://api-stage.us.springverify.com/company/package' \
        --header 'Authorization: Bearer JWT_TOKEN'
```

> Success Response:

```json
{
    "success": true,
    "data": {
        "package": [
            {
                "id": "1",
                "name": "bronze",
                "employment": null,
                "education": "0",
                "professional_license": "0",
                "civil_court": "0",
                "driving_license": "0",
                "county_criminal_search": "0",
                "all_county_criminal_search": false,
                "national_criminal_search": "1",
                "sex_offender_search": "1",
                "ssn_trace": "1",
                "global_watchlist": "1",
                "price": "750",
                "created_at": "2019-07-03T11:07:27.000Z",
                "updated_at": "2019-11-11T11:53:38.000Z"
            },
            {
                "id": "2",
                "name": "silver",
                "employment": null,
                "education": "0",
                "professional_license": "0",
                "civil_court": "0",
                "driving_license": "0",
                "county_criminal_search": "0",
                "all_county_criminal_search": false,
                "national_criminal_search": "1",
                "sex_offender_search": "1",
                "ssn_trace": "1",
                "global_watchlist": "1",
                "price": "1500",
                "created_at": "2019-07-03T11:07:27.000Z",
                "updated_at": "2019-11-11T11:53:38.000Z"
            },
            {
                "id": "3",
                "name": "gold",
                "employment": null,
                "education": "0",
                "professional_license": "0",
                "civil_court": "0",
                "driving_license": "0",
                "county_criminal_search": "1",
                "all_county_criminal_search": false,
                "national_criminal_search": "1",
                "sex_offender_search": "1",
                "ssn_trace": "1",
                "global_watchlist": "1",
                "price": "2000",
                "created_at": "2019-07-03T11:07:27.000Z",
                "updated_at": "2019-11-11T11:53:39.000Z"
            },
            {
                "id": "4",
                "name": "platinum",
                "employment": null,
                "education": "1",
                "professional_license": "0",
                "civil_court": "0",
                "driving_license": "0",
                "county_criminal_search": "0",
                "all_county_criminal_search": true,
                "national_criminal_search": "1",
                "sex_offender_search": "1",
                "ssn_trace": "1",
                "global_watchlist": "1",
                "price": "3000",
                "created_at": "2019-07-03T11:07:27.000Z",
                "updated_at": "2019-11-11T11:53:39.000Z"
            },
            {
                "id": "5",
                "name": "diamond",
                "employment": null,
                "education": "1",
                "professional_license": "0",
                "civil_court": "1",
                "driving_license": "0",
                "county_criminal_search": "0",
                "all_county_criminal_search": true,
                "national_criminal_search": "1",
                "sex_offender_search": "1",
                "ssn_trace": "1",
                "global_watchlist": "1",
                "price": "4000",
                "created_at": "2019-07-03T11:07:27.000Z",
                "updated_at": "2019-11-11T11:53:39.000Z"
            }
        ],
        "prices": {
            "id": "1",
            "env": "development",
            "passport": 200,
            "driving_license": 200,
            "enhanced_driving_license": 200,
            "kba": 400,
            "employment": 750,
            "education": 750,
            "professional_license": 500,
            "civil_court": 1000,
            "one_county_criminal_search": 1000,
            "all_county_criminal_search": 1000,
            "county_criminal_search": 1000,
            "package": "",
            "created_at": "2019-07-03T11:07:27.000Z",
            "updated_at": "2019-09-18T13:27:52.000Z"
        }
    }
}
```

This API is used to get the currently available packages and prices.

## Invite Candidates

```shell
curl --location --request POST 'localhost:3080/employee/invite' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer JWT_TOKEN' \
--data-raw '{
    "email_list": ["johndoe@gmail.com"],
    "package": "diamond",
    "add_ons": {
        "employment": 2,
        "education": 1,
        "license": 0,
        "driving_license": 0,
        "civil_court": 1,
        "all_county_criminal_search": true,
        "county_criminal_search": 0
    },
    coupon_code:""
}'
```

>Success Response:

```json
{
    "success": true,
    "data": {
        "price": 4000,
        "id": "2612d112-5827-4b1c-a177-06a85eabd03d",
        "count": 1
    }
}
```

AIM of this API is used to invite employee/employees. It can be used to invite employee in bulk. Once payment is done sucessfully then email is sent to the employee.

<aside class="notice">This API should only be used after getting the packages in previous API.</aside>

### URL Parameters

Parameter | Type | Description
--------- | ------- | -----------
email_list|array|Contains the list of employee emails.
package|string|Name of the package (retrieved in get packages api).
add_ons|Object|Extra checks can be added in a package.
coupon_code|string|This coupon Code is to avail discounts.

### Response Parameters

Parameter | Type | Description
--------- | ------- | -----------
price|number|It gives price in cents that has to be payed.
id|string|It is the reference id against which payment has to be made.
count|number|It gives the number of candidates.
## Save Credit Card in Stripe for Payments


```shell
curl --location --request POST 'https://api.stripe.com/v1/tokens' \
--header 'Authorization: Bearer pk_test_51H1niLFq1aDrrzKzoQEG7espm3z3HirSoy5IJl8tWbxJk18pZDx67Y70mCynyLKOrEJFWarCiVSigW9RuW1bqdeU00lRNETXxe' \
--header 'Content-Type: application/x-www-form-urlencoded' \
--data-urlencode 'card[number]=4242424242424242' \
--data-urlencode 'card[exp_month]=12' \
--data-urlencode 'card[exp_year]=2020' \
--data-urlencode 'card[cvc]=123'
```

> Success Response

```json
{
  "id": "tok_1GidnJ4wweuFtc0n81Lzq51P",
  "object": "token",
  "card": {
    "id": "card_1GidnJ4wweuFtc0nu2WUlvc5",
    "object": "card",
    "address_city": null,
    "address_country": null,
    "address_line1": null,
    "address_line1_check": null,
    "address_line2": null,
    "address_state": null,
    "address_zip": null,
    "address_zip_check": null,
    "brand": "Visa",
    "country": "US",
    "customer": null,
    "cvc_check": null,
    "dynamic_last4": null,
    "exp_month": 12,
    "exp_year": 2020,
    "fingerprint": "hV2YANZB970jzVzt",
    "funding": "credit",
    "last4": "4242",
    "metadata": {},
    "name": null,
    "tokenization_method": null
  },
  "client_ip": "106.51.30.155",
  "created": 1589450161,
  "livemode": false,
  "type": "card",
  "used": false
}
```

This API is used to register your Card with Stripe. Once the API call is successful the response will incude an `id` parameter which will be used in the next API request for authorizing payments (Payment For Invites).

## Save the Payment Info

```shell
curl --location --request POST 'https://api.us.springverify.com/payment/save-card' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer JWT_TOKEN' \
--data-raw '{
	"source":"tok_1EkXOa4wweuFtc0nQd0eOOhs"
}'
```

The token received from the previous API has to be saved with SpringVerify.

> Success Response

```json
{
    "success": true,
    "data": "card saved for the specified user"
}
```

## Payment For Invites

```shell
curl --location --request POST 'https://api.us.springverify.com/payment/charge-user' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer JWT_TOKEN' \
--data-raw '{
    "id": "2612d112-5827-4b1c-a177-03a85eabd03d"
}'
```

>Success Response

```json
{
    "success": true,
    "data": {
        "id": "ch_1HJdvnFq1aDrrzKzZE6729eZ",
        "object": "charge",
        "amount": 9999,
        "amount_refunded": 0,
        "application": null,
        "application_fee": null,
        "application_fee_amount": null,
        "balance_transaction": "txn_1HJdvnFq1aDrrzKzbZXCgvAO",
        "billing_details": {
            "address": {
                "city": null,
                "country": null,
                "line1": null,
                "line2": null,
                "postal_code": "42424",
                "state": null
            },
            "email": null,
            "name": "johndoe@gmail.com",
            "phone": null
        },
        "calculated_statement_descriptor": "SPRINGVERIFY.COM",
        "captured": true,
        "created": 1598268823,
        "currency": "usd",
        "customer": "cus_HjgWvwfPN7bKob",
        "description": null,
        "destination": null,
        "dispute": null,
        "disputed": false,
        "failure_code": null,
        "failure_message": null,
        "fraud_details": {},
        "invoice": null,
        "livemode": false,
        "metadata": {},
        "on_behalf_of": null,
        "order": null,
        "outcome": {
            "network_status": "approved_by_network",
            "reason": null,
            "risk_level": "normal",
            "risk_score": 28,
            "seller_message": "Payment complete.",
            "type": "authorized"
        },
        "paid": true,
        "payment_intent": null,
        "payment_method": "card_1HAD9gFq1aDrrzKzPZUtu418",
        "payment_method_details": {
            "card": {
                "brand": "visa",
                "checks": {
                    "address_line1_check": null,
                    "address_postal_code_check": "pass",
                    "cvc_check": null
                },
                "country": "US",
                "exp_month": 4,
                "exp_year": 2024,
                "fingerprint": "IpgpMeLB1qGPPEnT",
                "funding": "credit",
                "installments": null,
                "last4": "4242",
                "network": "visa",
                "three_d_secure": null,
                "wallet": null
            },
            "type": "card"
        },
        "receipt_email": "johndoe@gmail.com",
        "receipt_number": null,
        "receipt_url": "receipt_url_link",
        "refunded": false,
        "refunds": {
            "object": "list",
            "data": [],
            "has_more": false,
            "total_count": 0,
            "url": "/v1/charges/ch_1HJdvnFq1aDrrzKzZE6729eZ/refunds"
        },
        "review": null,
        "shipping": null,
        "source": {
            "id": "card_1HAD9gFq1aDrrzKzPZUtu418",
            "object": "card",
            "address_city": null,
            "address_country": null,
            "address_line1": null,
            "address_line1_check": null,
            "address_line2": null,
            "address_state": null,
            "address_zip": "42424",
            "address_zip_check": "pass",
            "brand": "Visa",
            "country": "US",
            "customer": "cus_HjgWvwfPN7bKob",
            "cvc_check": null,
            "dynamic_last4": null,
            "exp_month": 4,
            "exp_year": 2024,
            "fingerprint": "IygpMeLB1qGPPEnT",
            "funding": "credit",
            "last4": "4242",
            "metadata": {},
            "name": "johndoe@gmail.com",
            "tokenization_method": null
        },
        "source_transfer": null,
        "statement_descriptor": null,
        "statement_descriptor_suffix": null,
        "status": "succeeded",
        "transfer_data": null,
        "transfer_group": null
    }
}
```

This API is called right after invite API. The reference id retrieved in previous api is used in this api (Invite Candidates). Once the payment is done successfully the email is sent out to the candidate.

### URL Parameters

Parameter | Type | Description
--------- | ------- | -----------
id|string|Reference id retrieved in invite employee API.


## Get Single Candidate

```shell
curl --location --request GET 'https://api.us.springverify.com/company/employee?id=EMPLOYEE_ID' \
--header 'Authorization: Bearer JWT_TOKEN' \
--data-raw ''
```

> Success Response

```json
{
    "success": true,
    "data": {
        "employee": {
            "id": "6d6d81f5-6f00-4a38-a7d7-249cc2127ab6",
            "access_id": null,
            "email": "johndoe@gmail.com",
            "password_hash": null,
            "first_name": null,
            "middle_name": null,
            "last_name": null,
            "name_verified": null,
            "created_at": "2020-08-24T10:57:01.000Z",
            "updated_at": "2020-08-24T10:57:01.000Z",
            "employer_id": "fd725660-58f7-4a97-b626-5a4588e4ca31",
            "payment_id": "0e9a7695-91bd-4e60-aefc-ba47f80ab0f0",
            "email_sent": true,
            "payment": false,
            "status": null,
            "flow_completed": null,
            "company_created_by": "john@wick.com",
            "employee_limit_id": "bd95351f-fc2a-4ff9-8652-88abbfe67bdd",
            "kbaqna": null,
            "employments": [],
            "education": [],
            "professional_licenses": [],
            "employee_limit": {
                "id": "bd95351f-fc2a-4ff9-8652-88abbfe67bdd",
                "employment": 2,
                "education": 1,
                "professional_license": 0,
                "all_county_criminal_search": true,
                "county_criminal_search": 0,
                "civil_court": 1,
                "driving_license": 0,
                "package_id": null,
                "created_at": "2020-08-24T10:57:01.000Z",
                "updated_at": "2020-08-24T10:57:01.000Z",
                "employee_invite_group_id": "4153a150-4157-48ab-b258-9a9b47a05fc2",
                "employee_invite_group": {
                    "id": "4153a150-4157-48ab-b258-9a9b47a05fc2",
                    "package": "diamond",
                    "active": true,
                    "created_at": "2020-08-24T10:57:00.000Z",
                    "updated_at": "2020-08-24T10:57:00.000Z",
                    "company_created_by": "john@wick.com",
                    "package_id": "5"
                }
            },
            "employee_detail": null,
            "employee_verification": null,
            "s3_files": [],
            "criminal_statuses": [],
            "cic_criminal_records": [],
            "sjv_criminal_reports": []
        },
        "review": [],
        "criminal_status": {
            "national_criminal": "PENDING",
            "sex_offender": "PENDING",
            "global_watchlist": "PENDING",
            "county_criminal_search": "PENDING",
            "civil_court": "PENDING",
            "overall_criminal_status": "PENDING"
        }
    }
}
```

AIM of this API is used to get details of a particular employee.

### URL Parameters

Parameter | Type | Description
--------- | ------- | -----------
id|uuid|Contains the unique id of the employee.


## Search Employee in Company

```shell
curl --location --request POST 'https://api.us.springverify.com/company/employee/search' \
--header 'Authorization: Bearer JWT_TOKEN' \
--header 'Content-Type: application/json' \
--data-raw '{
    "search":"johndoe@gmail.com"
}'
```

> Success Response

```json
{
    "success": true,
    "data": [
        {
            "id": "6d6d81f5-6f00-4a38-a7d7-249cc2127ab6",
            "email": "johndoe@gmail.com",
            "first_name": null,
            "middle_name": null,
            "last_name": null,
            "status": null,
            "employee_limit": {
                "id": "bd95351f-fc2a-4ff9-8652-88abbfe67bdd",
                "employee_invite_group": {
                    "id": "4153a150-4157-48ab-b258-9a9b47a05fc2",
                    "package": "diamond"
                }
            }
        }
    ]
}
```

AIM of this API is used to search a particular employee in your company. 


### URL Parameters

Parameter | Type | Description
--------- | ------- | -----------
search|string|Contains the search term.


## Get Company Actions

```shell
curl --location --request POST 'https://api.us.springverify.com/company/actions' \
--header 'Authorization: Bearer JWT_TOKEN' \
--header 'Content-Type: application/json' \
--data-raw '{
    "limit":10,
    "offset":0
}'
```

> Success Response

```json
{
  "success": true,
  "data": [
    {
      "string": "identity verification failed. (using their driving license)",
      "time": "2020-08-27T05:59:02.000Z",
      "id": "9282698a-72ac-49df-a750-85c169bfe08e",
      "first_name": "John",
      "middle_name": "Mark",
      "last_name": "Smith",
      "email": "johndoe@gmail.com"
    },
    {
      "string": "identity verification failed. (using their driving license)",
      "time": "2020-08-27T05:59:02.000Z",
      "id": "9282698a-72ac-49df-a750-85c169bfe08e",
      "first_name": "John",
      "middle_name": "Mark",
      "last_name": "Smith",
      "email": "johndoe@gmail.com"
    },
    {
      "string": "identity verification failed. (using their driving license)",
      "time": "2020-08-27T05:59:02.000Z",
      "id": "9282698a-72ac-49df-a750-85c169bfe08e",
      "first_name": "John",
      "middle_name": "Mark",
      "last_name": "Smith",
      "email": "johndoe@gmail.com"
    },
    {
      "string": "has successfully verified their identity. (using their driving license)",
      "time": "2020-08-27T05:57:03.000Z",
      "id": "9282698a-72ac-49df-a750-85c169bfe08e",
      "first_name": "John",
      "middle_name": "Mark",
      "last_name": "Smith",
      "email": "johndoe@gmail.com"
    },
    {
      "string": "has uploaded their driving license.",
      "time": "2020-08-27T05:56:44.000Z",
      "id": "9282698a-72ac-49df-a750-85c169bfe08e",
      "first_name": "John",
      "middle_name": "Mark",
      "last_name": "Smith",
      "email": "johndoe@gmail.com"
    },
    {
      "string": "has entered personal details.",
      "time": "2020-08-27T05:55:40.000Z",
      "id": "9282698a-72ac-49df-a750-85c169bfe08e",
      "first_name": "John",
      "middle_name": "Mark",
      "last_name": "Smith",
      "email": "johndoe@gmail.com"
    },
    {
      "string": "has been added as a candidate and invited to enter his information.",
      "time": "2020-08-27T05:54:24.000Z",
      "id": "9282698a-72ac-49df-a750-85c169bfe08e",
      "first_name": "John",
      "middle_name": "Mark",
      "last_name": "Smith",
      "email": "johndoe@gmail.com"
    },
    {
      "string": "identity verification failed. (using their driving license)",
      "time": "2020-08-27T04:29:02.000Z",
      "id": "8bf63c9a-b89c-4dff-a03c-0d571dc4c47e",
      "first_name": "John",
      "middle_name": "Mark",
      "last_name": "Smith",
      "email": "johndoe@gmail.com"
    },
    {
      "string": "identity verification failed. (using their driving license)",
      "time": "2020-08-27T04:29:02.000Z",
      "id": "8bf63c9a-b89c-4dff-a03c-0d571dc4c47e",
      "first_name": "John",
      "middle_name": "Mark",
      "last_name": "Smith",
      "email": "johndoe@gmail.com"
    },
    {
      "string": "identity verification failed. (using their driving license)",
      "time": "2020-08-27T04:29:02.000Z",
      "id": "8bf63c9a-b89c-4dff-a03c-0d571dc4c47e",
      "first_name": "John",
      "middle_name": "Mark",
      "last_name": "Smith",
      "email": "johndoe@gmail.com"
    }
  ]
}
```


## Get Company Actions Pertaining to an employee

```shell
curl --location --request POST 'https://api.us.springverify.com/company/actions' \
--header 'Authorization: Bearer JWT_TOKEN' \
--header 'Content-Type: application/json' \
--data-raw '{
    "limit":10,
    "offset":0,
    "email":"employee@email.com"
}'
```

> Success Response

```json
{
  "success": true,
  "data": [
    {
      "string": "employment check is in progress.",
      "time": "2020-08-13T14:32:50.000Z",
      "id": "298af70a-b3a3-4605-8789-37387bb276a1",
      "first_name": "Mark",
      "middle_name": "James",
      "last_name": "Smith",
      "email": "johhndoe@gmail.com"
    },
    {
      "string": "education check is in progress.",
      "time": "2020-08-13T14:32:08.000Z",
      "id": "298af70a-b3a3-4605-8789-37387bb276a1",
      "first_name": "Mark",
      "middle_name": "James",
      "last_name": "Smith",
      "email": "johhndoe@gmail.com"
    },
    {
      "string": "has entered their education.",
      "time": "2020-08-13T14:31:34.000Z",
      "id": "298af70a-b3a3-4605-8789-37387bb276a1",
      "first_name": "Mark",
      "middle_name": "James",
      "last_name": "Smith",
      "email": "johhndoe@gmail.com"
    },
    {
      "string": "has entered their employment.",
      "time": "2020-08-13T14:26:25.000Z",
      "id": "298af70a-b3a3-4605-8789-37387bb276a1",
      "first_name": "Mark",
      "middle_name": "James",
      "last_name": "Smith",
      "email": "johhndoe@gmail.com"
    },
    {
      "string": "has successfully verified their identity. (using their driving license)",
      "time": "2020-08-13T14:24:44.000Z",
      "id": "298af70a-b3a3-4605-8789-37387bb276a1",
      "first_name": "Mark",
      "middle_name": "James",
      "last_name": "Smith",
      "email": "johhndoe@gmail.com"
    },
    {
      "string": "has uploaded their driving license.",
      "time": "2020-08-13T14:23:15.000Z",
      "id": "298af70a-b3a3-4605-8789-37387bb276a1",
      "first_name": "Mark",
      "middle_name": "James",
      "last_name": "Smith",
      "email": "johhndoe@gmail.com"
    },
    {
      "string": "has entered personal details.",
      "time": "2020-08-13T14:21:13.000Z",
      "id": "298af70a-b3a3-4605-8789-37387bb276a1",
      "first_name": "Mark",
      "middle_name": "James",
      "last_name": "Smith",
      "email": "johhndoe@gmail.com"
    },
    {
      "string": "has been added as a candidate and invited to enter his information.",
      "time": "2020-08-13T14:19:32.000Z",
      "id": "298af70a-b3a3-4605-8789-37387bb276a1",
      "first_name": "Mark",
      "middle_name": "James",
      "last_name": "Smith",
      "email": "johhndoe@gmail.com"
    }
  ]
}
```


## Get Company Employees by Filter

```shell
curl --location --request POST 'https://api.us.springverify.com/company/employees' \
--header 'Authorization: Bearer JWT_TOKEN' \
--header 'Content-Type: application/json' \
--data-raw '{
    "limit":1,
    "offset":0,
    "filter":"NULL"
}'
```

> Success Response

```json
{
    "success": true,
    "data": {
        "employees": [{
            "id": "5d7bea97-fdca-4f2b-be32-72637a3491f4",
            "email": "johbobby@Johns.com",
            "first_name": "John",
            "middle_name": "Bobby",
            "last_name": "Johns",
            "status": "PENDING",
            "created_at": "2020-08-19T16:32:11.000Z",
            "kbaqna": null,
            "employments": [{
                "status": 0,
                "super_admin_status": null,
                "status_new": null,
                "super_admin_status_new": null,
                "adverse_action": null
            }],
            "education": [{
                "status": 3,
                "super_admin_status": null,
                "status_new": null,
                "super_admin_status_new": null,
                "adverse_action": null
            }],
            "professional_licenses": [],
            "employee_limit": {
                "employment": 2,
                "education": 1,
                "professional_license": 0,
                "civil_court": 0,
                "county_criminal_search": 0,
                "all_county_criminal_search": true,
                "employee_invite_group": {
                    "package": "platinum"
                }
            },
            "employee_verification": {
                "s3_passport_verified": null,
                "s3_dl_verified": 1,
                "verification_type": "id",
                "super_admin_status": null,
                "passport_status": null,
                "dl_status": null,
                "super_admin_status_new": null
            },
            "criminal_statuses": {
                "national_criminal": "PENDING",
                "sex_offender": "PENDING",
                "global_watchlist": "PENDING",
                "county_criminal_search": "PENDING"
            },
            "cic_criminal_records": [{
                    "id": "8b454419-8691-4688-9719-72571012978c",
                    "record_type": "SEX_OFFENDER",
                    "reviewed": false,
                    "adverse_action": {
                        "status": "CLOSED",
                        "cleared": false,
                        "id": "af1cef6e-e38f-4545-8fc4-6a31c0886b37"
                    }
                },
                {
                    "id": "ade7b7cd-c80c-4302-a2f0-6066f81322b3",
                    "record_type": "SEX_OFFENDER",
                    "reviewed": true,
                    "adverse_action": {
                        "status": "CLOSED",
                        "cleared": false,
                        "id": "af1cef6e-e38f-4545-8fc4-6a31c0886b37"
                    }
                }
            ],
            "sjv_criminal_reports": [{
                    "id": "428fc0d7-9122-4eed-8a76-a382be5e847f",
                    "sjv_search_type": "COUNTY_CRIMINAL",
                    "county_name": {
                        "county_name": "Ocean County"
                    },
                    "adverse_action": {
                        "status": "PENDING",
                        "cleared": null,
                        "id": "350849d9-24c2-40ea-ad20-d5654854fac9"
                    }
                },
                {
                    "id": "281edcbe-cdb4-411e-8d8e-87f7aea33b2a",
                    "sjv_search_type": "COUNTY_CRIMINAL_NOTIFICATION",
                    "county_name": null,
                    "adverse_action": null
                },
                {
                    "id": "96b6aef0-1114-4e5b-99a4-f9ed917504a2",
                    "sjv_search_type": "NATIONAL_CRIMINAL",
                    "county_name": {
                        "county_name": "Palm Beach County"
                    },
                    "adverse_action": null
                }
            ]
        }],
        "count": 238
    }
}
```

### URL Parameters

Parameter | Type | Description
--------- | ------- | -----------
limit|integer|Page limit.
offset|integer|Page offset.
filter|string|ALL/COMPLETED/VERIFIED/PENDING/NULL(AWAITING INPUT FROM EMPLOYEE)/FAILED.


## Get Company Adverse Actoion Counts

```shell
curl --location --request GET 'https://api.us.springverify.com/company/action/adverse/counts' \
--header 'Authorization: Bearer JWT_TOKEN'
```

> Success Response

```json
{
    "message": "successfully retrieved count",
    "data": {
        "PENDING": {
            "pendingCount": 4,
            "totalCount": 8
        },
        "NOTICE_SENT": {
            "pendingCount": 2,
            "totalCount": 19
        },
        "IN_REVIEW": {
            "pendingCount": 2,
            "totalCount": 3
        },
        "FINAL_CALL": {
            "pendingCount": 0,
            "totalCount": 24
        },
        "CLOSED": {
            "pendingCount": 5,
            "totalCount": 30
        }
    }
}
```

## Get Company Adverse Actions

```shell
curl --location --request POST 'https://api.us.springverify.com/company/action/adverse/fetch' \
--header 'Authorization: Bearer JWT_TOKEN' \
--header 'Content-Type: application/json' \
--data-raw '{
    "limit":10,
    "offset":0,
    "status":"PENDING"
}'
```

> Success Response

```json
{
    "success": true,
    "data": {
        "adverse_actions": [
            {
                "id": "2504d7c7-3f68-47a7-b9dc-be2adb99936e",
                "check_id": null,
                "status": "PENDING",
                "type": "SEX_OFFENDER",
                "cleared": null,
                "comments": null,
                "created_at": "2020-08-25T08:17:24.000Z",
                "updated_at": "2020-08-25T08:17:24.000Z",
                "deleted_at": null,
                "education": null,
                "employment": null,
                "read_statuses": [],
                "cic_criminal_records": [
                    {
                        "id": "0548ea1c-b3de-4008-8147-1376c5870b8f",
                        "employee_email_fk": "johndoe@gmail.com",
                        "cic_criminal_report_id_fk": "62b71576-8456-42db-8ce9-4d12b2d864cb",
                        "created_at": "2020-08-13T14:24:42.000Z",
                        "updated_at": "2020-08-25T08:17:24.000Z",
                        "employee": {
                            "id": "298af70a-b3a3-4605-8789-37387bb276a1",
                            "access_id": "05065b6d-8272-4ba5-b31e-b9f576c0a44a",
                            "email": "johndoe@gmail.com",
                            "password_hash": "qwerty123",
                            "first_name": "John",
                            "middle_name": "Mark",
                            "last_name": "Smith",
                            "name_verified": null,
                            "created_at": "2020-08-13T13:59:22.000Z",
                            "updated_at": "2020-08-25T08:17:24.000Z",
                            "employer_id": "1d4fb8ba-09ac-412c-aa62-58970b4d7472",
                            "payment_id": "3aa91fd2-aca5-456b-a375-cf4df6ededed",
                            "email_sent": true,
                            "payment": false,
                            "status": "VERIFIED",
                            "flow_completed": true,
                            "company_created_by": "john@wick.com",
                            "employee_limit_id": "0e3ee39a-32c2-4eb6-a7f2-61a3507a26ba"
                        }
                    }
                ],
                "sjv_criminal_reports": []
            },
            {
                "id": "df359f7d-fff3-4de6-bbfa-f3719b1f4b9b",
                "check_id": null,
                "status": "PENDING",
                "type": "SEX_OFFENDER",
                "cleared": null,
                "comments": null,
                "created_at": "2020-08-20T12:45:29.000Z",
                "updated_at": "2020-08-20T12:45:29.000Z",
                "deleted_at": null,
                "education": null,
                "employment": null,
                "read_statuses": [],
                "cic_criminal_records": [
                    {
                        "id": "271e6b31-cef1-4b67-a8b5-28a1781081c5",
                        "employee_email_fk": "johndoe@gmail.com",
                        "cic_criminal_report_id_fk": "45127f48-b878-4aa5-8bab-881b79883d4a",
                        "created_at": "2020-08-20T12:07:49.000Z",
                        "updated_at": "2020-08-20T12:45:29.000Z",
                        "employee": {
                            "id": "37d40f7f-90a9-415d-8679-75500654aa5e",
                            "access_id": "3d5ed4fd-f2c4-4d86-94c0-c8836fe12132",
                            "email": "johndoe@gmail.com",
                            "password_hash": "100d5fcb45dc552d1a9011e2707b937904f79df410199fcb7a1e2b3c022d9911",
                            "first_name": "John",
                            "middle_name": "Mark",
                            "last_name": "Smith",
                            "name_verified": null,
                            "created_at": "2020-08-20T11:27:01.000Z",
                            "updated_at": "2020-08-20T12:45:30.000Z",
                            "employer_id": "1d4fb8ba-09ac-412c-aa62-58970b4d7472",
                            "payment_id": "881e74c4-0c8c-4c7c-9238-be5053474ee2",
                            "email_sent": true,
                            "payment": false,
                            "status": "FAILED",
                            "flow_completed": true,
                            "company_created_by": "john@wick.com",
                            "employee_limit_id": "8a51971e-bf6f-4b03-9db9-9062546e0533"
                        }
                    }
                ],
                "sjv_criminal_reports": []
            },
            {
                "id": "350849d9-24c2-40ea-ad20-d5654854fac9",
                "check_id": null,
                "status": "PENDING",
                "type": "COUNTY_CRIMINAL",
                "cleared": null,
                "comments": null,
                "created_at": "2020-08-19T18:11:53.000Z",
                "updated_at": "2020-08-19T18:11:53.000Z",
                "deleted_at": null,
                "education": null,
                "employment": null,
                "read_statuses": [],
                "cic_criminal_records": [],
                "sjv_criminal_reports": [
                    {
                        "id": "428fc0d7-9122-4eed-8a76-a382be5e847f",
                        "employee_email_fk": "johndoe@gmail.com",
                        "status": "RECEIVED",
                        "sjv_search_type": "COUNTY_CRIMINAL",
                        "reference_id": null,
                        "cic_criminal_record_fk": null,
                        "report_link": "report_link",
                        "marked_done": 0,
                        "marked_reviewed": 0,
                        "county_id": 1789,
                        "adverse_action_fk": "350849d9-24c2-40ea-ad20-d5654854fac9",
                        "authenticating_unique_identifier": "08bdee45-0cf6-4630-9f34-a9b0e50d2fcf",
                        "created_at": "2020-08-19T18:11:51.000Z",
                        "updated_at": "2020-08-19T18:11:53.000Z",
                        "county_name": {
                            "id": "1789",
                            "county_name": "Ocean County",
                            "state_name": "New Jersey",
                            "created_at": null,
                            "updated_at": null
                        },
                        "employee": {
                            "id": "5d7bea97-fdca-4f2b-be32-72637a3491f4",
                            "access_id": "67a61c82-8bf6-4717-8df1-a0ece4ffd2c8",
                            "email": "johndoe@gmail.com",
                            "password_hash": "100d5fcb45dc552d1a9011e2707b937904f79df410199fcb7a1e2b3c022d9911",
                            "first_name": "Jane",
                            "middle_name": "Elizabeth",
                            "last_name": "Smith",
                            "name_verified": null,
                            "created_at": "2020-08-19T16:32:11.000Z",
                            "updated_at": "2020-08-19T17:45:05.000Z",
                            "employer_id": "1d4fb8ba-09ac-412c-aa62-58970b4d7472",
                            "payment_id": "c26bc8a4-c136-4af6-bf2e-497745597085",
                            "email_sent": true,
                            "payment": false,
                            "status": "PENDING",
                            "flow_completed": true,
                            "company_created_by": "john@wick.com",
                            "employee_limit_id": "372b4cc6-63ce-4811-9d80-eb0527bce1ea"
                        }
                    }
                ]
            },
            {
                "id": "5c0a073a-ebd8-4417-989f-7ac5f3b11f98",
                "check_id": "b33aaaa7-5253-4542-a18f-ab4ff91ae8d3",
                "status": "PENDING",
                "type": "EMPLOYMENT",
                "cleared": null,
                "comments": null,
                "created_at": "2020-08-12T13:59:03.000Z",
                "updated_at": "2020-08-12T13:59:03.000Z",
                "deleted_at": null,
                "education": null,
                "employment": {
                    "id": "b33aaaa7-5253-4542-a18f-ab4ff91ae8d3",
                    "email": "johndoe@gmail.com",
                    "access_id": null,
                    "employer_name": "Nags",
                    "employer_name_verified": null,
                    "employer_phone": null,
                    "employer_phone_verified": null,
                    "employer_address": "Philadelphia International Airport (PHL), Essington Ave, Philadelphia, PA, USA",
                    "employer_address_verified": null,
                    "employer_town": "Philadelphia",
                    "employer_town_verified": null,
                    "state": "PA",
                    "state_verified": null,
                    "zipcode": "19153",
                    "zipcode_verified": null,
                    "employer_country": "United States",
                    "employer_country_verified": null,
                    "job_title": "sefdwe",
                    "job_title_verified": null,
                    "start_date": "01-01-2010",
                    "start_date_verified": null,
                    "end_date": "01-01-2018",
                    "end_date_verified": null,
                    "supervisor_name": "ber",
                    "supervisor_contact": null,
                    "current_employment": null,
                    "current_employment_verified": null,
                    "contract_type": null,
                    "contract_type_verified": null,
                    "source": null,
                    "created_at": "2020-08-12T13:58:20.000Z",
                    "updated_at": "2020-08-19T14:14:15.000Z",
                    "job_type": "full time employee",
                    "reason_for_leaving": null,
                    "status": 2,
                    "status_new": "FAILED",
                    "super_admin_status": null,
                    "super_admin_status_new": null,
                    "consent": false,
                    "employee": {
                        "id": "ee561c16-db51-4da0-a2b5-57780703ed3c",
                        "access_id": "36761e70-abcc-4d25-8b88-cf9f5ecd0ae5",
                        "email": "johndoe@gmail.com",
                        "password_hash": null,
                        "first_name": "Jane",
                        "middle_name": "Elizabeth",
                        "last_name": "Smith",
                        "name_verified": null,
                        "created_at": "2020-08-12T13:53:46.000Z",
                        "updated_at": "2020-08-12T13:55:43.000Z",
                        "employer_id": "1d4fb8ba-09ac-412c-aa62-58970b4d7472",
                        "payment_id": "b76847d7-fd23-4a64-8fb9-5d57f3c2693a",
                        "email_sent": true,
                        "payment": false,
                        "status": null,
                        "flow_completed": null,
                        "company_created_by": "john@wick.com",
                        "employee_limit_id": "2621a7d1-b00c-408d-99bc-c1f072e4c8ac"
                    }
                },
                "read_statuses": [],
                "cic_criminal_records": [],
                "sjv_criminal_reports": []
            },
            {
                "id": "b376ef1b-65d7-4f5a-ab0c-ccb81a05f55b",
                "check_id": null,
                "status": "PENDING",
                "type": "SEX_OFFENDER",
                "cleared": null,
                "comments": null,
                "created_at": "2020-07-17T15:11:45.000Z",
                "updated_at": "2020-07-17T15:11:45.000Z",
                "deleted_at": null,
                "education": null,
                "employment": null,
                "read_statuses": [
                    {
                        "id": "772ae675-f222-4baa-b31b-75007cba318a",
                        "reference_id": "b376ef1b-65d7-4f5a-ab0c-ccb81a05f55b",
                        "user_email": "john@wick.com",
                        "read": true,
                        "table": null,
                        "user_role": null,
                        "created_at": "2020-07-17T15:38:28.000Z",
                        "updated_at": "2020-07-17T15:38:28.000Z"
                    }
                ],
                "cic_criminal_records": [
                    {
                        "id": "bdb73700-4445-4954-8b36-9c144c46ab29",
                        "employee_email_fk": "johndoe@gmail.com",
                        "cic_criminal_report_id_fk": "234220d6-1a3b-49c5-9ea1-a1daa1cb710f",
                        "created_at": "2020-07-17T15:09:02.000Z",
                        "updated_at": "2020-07-20T13:32:48.000Z",
                        "employee": {
                            "id": "7cdbce62-b788-461a-8560-3264d10eeaac",
                            "access_id": "ce907ccd-ccd6-4aab-bc81-e2b7053fd915",
                            "email": "johndoe@gmail.com",
                            "password_hash": "100d5fcb45dc552d1a9011e2707b937904f79df410199fcb7a1e2b3c022d9911",
                            "first_name": "Jane",
                            "middle_name": "Elizabeth",
                            "last_name": "Smith",
                            "name_verified": null,
                            "created_at": "2020-07-17T15:06:30.000Z",
                            "updated_at": "2020-07-17T15:09:29.000Z",
                            "employer_id": "1d4fb8ba-09ac-412c-aa62-58970b4d7472",
                            "payment_id": "8779db6f-99a8-4d2d-b05a-6481112f1c5c",
                            "email_sent": true,
                            "payment": false,
                            "status": "PENDING",
                            "flow_completed": true,
                            "company_created_by": "john@wick.com",
                            "employee_limit_id": "797dbc70-04df-4d26-8140-dfd11bdb37aa"
                        }
                    }
                ],
                "sjv_criminal_reports": []
            },
            {
                "id": "0c495ae6-756b-4681-b295-3b0f8ea96eac",
                "check_id": null,
                "status": "PENDING",
                "type": "SEX_OFFENDER",
                "cleared": null,
                "comments": null,
                "created_at": "2020-07-09T14:57:35.000Z",
                "updated_at": "2020-07-09T14:57:35.000Z",
                "deleted_at": null,
                "education": null,
                "employment": null,
                "read_statuses": [
                    {
                        "id": "7347e6c6-d863-4de4-b945-049dc00583c6",
                        "reference_id": "0c495ae6-756b-4681-b295-3b0f8ea96eac",
                        "user_email": "john@wick.com",
                        "read": true,
                        "table": null,
                        "user_role": null,
                        "created_at": "2020-07-24T10:49:38.000Z",
                        "updated_at": "2020-07-24T10:49:38.000Z"
                    }
                ],
                "cic_criminal_records": [
                    {
                        "id": "e8e33362-8fb0-4fed-8e5c-b2f6575f33bf",
                        "employee_email_fk": "johndoe@gmail.com",
                        "cic_criminal_report_id_fk": "3dca9124-83e9-4a2c-adac-a16b80e40fa5",
                        "created_at": "2020-07-09T10:52:32.000Z",
                        "updated_at": "2020-07-09T14:57:35.000Z",
                        "employee": {
                            "id": "00890df3-2595-4cb9-9a76-6d743277ef21",
                            "access_id": "573ab2de-9aea-4355-8c73-34329f815ac4",
                            "email": "johndoe@gmail.com",
                            "password_hash": null,
                            "first_name": "John",
                            "middle_name": "Mark",
                            "last_name": "Smith",
                            "name_verified": null,
                            "created_at": "2020-07-09T10:38:53.000Z",
                            "updated_at": "2020-07-09T10:43:23.000Z",
                            "employer_id": "1d4fb8ba-09ac-412c-aa62-58970b4d7472",
                            "payment_id": "acf07517-7b5f-4145-8b19-311e3b42ca9e",
                            "email_sent": true,
                            "payment": false,
                            "status": null,
                            "flow_completed": false,
                            "company_created_by": "john@wick.com",
                            "employee_limit_id": "9bd3df0b-eabe-4e2e-bf29-0d8f9448b977"
                        }
                    }
                ],
                "sjv_criminal_reports": []
            },
            {
                "id": "9f27106e-3536-429e-b33c-a6c6949552cd",
                "check_id": "734bbf7d-fada-490d-a448-68601f29b235",
                "status": "PENDING",
                "type": "EMPLOYMENT",
                "cleared": null,
                "comments": null,
                "created_at": "2020-06-26T13:44:02.000Z",
                "updated_at": "2020-06-26T13:44:02.000Z",
                "deleted_at": null,
                "education": null,
                "employment": {
                    "id": "734bbf7d-fada-490d-a448-68601f29b235",
                    "email": "kavya@yopmail.com",
                    "access_id": null,
                    "employer_name": "Reliance",
                    "employer_name_verified": null,
                    "employer_phone": "1-9089877877",
                    "employer_phone_verified": null,
                    "employer_address": "3045 West 26th Street, Chicago, IL, USA",
                    "employer_address_verified": null,
                    "employer_town": "Chicago",
                    "employer_town_verified": null,
                    "state": "IL",
                    "state_verified": null,
                    "zipcode": "60623",
                    "zipcode_verified": null,
                    "employer_country": "United States",
                    "employer_country_verified": null,
                    "job_title": "SDET",
                    "job_title_verified": null,
                    "start_date": "01-01-2010",
                    "start_date_verified": null,
                    "end_date": "01-01-2017",
                    "end_date_verified": null,
                    "supervisor_name": "Kiya",
                    "supervisor_contact": null,
                    "current_employment": null,
                    "current_employment_verified": null,
                    "contract_type": null,
                    "contract_type_verified": null,
                    "source": null,
                    "created_at": "2020-06-26T13:39:30.000Z",
                    "updated_at": "2020-08-19T14:14:15.000Z",
                    "job_type": "full time employee",
                    "reason_for_leaving": null,
                    "status": 2,
                    "status_new": "FAILED",
                    "super_admin_status": null,
                    "super_admin_status_new": null,
                    "consent": false,
                    "employee": {
                        "id": "e4782824-57ac-49da-9a7c-f5d2633d73c1",
                        "access_id": "a7f3d1c0-2b52-48a5-b49d-d1bbe1f5b25c",
                        "email": "kavya@yopmail.com",
                        "password_hash": "319f97ff7cbeee9de027f4b4ae09b3e50b05a747d5b1ebd6bed4f8014fdd1ca2",
                        "first_name": "John",
                        "middle_name": "Mark",
                        "last_name": "Smith",
                        "name_verified": null,
                        "created_at": "2019-12-26T09:03:33.000Z",
                        "updated_at": "2020-06-26T13:39:51.000Z",
                        "employer_id": "1d4fb8ba-09ac-412c-aa62-58970b4d7472",
                        "payment_id": "1cfefa51-6698-42f7-a864-eb377509b4a6",
                        "email_sent": true,
                        "payment": false,
                        "status": "PENDING",
                        "flow_completed": true,
                        "company_created_by": "john@wick.com",
                        "employee_limit_id": "7dae42ca-5718-4fb3-ab21-74099c737c9c"
                    }
                },
                "read_statuses": [
                    {
                        "id": "dc1955a0-4903-4621-80e5-4dc2224e4edc",
                        "reference_id": "9f27106e-3536-429e-b33c-a6c6949552cd",
                        "user_email": "john@wick.com",
                        "read": true,
                        "table": "AdverseAction",
                        "user_role": null,
                        "created_at": "2020-06-26T13:44:02.000Z",
                        "updated_at": "2020-06-29T13:29:59.000Z"
                    }
                ],
                "cic_criminal_records": [],
                "sjv_criminal_reports": []
            },
            {
                "id": "99c97685-1653-4245-96a7-1b20d18e62d2",
                "check_id": "5844aec7-534b-4249-8a09-789cd76efeb2",
                "status": "PENDING",
                "type": "EMPLOYMENT",
                "cleared": null,
                "comments": null,
                "created_at": "2020-04-29T11:29:02.000Z",
                "updated_at": "2020-04-29T11:29:02.000Z",
                "deleted_at": null,
                "education": null,
                "employment": {
                    "id": "5844aec7-534b-4249-8a09-789cd76efeb2",
                    "email": "johndoe@gmail.com",
                    "access_id": null,
                    "employer_name": "HAweifg",
                    "employer_name_verified": null,
                    "employer_phone": null,
                    "employer_phone_verified": null,
                    "employer_address": "",
                    "employer_address_verified": null,
                    "employer_town": "Bengaluru",
                    "employer_town_verified": null,
                    "state": "KA",
                    "state_verified": null,
                    "zipcode": "560103",
                    "zipcode_verified": null,
                    "employer_country": "India",
                    "employer_country_verified": null,
                    "job_title": "Erew",
                    "job_title_verified": null,
                    "start_date": "01-01-2000",
                    "start_date_verified": null,
                    "end_date": "01-01-2010",
                    "end_date_verified": null,
                    "supervisor_name": "berge",
                    "supervisor_contact": null,
                    "current_employment": null,
                    "current_employment_verified": null,
                    "contract_type": null,
                    "contract_type_verified": null,
                    "source": null,
                    "created_at": "2020-04-29T10:34:19.000Z",
                    "updated_at": "2020-08-19T14:14:15.000Z",
                    "job_type": "full time employee",
                    "reason_for_leaving": null,
                    "status": 2,
                    "status_new": "FAILED",
                    "super_admin_status": 1,
                    "super_admin_status_new": "VERIFIED",
                    "consent": false,
                    "employee": {
                        "id": "dd924f25-ee91-4b94-af86-71451cec6695",
                        "access_id": "c6fcd2ff-d56f-4afd-b0bc-64039a00fc6b",
                        "email": "johndoe@gmail.com",
                        "password_hash": "100d5fcb45dc552d1a9011e2707b937904f79df410199fcb7a1e2b3c022d9911",
                        "first_name": "John",
                        "middle_name": "Mark",
                        "last_name": "Smith",
                        "name_verified": null,
                        "created_at": "2020-04-29T09:04:01.000Z",
                        "updated_at": "2020-05-14T14:23:08.000Z",
                        "employer_id": "1d4fb8ba-09ac-412c-aa62-58970b4d7472",
                        "payment_id": "5e758c74-4f5c-4846-9297-93ac85fab3d5",
                        "email_sent": true,
                        "payment": false,
                        "status": "VERIFIED",
                        "flow_completed": true,
                        "company_created_by": "john@wick.com",
                        "employee_limit_id": "8e4d574c-8ff9-472d-b432-9d9e1383abf8"
                    }
                },
                "read_statuses": [
                    {
                        "id": "adde9c54-3b73-4dd7-860c-823fc598c8a8",
                        "reference_id": "99c97685-1653-4245-96a7-1b20d18e62d2",
                        "user_email": "john@wick.com",
                        "read": true,
                        "table": "AdverseAction",
                        "user_role": null,
                        "created_at": "2020-04-29T11:29:02.000Z",
                        "updated_at": "2020-05-04T12:04:57.000Z"
                    }
                ],
                "cic_criminal_records": [],
                "sjv_criminal_reports": []
            }
        ],
        "count": 8
    }
}
```


This API is used to fetch Adverse actions. Possible status values - PENDING,NOTICE_SENT,IN_REVIEW,FINAL_CALL,CLOSED,IGNORED


### URL Parameters

Parameter | Type | Description
--------- | ------- | -----------
limit| integer | Raw File of the logo. 
offset| integer | Raw File of the logo. 
status | string | Current Status of the action. (PENDING, NOTICE_SENT, IN_REVIEW, FINAL_CALL, CLOSED)


## Adverse Action of Single Employee

```shell
curl --location --request GET 'https://api.us.springverify.com/company/action/adverse?adverse_action_id=2504d7c7-3f68-47a7-b9dc-be2adb99936e' \
--header 'Authorization: Bearer JWT_TOKEN' \
--header 'Content-Type: application/json'
```

> Success Response:

```json
{
    "success": true,
    "data": {
        "id": "2504d7c7-3f68-47a7-b9dc-be2adb99936e",
        "check_id": null,
        "status": "CLOSED",
        "type": "SEX_OFFENDER",
        "cleared": true,
        "comments": null,
        "created_at": "2020-08-25T08:17:24.000Z",
        "updated_at": "2020-08-26T12:00:11.000Z",
        "deleted_at": null,
        "education": null,
        "employment": null,
        "adverse_action_ledgers": [
            {
                "id": "21484067-1194-4495-a2f2-e80d54aff4be",
                "reference_id": "2504d7c7-3f68-47a7-b9dc-be2adb99936e",
                "status": "NOTICE_SENT",
                "comments": null,
                "cleared": null,
                "deleted_at": null,
                "created_at": "2020-08-26T11:58:27.000Z",
                "updated_at": "2020-08-26T11:58:27.000Z"
            },
            {
                "id": "622008d7-2553-4c32-bca0-3f5c833d2b66",
                "reference_id": "2504d7c7-3f68-47a7-b9dc-be2adb99936e",
                "status": "PENDING",
                "comments": null,
                "cleared": null,
                "deleted_at": null,
                "created_at": "2020-08-25T08:17:24.000Z",
                "updated_at": "2020-08-25T08:17:24.000Z"
            },
            {
                "id": "6d0a88fb-a297-47ca-a892-f06257fe2e48",
                "reference_id": "2504d7c7-3f68-47a7-b9dc-be2adb99936e",
                "status": "CLOSED",
                "comments": null,
                "cleared": true,
                "deleted_at": null,
                "created_at": "2020-08-26T12:00:11.000Z",
                "updated_at": "2020-08-26T12:00:11.000Z"
            }
        ],
        "adverse_action_comments": [],
        "cic_criminal_records": [
            {
                "id": "0548ea1c-b3de-4008-8147-1376c5870b8f",
                "employee_email_fk": "johndoe@gmail.com",
                "cic_criminal_report_id_fk": "62b71576-8456-42db-8ce9-4d12b2d864cb",
                "adverse_action_id_fk": "2504d7c7-3f68-47a7-b9dc-be2adb99936e",
                "record_type": "SEX_OFFENDER",
                "reviewed": false,
                "deleted_at": null,
                "created_at": "2020-08-13T14:24:42.000Z",
                "updated_at": "2020-08-25T08:17:24.000Z",
                "employee": {
                    "id": "298af70a-b3a3-4605-8789-37387bb276a1",
                    "access_id": "05065b6d-8272-4ba5-b31e-b9f576c0a44a",
                    "email": "johndoe@gmail.com",
                    "password_hash": "qwerty123",
                    "first_name": "John",
                    "middle_name": "Mark",
                    "last_name": "Smith",
                    "name_verified": null,
                    "created_at": "2020-08-13T13:59:22.000Z",
                    "updated_at": "2020-08-25T08:17:24.000Z",
                    "employer_id": "1d4fb8ba-09ac-412c-aa62-58970b4d7472",
                    "payment_id": "3aa91fd2-aca5-456b-a375-cf4df6ededed",
                    "email_sent": true,
                    "payment": false,
                    "status": "VERIFIED",
                    "flow_completed": true,
                    "company_created_by": "ajitesh.tiwari@springrole.com",
                    "employee_limit_id": "0e3ee39a-32c2-4eb6-a7f2-61a3507a26ba"
                },
                "record_data": {
                    "subject": {
                        "full_name": "JONATHAN DOE",
                        "dob": 19510000,
                        "image": "https://www.securecontinfo.com/user/images/sampleoffender.jpg",
                        "category": "SEX OFFENDER",
                        "source": "WI SEX OFFENDER REGISTRY",
                        "sex": "MALE",
                        "race": "BROWN",
                        "hair_color": "BROWN",
                        "eye_color": "BROWN",
                        "weight": "199 LBS",
                        "height": "5 FEET 10 INCHES",
                        "age": 64,
                        "case_number": 123456789,
                        "state": "CA",
                        "address": "123 CONSUMER RD, ca 95555",
                        "comments": "PHOTO DATE:  12/20/2010; CUSTODY/SUPERVISION:  TERMINATED; REGISTRATION END DATE:  LIFE REGISTRATION; COMPLIANCE STATUS:  COMPLIANT; PROGRAM INFORMATION:  DOC SORP ADMINISTRATION 3099 E WASHINGTON AVENUE Jane WI 53704 (608)240-5830;ID NUMBERS: DEPARTMENT OF CORRECTIONS NUMBER:00223018;  OFFENDER REGISTER DATE: 19901127",
                        "alias": "GREGORY CONSUMER"
                    },
                    "offenses": {
                        "offense": {
                            "description": "SEXUAL ASSAULT OF A CHILD",
                            "statute": "948.02(1)",
                            "conviction_date": "11/27/1990",
                            "conviction_location": "ROCK, WI"
                        }
                    }
                }
            },
            {
                "id": "63216b70-1c5e-4c46-bd5e-9d72ce7d78f5",
                "employee_email_fk": "johndoe@gmail.com",
                "cic_criminal_report_id_fk": "62b71576-8456-42db-8ce9-4d12b2d864cb",
                "adverse_action_id_fk": "2504d7c7-3f68-47a7-b9dc-be2adb99936e",
                "record_type": "SEX_OFFENDER",
                "reviewed": false,
                "deleted_at": null,
                "created_at": "2020-08-13T14:24:42.000Z",
                "updated_at": "2020-08-25T08:17:24.000Z",
                "employee": {
                    "id": "298af70a-b3a3-4605-8789-37387bb276a1",
                    "access_id": "05065b6d-8272-4ba5-b31e-b9f576c0a44a",
                    "email": "johndoe@gmail.com",
                    "password_hash": "qwerty123",
                    "first_name": "John",
                    "middle_name": "Mark",
                    "last_name": "Smith",
                    "name_verified": null,
                    "created_at": "2020-08-13T13:59:22.000Z",
                    "updated_at": "2020-08-25T08:17:24.000Z",
                    "employer_id": "1d4fb8ba-09ac-412c-aa62-58970b4d7472",
                    "payment_id": "3aa91fd2-aca5-456b-a375-cf4df6ededed",
                    "email_sent": true,
                    "payment": false,
                    "status": "VERIFIED",
                    "flow_completed": true,
                    "company_created_by": "ajitesh.tiwari@springrole.com",
                    "employee_limit_id": "0e3ee39a-32c2-4eb6-a7f2-61a3507a26ba"
                },
                "record_data": {
                    "subject": {
                        "full_name": "JONATHAN DOE",
                        "dob": "12/04/1951",
                        "category": "CRIMINAL",
                        "source": "FL DEPT OF CORRECTIONS- INMATE",
                        "sex": "MALE",
                        "race": "BLACK",
                        "hair_color": "BLACK",
                        "eye_color": "BROWN",
                        "weight": "240 LBS",
                        "height": "5 FEET 10 INCHES",
                        "age": 64,
                        "scars_marks": "GLASSES",
                        "case_number": 1550682,
                        "state": "FL",
                        "comments": "SENTENCE LENGTH: 13Y 0M 0D",
                        "alias": "JONATHAN CONSUMER,JONATHAN QUINCY CONSUMER,JOSEPH QUINCY CONSUMER"
                    },
                    "offenses": {
                        "offense": [
                            {
                                "description": "ROBB. GUN/DEADLY WPN(CONSPIRACY TO COMMIT)",
                                "disposition": "NOT PROVIDED BY SOURCE",
                                "disposition_date": "02-17-2012",
                                "offense_date": "07-26-2010",
                                "commitment_date": "03-12-2012",
                                "release_date": "07-26-2023"
                            },
                            {
                                "description": "2ND DEG.MURD,DANGEROUS ACT",
                                "disposition": "NOT PROVIDED BY SOURCE",
                                "disposition_date": "02-17-2012",
                                "offense_date": "07-26-2010",
                                "commitment_date": "03-12-2012",
                                "release_date": "07-26-2023"
                            },
                            {
                                "description": "ROBB. GUN/DEADLY WPN",
                                "disposition": "NOT PROVIDED BY SOURCE",
                                "disposition_date": "02-17-2012",
                                "offense_date": "07-26-2010",
                                "commitment_date": "03-12-2012",
                                "release_date": "07-26-2023"
                            }
                        ]
                    }
                }
            }
        ],
        "sjv_criminal_reports": []
    }
}
```

Retrive Adverse action for a single employee.

### URL Parameters

Parameter | Type | Description
--------- | ------- | -----------
adverse_action_id| uuid | ID of Adverse Action. 

## Mark Adverse Action as Read

```shell
curl --location --request PUT 'http://localhost:3080/company/action/adverse/mark-read' \
--header 'Authorization: Bearer JWT_TOKEN' \
--header 'Content-Type: application/json' \
--data-raw '{
    "IDs":["2504d7c7-3f68-47a7-b9dc-be2adb99936e"]
}'
```

> Success Response 

```json
{
    "success": true,
    "data": true
}
```

Mark adverse action as read for Company Admins.

### URL Parameters

Parameter | Type | Description
--------- | ------- | -----------
IDs | array | Array of IDs to be marked read.


## Clear or Send Adverse Action Notice 

```shell
curl --location --request PUT 'http://localhost:3080/company/action/adverse/pending' \
--header 'Authorization: Bearer JWT_TOKEN' \
--header 'Content-Type: application/json' \
--data-raw '{
    "adverse_action_id":"2504d7c7-3f68-47a7-b9dc-be2adb99936e",
    "status":"NOTICE_SENT"
}'
```


> Success Response

```json
{
    "success": true,
    "data": true
}
```

Set Adverse action status. This is the intial action in a particular adverse action lifecycle for the Admin.

### URL Parameters

Parameter | Type | Description
--------- | ------- | -----------
status | string | "NOTICE_SENT", "IGNORED"
adverse_action_id | string | Adverse Action ID on which status is being updated.


## Set Final Adverse Action Status

```shell
curl --location --request PUT 'http://localhost:3080/company/action/adverse/final' \
--header 'Authorization: Bearer JWT_TOKEN' \
--header 'Content-Type: application/json' \
--data-raw '{
    "adverse_action_id":"2504d7c7-3f68-47a7-b9dc-be2adb99936e",
    "status":"CLEAR"
}'
```


> Success Response

```json
{
    "success": true,
    "data": true
}
```

Set Final Adverse action status. This is the final action in a particular adverse action lifecycle for the Admin.

### URL Parameters

Parameter | Type | Description
--------- | ------- | -----------
status | string | "CLEAR", "REJECT"
adverse_action_id | string | Adverse Action ID on which status is being updated.

## Get Criminal Report


```shell
curl --location --request GET 'http://localhost:3080/company/criminal/sjv/report?sjv_id=0db2b92a-ec0d-4506-a356-fcb97f31e0c3' \
--header 'Authorization: Bearer JWT_TOKEN'
```

> Success Response

```json
{
    "message": "successfully retrieved sjv report",
    "data": {
        "glossary.title": "example glossary",
        "glossary.GlossDiv.title": "S",
        "glossary.GlossDiv.GlossList.GlossEntry.ID": "SGML",
        "glossary.GlossDiv.GlossList.GlossEntry.SortAs": "SGML",
        "glossary.GlossDiv.GlossList.GlossEntry.GlossTerm": "Standard Generalized Markup Language",
        "glossary.GlossDiv.GlossList.GlossEntry.Acronym": "SGML",
        "glossary.GlossDiv.GlossList.GlossEntry.Abbrev": "ISO 8879:1986",
        "glossary.GlossDiv.GlossList.GlossEntry.GlossDef.para": "A meta-markup language, used to create markup languages such as DocBook.",
        "glossary.GlossDiv.GlossList.GlossEntry.GlossDef.GlossSeeAlso.0": "GML",
        "glossary.GlossDiv.GlossList.GlossEntry.GlossDef.GlossSeeAlso.1": "XML",
        "glossary.GlossDiv.GlossList.GlossEntry.GlossSee": "markup"
    }
}
```

Get criminal Report using SJV ID in the adverse action object. This is a sample JSON response.

### URL Parameters

Parameter | Type | Description
--------- | ------- | -----------
sjv_id | string | SJV ID of crminal report.


# User API Flow

## Submit Personal Details

```shell
curl --location --request POST 'https://api.us.springverify.com/employee/PersonalDetails' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer JWT_TOKEN' \
--header 'Content-Type: text/plain' \
--data-raw '{
	"firstName":"John",
	"middleName":"David",
	"lastName":"Doe",
	"dob":"12-11-1980",
	"ssn":"123456789",
	"email":"johndoe@gmail.com",
	"address":"Address Line",
	"city":"Gotham",
	"state":"CA",
	"zipCode":"33433",
	"phone":"+56-999222992"
}'
```


> Success Response

```json
{
  "success":true,
  "successMsg":"personal details set successfully",
  "data":{}
}
```

This API records the information of the USER whose details will be verified. It is utmost essential that the information provided is absolutely accurate. The email provided here should be same as the one on which verification request was received. 

### URL Parameters

Parameter | Type | Description
--------- | ------- | -----------
firstName|string|First name if the employee.
middleName|string|Middle name if the employee.
lastName|string|Last name if the employee.
dob|string|Date of birth of the employee in DD-MM-YYYY.
ssn|string|SSN of the employee.
email|string|email of the employee.
address|string|Address of the employee.
city|string|City of the employee.
state|string|State if the employee.
zipCode|string|zipCode if the employee.
phone|string|Phone if the employee.


## Provide Consent

```shell
curl --location --request POST 'https://api.us.springverify.com/employee/consentupdate' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer JWT_TOKEN' \
--header 'Content-Type: text/plain' \
--data-raw '{
	 "summoryOfRights":true,
     "backgroundCheck":true,
     "reportChecked":true,
     "fullName":"John James Doe"
}'
```

> Success Response

```json
{
    "success": true,
    "successMsg": "consent updated successfully",
    "data": {}
}
```

This API records the consent of the USER whose details will be verified. The name provided here should match the full name provided in the Submit Personal Details API.

### URL Parameters

Parameter | Type | Description
--------- | ------- | -----------
summoryOfRights|boolean|summoryOfRights flag can be true or false.
backgroundCheck|boolean|backgroundCheck flag can be true or false.
reportChecked|boolean|reportChecked flag can be true or false.
fullName|string|Full name if the employee.


## Knowledge Based Quiz

```shell
curl --location --request GET 'https://api.us.springverify.com/employee/kbaquestions?email=johndoe@gmail.com' \
--header 'Authorization: Bearer JWT_TOKEN'
```

> Success Response

```json
{
  "success": true,
  "successMsg": "kba questions retirved successfully",
  "data": {
    "data": {
      "IDMSessionId": "916bac98cc38defd",
      "IDMKBAResponse": {
        "KBAQuestion": [
            {
                "QuestionId": 1,
                "Question": "Which of the following addresses have you lived at?",
                "QuestionType": "StreetAddress",
                "Options": [
                    {
                        "id": 1,
                        "option": "1906 N MARIANNA AVE"
                    },
                    {
                        "id": 2,
                        "option": "465 BRICKELL AVE"
                    },
                    {
                        "id": 3,
                        "option": "882 N MONTANA ST"
                    },
                    {
                        "id": 4,
                        "option": "1619 E TYROL AVE"
                    },
                    {
                        "id": 5,
                        "option": "NONE OF THE ABOVE"
                    }
                ]
            },
            {
                "QuestionId": 2,
                "Question": "Which of the following streets have you lived in?",
                "QuestionType": "StreetName",
                "Options": [
                    {
                        "id": 1,
                        "option": "LINCOYA BAY DR"
                    },
                    {
                        "id": 2,
                        "option": "20TH ST"
                    },
                    {
                        "id": 3,
                        "option": "W JOHNNY LYTLE AVE"
                    },
                    {
                        "id": 4,
                        "option": "RUSHMORE"
                    },
                    {
                        "id": 5,
                        "option": "NONE OF THE ABOVE"
                    }
                ]
            },
            {
                "QuestionId": 3,
                "Question": "Which of these cities are you associated with?",
                "QuestionType": "Cities",
                "Options": [
                    {
                        "id": 1,
                        "option": ":LONDON"
                    },
                    {
                        "id": 2,
                        "option": "NEW YORK"
                    },
                    {
                        "id": 3,
                        "option": "SANTA MONICA"
                    },
                    {
                        "id": 4,
                        "option": "COLORADO"
                    },
                    {
                        "id": 5,
                        "option": "NONE OF THE ABOVE"
                    }
                ]
            },
            {
                "QuestionId": 4,
                "Question": "Which of these phone numbers have you ever used previously?",
                "QuestionType": "PhoneNumbers",
                "Options": [
                    {
                        "id": 1,
                        "option": "(917) 914-9870"
                    },
                    {
                        "id": 2,
                        "option": "(234) 310-8490"
                    },
                    {
                        "id": 3,
                        "option": "(843) 514-3543"
                    },
                    {
                        "id": 4,
                        "option": "(865) 981-0265"
                    },
                    {
                        "id": 5,
                        "option": "NONE OF THE ABOVE"
                    }
                ]
            },
            {
                "QuestionId": 5,
                "Question": "Which of these businesses are you associated with?",
                "QuestionType": "Businesses",
                "Options": [
                    {
                        "id": 1,
                        "option": "DELL INC"
                    },
                    {
                        "id": 2,
                        "option": "MARIE D APPLYRS"
                    },
                    {
                        "id": 3,
                        "option": "AMAZON INC"
                    },
                    {
                        "id": 4,
                        "option": "ACME INC"
                    },
                    {
                        "id": 5,
                        "option": "NONE OF THE ABOVE"
                    }
                ]
            }
        ],
        "KBAStatus": {
          "QuestionsAnswered": 0,
          "NoOfQuestions": 5
        },
        "IsKbaEnabled": "1"
      }
    },
    "attemptNumber": 1,
    "success": true
  }
}
```

This API is used to fetch a Knowledge based quiz. The users will give their answers to the multiple choice questions and this will score their results.

### URL Parameters

Parameter | Type | Description
--------- | ------- | -----------
email|string|email of the employee.


## Submit KBA Quiz

```shell
curl --location --request POST 'https://api.us.springverify.com/employee/kbaverify' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer JWT_TOKEN' \
--header 'Content-Type: text/plain' \
--data-raw '{
"qna":[
    {
      "QuestionId": 1,
      "AnswerId": 5
    },{
      "QuestionId": 2,
      "AnswerId": 5
    },{
      "QuestionId": 3,
      "AnswerId": 5
    },{
      "QuestionId": 4,
      "AnswerId": 5
    },{
      "QuestionId": 5,
      "AnswerId": 5
    }
  ]
}'
```

> Success Response

```json
{
  "success": true,
  "successMsg": "kba result was derived",
  "data": {
    "correct_answers": 5,
    "total_questions": 5,
    "success": true
  }
}
```

> Error Response

```json
{
  "error": true,
  "errorCode": "02",
  "errorMsg": "Your answers didn't seem to be correct, please try uploading IDs."
}
```

This API is used to submit the answers to the Knowledge based quiz.


### URL Parameters

Parameter | Type | Description
--------- | ------- | -----------
qna|array|Contains the array of questions and their respective answers.


## Upload and Verify ID

```shell
curl --location --request POST 'https://api.us.springverify.com/employee/uploadreviewid' \
--header 'Authorization: Bearer JWT_TOKEN' \
--form 'frontImage=@/path/to/file' \
--form 'backImage=@/path/to/file' \
--form 'type=driving'
```

> Success Response

```json
{
  "success": true,
  "successMsg": "driving license uploaded successfully",
  "data": {
    
  }
}
```

This API records the ID of the user and verifies it authenticity.

**Important Notes:**

1. The maximum allowed size of the image uploaded (per image) is capped at 2MB. Please do not send in photos larger than 2MB as an exception will be thrown.
2. Capturing a clear, non-blurry image with the mobile device is extremely important so that the ID can be processed accurately.
3. Make sure to use a minimum 5 Megapixel camera to capture the images for UploadID.
4. Make sure that there is a contrast between the ID and the background, and the background is a solid colored background.
5. Make sure that there is a contrast between the ID and the background, and the background is a solid colored background.
6. Make sure all the edges of the ID are visible and no edge is getting cutoff.



### URL Parameters

Parameter | Type | Description
--------- | ------- | -----------
frontImage|image|Front side of the ID.
backImage|image|Back side of the ID.
type|string|type of the ID submitted, currently we support Driving License, which has to be passed as `driving`.



## Upload and Verify Passport

```shell
curl --location --request POST 'https://api.us.springverify.com/employee/uploadpassport' \
--header 'Authorization: Bearer JWT_TOKEN' \
--form 'front=@/path/to/file'
```

> Success Response

```json
{
  "success": true,
  "successMsg": "passport uploaded successfully",
  "data": {
    
  }
}
```

> Error Response

```json
{
  "error": true,
  "errorCode": "02",
  "errorMsg": "The Passport could not be parsed. Please make sure all four sides are visible and the Passport is in front of a dark background."
}
```

This API records the Passport of the user and verifies it authenticity.

This call is used to upload an image of a passport to be scanned and parsed. The image should be of the page with the user’s information on it (generally on the first or second page). It is one picture that captures both pages of the passport. This follows the same logic as the other uploadId endpoints in that the picture needs to be clear, it needs to have minimal glare, and it must have sufficient lighting.

There are several variables that are more likely to cause a document to fail:

1. Image capture quality (for example blurriness or reflection)
2. Personalization of the document (such as especially long names)
3. Variations in manufacturing techniques (for example card printed on wrong side or slight variations in printing location)
4. Wear and aging of the document (a worn or dirty card can cause failure) Tampering and counterfeiting (unlawful changes or reproduction of documents)


### URL Parameters

Parameter | Type | Description
--------- | ------- | -----------
front|image|Front side of the Passport.


## Request Manual Review of the ID or Passport

```shell
curl --location --request GET 'https://api.us.springverify.com/employee/putidmanualreview' \
--header 'Authorization: Bearer JWT_TOKEN'
```
<!--- 
Success Response

```json
```

> Error Response

```json
```

 -->

In case both Upload ID and Upload Passport fail twice or less, the ID can be requested for a manual review, which our Ops Team will look into.


## Get ID verification tries

```shell
curl --location --request GET 'https://api.us.springverify.com/employee/idtriescount' \
--header 'Authorization: Bearer JWT_TOKEN'
```

> Success Response

```json
{
  "success": true,
  "successMsg": "no of id checks number retrieved successfully",
  "data": {
    "DL": 0,
    "PASSPORT": 0,
    "KBA": 1
  }
}
```

This API will give you the number of Driving License ID , Passport and KBA tries an employee has made. The maximum allowed limit is 2 per method per candidate.   


## Get Candidate Info

```shell
curl --location --request GET 'https://api.us.springverify.com/employee/' \
--header 'Authorization: Bearer JWT_TOKEN'
```

> Success Response

```json
{
    "success": true,
    "successMsg": "packages retrived successfully",
    "data": {
        "employee": {
            "id": "5d9f192f-446d-4576-926c-289863d6dbf6",
            "access_id": "ca4be4ab-c256-4377-a9fa-9e60538ae6d3",
            "email": "fuspuzumlu@desoz.com",
            "first_name": "abc",
            "middle_name": "",
            "last_name": "xyz",
            "name_verified": null,
            "created_at": "2019-07-25T13:15:19.000Z",
            "updated_at": "2020-03-30T14:32:24.000Z",
            "employer_id": "1d4fb8ba-09ac-412c-aa62-58970b4d7472",
            "payment_id": "fff773ce-7c8c-48b1-8476-bceb88f5dd62",
            "email_sent": true,
            "payment": false,
            "status": "VERIFIED",
            "flow_completed": true,
            "CompanyCreatedBy": "johndoe@domain.com",
            "EmployeeLimitId": "574077a2-1800-4e5e-bec8-cb51adf42848",
            "kbaqna": {
                "id": "d7131f0a-e716-41d4-bbcd-fd8a419bd938",
                "access_id": "ca4be4ab-c256-4377-a9fa-9e60538ae6d3",
                "email": "fuspuzumlu@desoz.com",
                "questions_flag": 1,
                "kba_enabled": 1,
                "question_count": "4",
                "correct_answers": "1",
                "verified": false,
                "created_at": "2019-07-25T13:21:00.000Z",
                "updated_at": "2019-08-05T08:35:33.000Z",
                "IDMSessionId": "64ee9ce28fd84f43",
                "EmployeeEmail": "fuspuzumlu@desoz.com"
            },
            "Employements": [
                {
                    "id": "3cefb130-0d34-4859-ad95-ae5f7c42bcc7",
                    "email": "fuspuzumlu@desoz.com",
                    "access_id": null,
                    "employer_name": "SpringRole",
                    "employer_name_verified": 0,
                    "employer_phone": "1-",
                    "employer_phone_verified": null,
                    "employer_address": "Hosur Road",
                    "employer_address_verified": 0,
                    "employer_town": "Bengaluru",
                    "employer_town_verified": 0,
                    "state": "KA",
                    "state_verified": null,
                    "zipcode": "560029",
                    "zipcode_verified": null,
                    "employer_country": "India",
                    "employer_country_verified": 0,
                    "job_title": "Job",
                    "job_title_verified": 0,
                    "start_date": "01-01-2000",
                    "start_date_verified": 0,
                    "end_date": "01-01-2000",
                    "end_date_verified": 0,
                    "supervisor_name": "Name",
                    "supervisor_contact": null,
                    "current_employment": null,
                    "current_employment_verified": 0,
                    "contract_type": null,
                    "contract_type_verified": "0",
                    "source": null,
                    "created_at": "2019-08-01T12:40:47.000Z",
                    "updated_at": "2019-09-11T11:56:27.000Z",
                    "job_type": "full time employee",
                    "reason_for_leaving": null,
                    "status": 2,
                    "super_admin_status": 1,
                    "consent": null
                }
            ],
            "Education": [
                {
                    "id": "a3cb1983-e69f-4053-a494-3fce7323bf66",
                    "email": "fuspuzumlu@desoz.com",
                    "access_id": null,
                    "employee_alias_name": null,
                    "employee_alias_name_verified": null,
                    "school_name": "CMS",
                    "school_name_verified": null,
                    "school_campus": "CMS",
                    "school_campus_verified": null,
                    "phone": null,
                    "phone_verified": null,
                    "address": null,
                    "address_verified": null,
                    "town": null,
                    "town_verified": null,
                    "city": "Lucknow",
                    "city_verified": null,
                    "state": "UP",
                    "state_verified": null,
                    "zipcode": null,
                    "zipcode_verified": null,
                    "country": "India",
                    "country_verified": null,
                    "start_date": "01-01-2000",
                    "start_date_verified": null,
                    "end_date": "01-01-2000",
                    "end_date_verified": null,
                    "degree": "Degree",
                    "degree_verified": null,
                    "major": null,
                    "major_verified": null,
                    "school_type": "University",
                    "source": null,
                    "created_at": "2019-08-01T13:20:23.000Z",
                    "updated_at": "2020-03-30T14:32:22.000Z",
                    "currently_attending": 0,
                    "completed_successfully": 1,
                    "status": 2,
                    "super_admin_status": 1
                }
            ],
            "EmployeeLimit": {
                "id": "574077a2-1800-4e5e-bec8-cb51adf42848",
                "employement": 2,
                "education": 1,
                "professional_license": 1,
                "civil_court": 0,
                "driving_license": 0,
                "package_id": null,
                "created_at": "2019-07-25T13:15:19.000Z",
                "updated_at": "2019-07-25T13:15:19.000Z",
                "EmployeeInviteGroupId": "6d3eae64-32b8-42d4-9f87-86b746afabd2",
                "EmployeeInviteGroup": {
                    "id": "6d3eae64-32b8-42d4-9f87-86b746afabd2",
                    "package": "gold",
                    "active": false,
                    "created_at": "2019-07-25T13:15:17.000Z",
                    "updated_at": "2019-07-26T12:45:14.000Z",
                    "CompanyCreatedBy": "johndoe@domain.com",
                    "PackageId": "2"
                }
            },
            "EmployeeDetail": {
                "id": "29e8611d-1072-447f-914b-c38c19648138",
                "access_id": "ca4be4ab-c256-4377-a9fa-9e60538ae6d3",
                "address": "17 2nd Street, Suite 201",
                "address_verified": null,
                "driving_number": null,
                "driving_number_verified": null,
                "city": " Santa Monica,",
                "city_verified": null,
                "state": "CA",
                "state_verified": null,
                "zipcode": "90401",
                "zipcode_verified": null,
                "country": null,
                "country_verified": null,
                "birthdate": "29-05-1994",
                "birthdate_verified": null,
                "phone": "1-9179139020",
                "phone_verified": null,
                "ssn": "3485",
                "ssn_verified": null,
                "created_at": "2019-07-25T13:20:20.000Z",
                "updated_at": "2019-07-25T13:20:20.000Z",
                "EmployeeEmail": "fuspuzumlu@desoz.com"
            },
            "EmployeeVerification": {
                "id": "0a392e86-6973-46d9-a6a1-a7124c5efce1",
                "s3_gov_id": "link",
                "s3_gov_id_back": null,
                "s3_gov_id_match": false,
                "s3_web_img": null,
                "s3_passport_verified": 1,
                "s3_dl_verified": null,
                "verification_type": "id",
                "address": null,
                "address_verified": null,
                "city": null,
                "city_verified": null,
                "state": null,
                "state_verified": null,
                "zipcode": null,
                "zipcode_verified": null,
                "country": null,
                "country_verified": null,
                "birthdate": null,
                "birthdate_verified": null,
                "criminal_verified": null,
                "created_at": "2019-07-25T13:20:55.000Z",
                "updated_at": "2019-08-01T10:06:48.000Z",
                "is_report_checked": true,
                "summary_of_rights_accepted": true,
                "background_check_disclosure_accepted": true,
                "id_manual_review": null,
                "super_admin_status": null,
                "EmployeeEmail": "fuspuzumlu@desoz.com"
            }
        },
        "review": []
    }
}
```

This API returns all info pertaining to a candidate including all types of verification.

## Add Candidate Employment

```shell
curl --location --request POST 'https://api.us.springverify.com/employee/employement' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer JWT_TOKEN' \
--header 'Content-Type: text/plain' \
--data-raw '{	
	"employerName":"Stark Industries",
	"employerAddress":"12, Manhattan Street",
	"employerPhone":"9911991199",
	"employerTown":"New York",
	"state":"New York",
	"zipCode":"129012",
	"country":"USA",
	"jobTitle":"Senior Manager",
	"startDate":"19-11-2000",
	"endDate":"19-11-2002",
	"supervisorName": "Nick Fury",
	"currentEmployement": "0",
	"jobType": "Contract",
	"reasonForLeaving": "",
	"supervisorContact": "nickfury@starkindustries.com"
}'
```

<!---

 Success Response

```json
```

> Error Response

```json
```
 -->
 
This API will be used to submit the Employment records for the Employee.

### URL Parameters

Parameter | Type | Description
--------- | ------- | -----------
employerName|string|Name of the employer.
employerAddress|string|Address of the employer.
employerPhone|string|Phone number of the employer.
employerTown|string|Town of the employer.
state|string|State of the employer.
zipCode|string|Zip Code of the employer.
country|string|Country of the employer.
jobTitle|string|Job Title (latest one if multiple)
startDate|string|Start Date of the Job.
endDate|string|End Date of the Job.
supervisorName|string|Supervisor Name 
currentEmployement|string| Is this Employees current employment. 
jobType|string|Job Type (Contract/Employment)
reasonForLeaving|string| Reason for leaving the job (optional)
supervisorContact|string| Active contact of the supervisor

## Delete Employment

```shell
curl --location --request GET 'https://api.us.springverify.com/employee/deleteeducation?id=74191de6-2d48-4de4-8191-4472ec8b4c6a' \
--header 'Authorization: Bearer JWT_TOKEN'
```

<!---

 Success Response

```json
```

> Error Response

```json
```
 -->

This API is used to delete the employment submitted in the previous API before it goes for verification.

### URL Parameters

Parameter | Type | Description
--------- | ------- | -----------
id|UUID|UUID of the employment record.


## Edit Employment

```shell
curl --location --request POST 'https://api.us.springverify.com/employee/employement' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer JWT_TOKEN' \
--header 'Content-Type: text/plain' \
--data-raw '{	
        "id":"74191de6-2d48-4de4-8191-4472ec8b4c6a",
	"employerName":"Stark Industries",
	"employerAddress":"12, Manhattan Street",
	"employerPhone":"9911991199",
	"employerTown":"New York",
	"state":"New York",
	"zipCode":"129012",
	"country":"USA",
	"jobTitle":"Senior Manager",
	"startDate":"19-11-2000",
	"endDate":"19-11-2002",
	"supervisorName": "Nick Fury",
	"currentEmployement": "0",
	"jobType": "Contract",
	"reasonForLeaving": "",
	"supervisorContact": "nickfury@starkindustries.com"
}
```

<!---

 Success Response

```json
```

> Error Response

```json
```
 -->

This call is similar to add employment, if ID recieved in the Submit Employment response is added, it can be used to edit the employment data.

### URL Parameters

Parameter | Type | Description
--------- | ------- | -----------
id|UUID|UUID of the employment record.
employerName|string|Name of the employer.
employerAddress|string|Address of the employer.
employerPhone|string|Phone number of the employer.
employerTown|string|Town of the employer.
state|string|State of the employer.
zipCode|string|Zip Code of the employer.
country|string|Country of the employer.
jobTitle|string|Job Title (latest one if multiple)
startDate|string|Start Date of the Job.
endDate|string|End Date of the Job.
supervisorName|string|Supervisor Name 
currentEmployement|string| Is this Employees current employment. 
jobType|string|Job Type (Contract/Employment)
reasonForLeaving|string| Reason for leaving the job (optional)
supervisorContact|string| Active contact of the supervisor


## Trigger Employment Verification

```shell
curl --location --request GET 'https://api.us.springverify.com/employee/submitEmp' \
--header 'Authorization: Bearer JWT_TOKEN'
```

<!---

 Success Response

```json
```

> Error Response

```json
```
 -->

Once the employment records have been submitted and finalized, Employment Verification can be triggered using this API.


## Add Employee's Education 

```shell
curl --location --request POST 'https://api.us.springverify.com/employee/education' \
--header 'Content-Type: application/json' \
--data-raw '{
    "schoolName": "MIT",
    "schoolType": "University",
    "schoolCampus": "Boston",
    "city":"Boston",
    "state":"Massachusetts",
    "country":"USA",
    "startDate":"28-12-1991",
    "endDate":"12-28-1996", 
    "degree":"Engineering",
    "currentlyAttending":"0", 
    "completedSuccessfully":"1",
    "major":"Biotech"
}'
```

<!---

Success Response

```json
```

> Error Response

```json
```
 -->

This API will be used to submit the education records for the Employee.

### URL Parameters

Parameter | Type | Description
--------- | ------- | -----------
schoolName|string|School Name of the Employee.
schoolType|string|School Type of the Employee.
schoolCampus|string|School Campus of the Employee.
city|string|City in which the school is based.
state|string|State in which the school is based.
country|string|Country in which the school is based.
startDate|string|Start date of the course.
endDate|string|End date of the course.
degree|string|Official degree of the course. 
currentlyAttending|string| If Currently Attending 1, otherwise 0
completedSuccessfully|string| If Completed Successfully 1, otherwise 0
major|string|major if any

## Edit Education Entry

```shell
curl --location --request POST 'https://api.us.springverify.com/employee/education' \
--header 'Content-Type: application/json' \
--data-raw '{
    "id":"74191de6-2d48-4de4-8191-4472ec8b4c6a",
    "schoolName": "MIT",
    "schoolType": "University",
    "schoolCampus": "Boston",
    "city":"Boston",
    "state":"Massachusetts",
    "country":"USA",
    "startDate":"28-12-1991",
    "endDate":"12-28-1996", 
    "degree":"Engineering",
    "currentlyAttending":"0", 
    "completedSuccessfully":"1",
    "major":"Biotech"
}'
```

<!---

Success Response

```json
```

> Error Response

```json
```
 -->

This API will be used to edit the education records for the Employee. The id parameter passed will be the same as received at the time of Education Entry submission.

### URL Parameters

Parameter | Type | Description
--------- | ------- | -----------
id| UUID | UUID of the education entry.
schoolName|string|School Name of the Employee.
schoolType|string|School Type of the Employee.
schoolCampus|string|School Campus of the Employee.
city|string|City in which the school is based.
state|string|State in which the school is based.
country|string|Country in which the school is based.
startDate|string|Start date of the course.
endDate|string|End date of the course.
degree|string|Official degree of the course. 
currentlyAttending|string| If Currently Attending 1, otherwise 0
completedSuccessfully|string| If Completed Successfully 1, otherwise 0
major|string| Course Major if any

## Delete Education

```shell
curl --location --request GET 'https://api.us.springverify.com/employee/deleteeducation?id=74191de6-2d48-4de4-8191-4472ec8b4c6a' \
--header 'Authorization: Bearer JWT_TOKEN'
```

<!---

Success Response

```json
```

> Error Response

```json
```
 -->

This API is used to delete the education entry submitted in the previous API before it goes for verification.

### URL Parameters

Parameter | Type | Description
--------- | ------- | -----------
id| UUID | UUID of the education entry.


## Trigger Education Verification

```shell
curl --location --request GET 'https://api.us.springverify.com/employee/submitEdu' \
--header 'Authorization: Bearer JWT_TOKEN'
```

<!---

Success Response

```json
```

> Error Response

```json
```
 -->

Once the education records have been submitted and finalized, Education Verification can be triggered using this API.


## Submit Professional License

```shell
curl --location --request POST 'https://api.us.springverify.com/employee/license' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer JWT_TOKEN' \
--header 'Content-Type: text/plain' \
--data-raw '{
    "licenseTitle":"Falcon 1x",
    "licenseNumber":"TS101",
    "startDate":"09-09-2014",
    "endDate":"10-31-2019",
    "state":"CA",
    "country":"USA",
    "licenseOrganization":"Stark Industries"
}'
```

<!---

Success Response

```json
```

> Error Response

```json
```
 -->
 
This API will be used to submit the Professional License records for the Employee.

### URL Parameters

Parameter | Type | Description
--------- | ------- | -----------
licenseTitle|string|Name of the License.
licenseNumber|string|Serial Number of the License.
startDate|string| Valid-From date of the License.
endDate|string|Valid-To date of the License.
state|string|State in which the License is valid.
country|string|Country in which the License is valid.
licenseOrganization|string| Issuing organization of the license. 

## Delete License

```shell
curl --location --request GET 'https://api.us.springverify.com/employee/deletelicense?id=441f71eb-b79a-4b27-8835-7e7dcd9d53a6' \
--header 'Authorization: Bearer JWT_TOKEN'
```

<!---

Success Response

```json
```

> Error Response

```json
```
 -->

This API will be used to delete a single Professional License record for the Employee.

### URL Parameters

Parameter | Type | Description
--------- | ------- | -----------
id|UUID|UUID of the Professional License Record.

## Trigger Professional License Verification 

```shell
curl --location --request GET 'https://api.us.springverify.com/employee/submitLic' \
--header 'Authorization: Bearer JWT_TOKEN'
```

<!---
Success Response

```json
```

> Error Response

```json
```
 -->

Once the professional license records have been submitted and finalized, professional license verification can be triggered using this API.

## Create Password

```shell
curl --location --request POST 'https://api.us.springverify.com/employee/createPassword' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer JWT_TOKEN' \
--header 'Content-Type: text/plain' \
--data-raw '{
	"passwordHash":"yahoo123456789"
}'
```

<!---

Success Response

```json
```

> Error Response

```json
```
 -->


After the forementioned checks have been successfully submitted and triggered. Employee can create a password for their profile.

<aside class="notice">
    Password field should be hashed using SHA256 before hand. 
</aside>


### URL Parameters

Parameter | Type | Description
--------- | ------- | -----------
passwordHash| string | Hash of the password.

## Reset Password

```shell
curl --location --request POST 'https://api.us.springverify.com/employee/resetPassword' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer JWT_TOKEN' \
--header 'Content-Type: text/plain' \
--data-raw '{
	"oldPasswordHash":"100d5fcb45dc552d1a9011e2707b937904f79df410199fcb7a1e2b3c022d9911",
	"newPasswordHash":"1ajnfas184918491813nasfjh2481879asdajsdajsdjasdjs091301903910391"
}'
```

<aside class="notice">
    `oldPasswordHash` and `newPasswordHash` fields should be hashed using SHA256 before hand. 
</aside>

<!---

Success Response

```json
```

> Error Response

```json
```
 -->
 

### URL Parameters

Parameter | Type | Description
--------- | ------- | -----------
oldPasswordHash| string | Hash of the Old password.
newPasswordHash| string | Hash of the New password.


# Previous Versions

These docs are pertaining to the current version (v2). If you would like to view older docs to reference previous versions, the links are below:

1) [Version 1 Docs](https://https://docs.us.springverify.com/olddocs/v1/index.html) - End of Life on 2020-09-14.

