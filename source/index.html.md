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
curl --location --request POST 'https://api.us.springverify.com/signup' \
        --header 'Content-Type: application/json' \
        --data-raw '{
            "email": "johndoe@domianname.com",
            "password": "daaad6e5604e8e17bd9f108d91e26afe6281dac8fda0091040a7a6d7bd9b43b5",
            "confirmPassword": "daaad6e5604e8e17bd9f108d91e26afe6281dac8fda0091040a7a6d7bd9b43b5",
            "firstName": "John",
            "lastName": "Doe",
            "phone": "123456789",
            "domain": "domianname.com"
        }'
```

> Success Response

```json
{
        "message": "Account created successfully."
}
```

> Error Response

```json
{
    "error": true,
    "errorMsg": "wrong email"
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
confirmPassword|string|Password confirmation.
firstName|string|First name of the Admin.
lastName|string|Last name of the Admin.
phone|string|Phone number of the user.
domain|string|Domain of the company. Must be same as the email domain.

<aside class="success">
Congrats on getting started with SpringVerify!
</aside>

## Login

```shell
curl --location --request POST 'https://api.us.springverify.com/login' \
        --header 'Content-Type: application/json' \
        --data-raw '{
            "email": "johndoe@domain.com",
            "password": "daaad6e5604e8e17bd9f108d91e26afe6281dac8fda0091040a7a6d7bd9b43b5",
            "role":"admin"
        }'
```

> Success Response

```json
{
    "token": "JWT_TOKEN",
    "profile": {
        "firstName": "John",
        "lastName": "Doe",
        "email": "johndoe@domainname.com",
        "domain": "domainname.com",
        "company": {
            "id": 1,
            "name": "abcde",
            "address": "xyza",
            "city": "123",
            "state": "ME",
            "zipcode": "122330",
            "taxIdNumber": "12-3456789",
            "s3Logo": "logo_s3_link"
        }
    }
}
```

> Error Response:

```json
    {
        "errors": [
            {
                "location": "params",
                "param": "role",
                "msg": "role is missing"
            }
        ]
    }
```

Aim is to generate a JWT token , which will be used for all further API calls.

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

## Register Your Company

```shell
curl --location --request POST 'https://api.us.springverify.com/company' \
        --header 'Authorization: Bearer JWT_TOKEN' \
        --header 'Content-Type: application/json' \
        --data-raw '{
            "name": "Google",
            "address": "Address",
            "city": "reqbodycity",
            "state": "reqbodystate",
            "zipcode": "12345",
            "taxIdNumber": "reqbodytaxIdNumber"
        }'
```

> Success Response:

```json
{
    "message": "Company updated successfully"
}
```

This API is used to register a company. JWT token retrieved will be used for authentication.

### URL Parameters

Parameter | Type | Description
--------- | ------- | -----------
name|string|Name of the registered company.
address|string|Address of the company.
city|string|City of the company.
state|string|State of the registered company.
zipcode|string|Zipcode of the company.
taxIdNumber|string|Tax number of the company.


## Get Available Packages

```shell
curl --location --request GET 'http://api-stage.us.springverify.com/company/package' \
        --header 'Authorization: Bearer JWT_TOKEN'
```

> Success Response:

```json
{
    "success": true,
    "successMsg": "actions retrived successfuly",
    "data": {
        "package": [
            {
                "id": "1",
                "name": "bronze",
                "employement": "0",
                "education": "0",
                "professional_license": "0",
                "civil_court": "0",
                "driving_license": "0",
                "price": "750",
                "created_at": "2019-07-03T11:07:27.000Z",
                "updated_at": "2019-11-11T11:53:38.000Z"
            },
            {
                "id": "2",
                "name": "silver",
                "employement": "1",
                "education": "0",
                "professional_license": "0",
                "civil_court": "0",
                "driving_license": "0",
                "price": "1500",
                "created_at": "2019-07-03T11:07:27.000Z",
                "updated_at": "2019-11-11T11:53:38.000Z"
            },
            {
                "id": "3",
                "name": "gold",
                "employement": "2",
                "education": "0",
                "professional_license": "0",
                "civil_court": "0",
                "driving_license": "0",
                "price": "2000",
                "created_at": "2019-07-03T11:07:27.000Z",
                "updated_at": "2019-11-11T11:53:39.000Z"
            },
            {
                "id": "4",
                "name": "platinum",
                "employement": "2",
                "education": "1",
                "professional_license": "0",
                "civil_court": "0",
                "driving_license": "0",
                "price": "3000",
                "created_at": "2019-07-03T11:07:27.000Z",
                "updated_at": "2019-11-11T11:53:39.000Z"
            },
            {
                "id": "5",
                "name": "diamond",
                "employement": "2",
                "education": "1",
                "professional_license": "0",
                "civil_court": "1",
                "driving_license": "0",
                "price": "4000",
                "created_at": "2019-07-03T11:07:27.000Z",
                "updated_at": "2019-11-11T11:53:39.000Z"
            }
        ],
        "prices": {
            "id": "1",
            "env": "development",
            "PASSPORT": 200,
            "DRIVING_LICENSE": 200,
            "ENHANCED_DRIVING_LICENSE": 200,
            "KBA": 400,
            "EMPLOYEMENT": 750,
            "EDUCATION": 750,
            "PROFESSIONAL_LICENSE": 500,
            "CIVIL_COURT": 1000,
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
curl --location --request POST 'https://api.us.springverify.com/employee/invite' \
--header 'Content-Type: application/json' \
--data-raw '{
    "package": "bronze",
    "emailList": ["johndoe@dmail.com"],
    "addOns": {
        "employement": 0,
        "education": 0,
        "license": 0,
        "drivingLicense": 0,
        "civilCourt": 0,
        "county_criminal_search": "0",
        "all_county_criminal_search": false
    },
    "couponCode":""
}'

curl --location --request POST 'https://api.us.springverify.com/employee/invite' \
--header 'Content-Type: application/json' \
--data-raw '{
    "emailList":["johndoe@dmail.com"],
    "package":"silver",
    "addOns": {
        "employement": 1,
        "education": 0,
        "license": 0,
        "drivingLicense": 0,
        "civilCourt": 0,
        "county_criminal_search": "0",
        "all_county_criminal_search": false
    },
    "couponCode":""
}'


curl --location --request POST 'https://api.us.springverify.com/employee/invite' \
--header 'Content-Type: application/json' \
--data-raw '{
    "emailList":["johndoe@dmail.com"],
    "package":"gold",
    "addOns": {
        "employement": 2,
        "education": 0,
        "license": 0,
        "drivingLicense": 0,
        "civilCourt": 0,
        "county_criminal_search": "1",
        "all_county_criminal_search": false
    }
    "couponCode":""
}'

curl --location --request POST 'https://api.us.springverify.com/employee/invite' \
--header 'Content-Type: application/json' \
--data-raw '{
    "emailList":["johndoe@dmail.com"],
    "package":"platinum",
    "addOns": {
        "employement": 2,
        "education": 1,
        "license": 0,
        "drivingLicense": 0,
        "civilCourt": 0,
        "county_criminal_search": "0",
        "all_county_criminal_search": true
    }
    "couponCode":""
}'


curl --location --request POST 'https://api.us.springverify.com/employee/invite' \
--header 'Content-Type: application/json' \
--data-raw '{
    "emailList":["johndoe@dmail.com"],
    "package":"diamond",
    "addOns": {
        "employement": 2,
        "education": 1,
        "license": 0,
        "drivingLicense": 0,
        "civilCourt": 1,
        "county_criminal_search": "0",
        "all_county_criminal_search": true
    }
    "couponCode":""
}'
```

>Success Response:

```json
{
    "success": true,
    "successMsg": "invites sent successfuly",
    "data": {
        "price": 3950,
        "id": "d78a273d-b045-4dc7-8325-8fa7b585ff81",
        "count": 1
    }
}
```

AIM of this API is used to invite employee/employees. It can be used to invite employee in bulk. Once payment is done sucessfully then email is sent to the employee.

<aside class="notice">This API should only be used after getting the packages in previous API.</aside>

### URL Parameters

Parameter | Type | Description
--------- | ------- | -----------
emailList|array|Contains the list of employee emails.
package|string|Name of the package (retrieved in get packages api).
addOns|Object|Extra checks can be added in a package.
CouponCode|string|This coupon Code is to avail discounts.

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

This API is used to register your Card with Stripe. Once the API call is successful the response will incude an `id` parameter which will be used in the next API request for authorizing payments.

## Save the Payment Info

```shell
curl --location --request POST 'https://api.us.springverify.com/payment/savecard' \
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
    "successMsg": "card saved for the specified user",
    "data": {}
}
```

> Error Response

```json
{
    "error": true,
    "errorCode": "02",
    "errorMsg": "You cannot use a Stripe token more than once: tok_1EkXOa4wweuFtc0nQd0eOOhs."
}
```

## Payment For Invites

```shell
curl --location --request POST 'https://api.us.springverify.com/payment/chargeuser' \
        --header 'Content-Type: application/json' \
        --header 'Authorization: Bearer JWT_TOKEN' \
        --data-raw '{
                "id": "72586cfc-2b46-468a-8f80-af64de0106e0"
        }'
```

>Success Response

```json
{
        "success": true,
        "successMsg": "money added successfully",
        "data": {}
}
```

This API is called right after invite API. The reference id retrieved in previous api is used in this api (Invite Candidates). Once the payment is done successfully the email is sent out to the candidate.

### URL Parameters

Parameter | Type | Description
--------- | ------- | -----------
id|string|Reference id retrieved in invite employee API.


## Get Single Candidate

```shell
curl --location --request GET 'https://api.us.springverify.com/company/employee?id=
CANDIDATE_UUID' \
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
                "s3_gov_id": "https://spring-verify-us.s3.amazonaws.com/employee-id/fuspuzumlu%40desoz.com/passport.jpg",
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

AIM of this API is used to get employee details.

### URL Parameters

Parameter | Type | Description
--------- | ------- | -----------
employeeId|uuid|Contains the unique id of the employee.

<!---
************************************************************************
COMMENTED SINCE THIS PART IS NOT LIVE YET

## Reset Password

```shell
curl --location --request POST 'https://api.us.springverify.com/profile/resetpassword' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer JWT_TOKEN ' \
--header 'Content-Type: text/plain' \
--data-raw '{
	"password":"qwerty123"
}'
```

> Success Response

```json
```

> Error Response

```json
```

### URL Parameters

Parameter | Type | Description
--------- | ------- | -----------
password| string | Hash of the password.


## Forgot Password

```shell
curl --location --request POST 'https://api.us.springverify.com/forgotpassword' \
--header 'Content-Type: application/json' \
--header 'Content-Type: text/plain' \
--data-raw '{
	"email":"johndoe@domianname.com"
}'
```

> Success Response

```json
```

The forgot password API will trigger an email, with which password can be reset.

### URL Parameters

Parameter | Type | Description
--------- | ------- | -----------
email| string | Email with which account is registered.

************************************************************************
 -->



## Upload Company Logo

```shell
curl --location --request POST 'https://api.us.springverify.com/company/uploadlogo' \
--header 'Authorization: Bearer JWT_TOKEN' \
--form 'logo=@/path/to/file'
```

<!---

 Success Response

```json
```

> Error Response

```json
```
 -->

Set the company logo using this API

### URL Parameters

Parameter | Type | Description
--------- | ------- | -----------
logo| file | Raw File of the logo.

## Get Employees by Company

```shell
curl --location --request POST 'https://api.us.springverify.com/company/employees' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer JWT_TOKEN' \
--header 'Content-Type: text/plain' \
--data-raw '{
	"limit":1,
	"offset":0,
    "filter":'ALL'
}'
```

> Success Response

```json
{
  "success": true,
  "successMsg": "actions retrived successfuly",
  "data": {
    "employees": [
      {
        "id": "1b852036-1d76-4adc-af88-b40693b386d9",
        "email": "john@email.com",
        "first_name": "John",
        "middle_name": "James",
        "last_name": "Doe",
        "status": null,
        "created_at": "2020-05-14T13:54:01.000Z",
        "kbaqna": null,
        "Education": [

        ],
        "Employements": [

        ],
        "ProfessionalLicenses": [

        ],
        "EmployeeVerification": {
          "id": "37fb0a1a-93aa-404c-84a8-16b2c1ca145e",
          "s3_gov_id": "https://abc.com/passport.jpg",
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
          "criminal_verified": 1,
          "created_at": "2020-05-14T13:56:11.000Z",
          "updated_at": "2020-05-14T14:00:04.000Z",
          "is_report_checked": false,
          "summary_of_rights_accepted": true,
          "background_check_disclosure_accepted": true,
          "id_manual_review": null,
          "super_admin_status": null,
          "EmployeeEmail": "john@email.com"
        },
        "EmployeeLimit": {
          "id": "500a2786-ba41-44c0-b86a-8678dabc823a",
          "employement": 1,
          "education": 0,
          "professional_license": 0,
          "civil_court": 0,
          "driving_license": 0,
          "EmployeeInviteGroup": {
            "id": "b53a70fe-12b8-47e9-951f-7e2d114dff64",
            "package": "silver"
          }
        }
      }
    ],
    "count": 287
  }
}
```

Get a list of employees in a company.


### URL Parameters

Parameter | Type | Description
--------- | ------- | -----------
limit| integer | Raw File of the logo.
offset| integer | Raw File of the logo.
filter | String | Types - ALL, COMPLETED, VERIFIED, PENDING,FAILED


## Get Company Adverse Actions

```shell
curl --location --request POST 'https://api.us.springverify.com/company/fetchadverseactions' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer JWT_TOKEN' \
--header 'Content-Type: application/json' \
--data-raw '{
	"limit": 3,
	"offset": 1,
	"status": "PENDING"
}'
```

> Success Response

```json
{
  "message": "successfuly retrieved adverse actions",
  "data": {
    "adverseActions": [
      {
        "id": "8bece0e1-dfa2-4ac2-a051-be420f88af84",
        "check_id": "7ddfe2b3-43c5-4cdf-9c71-e1604146d64a",
        "status": "PENDING",
        "type": "EMPLOYMENT",
        "cleared": null,
        "comments": null,
        "created_at": "2020-04-24T13:59:02.000Z",
        "updated_at": "2020-04-24T13:59:02.000Z",
        "Education": null,
        "Employement": {
          "id": "7ddfe2b3-43c5-4cdf-9c71-e1604146d64a",
          "email": "janedoe@domain.com",
          "access_id": null,
          "employer_name": "Kdiepd",
          "employer_name_verified": null,
          "employer_phone": null,
          "employer_phone_verified": null,
          "employer_address": "Manhattan Bridge",
          "employer_address_verified": null,
          "employer_town": "New York",
          "employer_town_verified": null,
          "state": "NY",
          "state_verified": null,
          "zipcode": "11201",
          "zipcode_verified": null,
          "employer_country": "United States",
          "employer_country_verified": null,
          "job_title": "Jwyf",
          "job_title_verified": null,
          "start_date": "01-01-2010",
          "start_date_verified": null,
          "end_date": "01-01-2016",
          "end_date_verified": null,
          "supervisor_name": "Jasegfyu",
          "supervisor_contact": null,
          "current_employment": null,
          "current_employment_verified": null,
          "contract_type": null,
          "contract_type_verified": null,
          "source": null,
          "created_at": "2020-04-24T13:57:57.000Z",
          "updated_at": "2020-04-24T13:57:57.000Z",
          "job_type": "full time employee",
          "reason_for_leaving": null,
          "status": 2,
          "super_admin_status": null,
          "consent": false,
          "Employee": {
            "id": "6b479bcc-e2c6-42e1-99d1-a323d072c744",
            "access_id": "bb63542f-4fc0-4fbf-8d6e-cd9bb5871552",
            "email": "janedoe@domain.com",
            "password_hash": "100d5fcb45dc552d1a9011e2707b937904f79df410199fcb7a1e2b3c022d9911",
            "first_name": "John",
            "middle_name": "James",
            "last_name": "Doe",
            "name_verified": null,
            "created_at": "2020-04-24T13:52:53.000Z",
            "updated_at": "2020-04-29T16:12:22.000Z",
            "employer_id": "1d4fb8ba-09ac-412c-aa62-58970b4d7472",
            "payment_id": "16bf1560-435c-460a-b9eb-29809f0aa558",
            "email_sent": true,
            "payment": false,
            "status": "FAILED",
            "flow_completed": true,
            "CompanyCreatedBy": "johndoe@domain.com",
            "EmployeeLimitId": "b07fbefc-895b-4020-ab2b-0d0e349d5b6a"
          }
        },
        "ReadStatuses": [
          {
            "id": "c9788ca2-9676-443d-b191-39f66c7f1121",
            "reference_id": "8bece0e1-dfa2-4ac2-a051-be420f88af84",
            "user_email": "johndoe@domain.com",
            "read": false,
            "table": "AdverseAction",
            "created_at": "2020-04-24T13:59:03.000Z",
            "updated_at": "2020-04-24T13:59:03.000Z"
          }
        ]
      }
    ],
    "count": 7
  }
}
```


This API is used to fetch Adverse actions. Possible status values - PENDING,NOTICE_SENT,IN_REVIEW,FINAL_CALL,CLOSED,IGNORED


### URL Parameters

Parameter | Type | Description
--------- | ------- | -----------
limit| integer | Raw File of the logo.
offset| integer | Raw File of the logo.
status | string | Current Status of the action.

## Adverse Action of Single Employee

```shell
curl 'https://api.us.springverify.com/api/company/adverseaction/2862fd2f-1a9d-4e26-8b1f-85838b3df8b3' \
  -H 'Accept: application/json' \
  -H 'Authorization: Bearer JWT_TOKEN' \
```

> Success Response:

```json
{
  "message": "successfuly retreived adverse action",
  "data": {
    "id": "2862fd2f-1a9d-4e26-8b1f-85838b3df8b3",
    "check_id": "5844aec7-534b-4249-8a09-789cd76efeb2",
    "status": "PENDING",
    "type": "EMPLOYMENT",
    "cleared": null,
    "comments": null,
    "created_at": "2020-04-29T11:44:02.000Z",
    "updated_at": "2020-04-29T11:44:02.000Z",
    "Education": null,
    "Employement": {
      "id": "5844aec7-534b-4249-8a09-789cd76efeb2",
      "email": "jonedoe@domain.com",
      "access_id": null,
      "employer_name": "Google",
      "employer_name_verified": null,
      "employer_phone": null,
      "employer_phone_verified": null,
      "employer_address": "Mountain View, CA",
      "employer_address_verified": null,
      "employer_town": "Palo Alto",
      "employer_town_verified": null,
      "state": "CA",
      "state_verified": null,
      "zipcode": "23233",
      "zipcode_verified": null,
      "employer_country": "USA",
      "employer_country_verified": null,
      "job_title": "Sr. Manager",
      "job_title_verified": null,
      "start_date": "01-01-2000",
      "start_date_verified": null,
      "end_date": "01-01-2010",
      "end_date_verified": null,
      "supervisor_name": "John Doe",
      "supervisor_contact": null,
      "current_employment": null,
      "current_employment_verified": null,
      "contract_type": null,
      "contract_type_verified": null,
      "source": null,
      "created_at": "2020-04-29T10:34:19.000Z",
      "updated_at": "2020-04-29T13:13:55.000Z",
      "job_type": "EMPLOYMENT",
      "reason_for_leaving": null,
      "status": 2,
      "super_admin_status": null,
      "consent": false,
      "Employee": {
        "id": "dd924f25-ee91-4b94-af86-71451cec6695",
        "access_id": "c6fcd2ff-d56f-4afd-b0bc-64039a00fc6b",
        "email": "jonedoe@domain.com",
        "password_hash": "100d5fcb45dc552d1a9011e2707b937904f79df410199fcb7a1e2b3c022d9911",
        "first_name": "John",
        "middle_name": null,
        "last_name": "Doe",
        "name_verified": null,
        "created_at": "2020-04-29T09:04:01.000Z",
        "updated_at": "2020-04-29T10:34:24.000Z",
        "employer_id": "1d4fb8ba-09ac-412c-aa62-58970b4d7472",
        "payment_id": "5e758c74-4f5c-4846-9297-93ac85fab3d5",
        "email_sent": true,
        "payment": false,
        "status": "PENDING",
        "flow_completed": true,
        "CompanyCreatedBy": "johndoe@domain.com",
        "EmployeeLimitId": "8e4d574c-8ff9-472d-b432-9d9e1383abf8"
      }
    },
    "AdverseActionLedgers": [
      {
        "id": "4c719658-37b4-4d77-a690-784e2f9f3ce8",
        "reference_id": "2862fd2f-1a9d-4e26-8b1f-85838b3df8b3",
        "status": "PENDING",
        "comments": null,
        "cleared": null,
        "deleted_at": null,
        "created_at": "2020-04-29T11:44:02.000Z",
        "updated_at": "2020-04-29T11:44:02.000Z"
      }
    ],
    "S3Files": [

    ]
  }
}
```

Retrive Adverse action for a single employee.


## Set Adverse Action Status

```shell
curl 'https://api.us.springverify.com/api/company/adverseactionpending' \
  -X 'PUT' \
  -H 'Accept: application/json' \
  -H 'Authorization: Bearer JWT_TOKEN' \
  --data-binary '{"status":"IGNORED","adverseActionId":"2862fd2f-1a9d-4e26-8b1f-85838b3df8b3"}' \
  --compressed
```


> Success Response

```json
{
  "message": "successfuly updated status",
  "data": {}
}
```

Set Adverse action status.

### URL Parameters

Parameter | Type | Description
--------- | ------- | -----------
status | string | PENDING,NOTICE_SENT,IN_REVIEW,FINAL_CALL,CLOSED,IGNORED
adverseActionId | string | Adverse Action ID on which status is being updated.

## Mark Adverse Action as Read

```shell
curl --location --request PUT 'https://api.us.springverify.com/company/adverseaction/markread' \
--header 'Authorization: Bearer JWT_TOKEN' \
--header 'Content-Type: application/json' \
--data-raw '{
	"IDs": ["01f0eead-a380-49bf-97a3-1c05bc394053"]
}	'
```

Mark adverse action as read for Company Admins.

### URL Parameters

Parameter | Type | Description
--------- | ------- | -----------
IDs | array | Array of IDs to be marked read.


## Adverse Action final Call

```shell
curl --location --request PUT 'https://api.us.springverify.com/company/adverseactionfinal' \
--header 'Authorization: Bearer JWT_TOKEN' \
--header 'Content-Type: application/json' \
--data-raw '{
	"status": "CLEAR",
	"adverseActionId": "01f0eead-a380-49bf-97a3-1c05bc394053"
}'
```

Mark Adverse action as Clear or Rejected.

### URL Parameters

Parameter | Type | Description
--------- | ------- | -----------
status | string | CLEAR or REJECTED
adverseActionId | string | Adverse Action ID on which status is being updated.


# User API Flow

## Submit Personal Details

```shell
curl --location --request POST 'https://api.us.springverify.com/employee/personal-details' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer JWT_TOKEN' \
--header 'Content-Type: text/plain' \
--data-raw '{
	"first_name":"John",
	"middle_name":"David",
	"last_name":"Doe",
	"dob":"12-11-1980",
	"ssn":"123456789",
	"email":"johndoe@gmail.com",
    "house_number": "239",
    "street_name": "Avea street"
	"address":"236 Avea street",
	"city":"Gotham",
	"state":"CA",
	"zip_code":"33433",
	"phone":"56-999222992"
}'
```


> Success Response

```json
{
    "success": true,
    "data": {
        "id": "7671647e-c45b-4ae6-95f6-9e86688c54a9",
        "access_id": "afd930bc-7292-4dbd-a067-c657ee6d5e9d",
        "email": "johndoe@gmail.com",
        "password_hash": null,
        "first_name": "John",
        "middle_name": "David",
        "last_name": "Doe",
        "name_verified": null,
        "created_at": "2020-08-25T06:31:19.000Z",
        "updated_at": "2020-08-25T06:39:25.671Z",
        "employer_id": "1d4fb8ba-09ac-412c-aa62-58070b4d7472",
        "payment_id": "9f4d3ab8-de23-4038-874d-a01c93ef88fc",
        "email_sent": true,
        "payment": false,
        "status": null,
        "flow_completed": null,
        "company_created_by": "zed@max.com",
        "employee_limit_id": "2e7155b1-947b-4fc7-aca8-7e81d19a7a2e",
        "employments": [],
        "education": [],
        "cic_criminal_records": [],
        "professional_licenses": [],
        "employee_detail": {
            "id": "518acb73-dfb2-4a29-b33e-7a35b0dbfb9b",
            "access_id": "afd930bc-7292-4dbd-a067-c657ee6d5e9d",
            "address": "236 Avea street",
            "address_verified": null,
            "driving_number": null,
            "driving_number_verified": null,
            "city": "Gotham",
            "city_verified": null,
            "state": "CA",
            "state_verified": null,
            "zipcode": "33433",
            "zipcode_verified": null,
            "country": null,
            "country_verified": null,
            "birthdate": "12-11-1980",
            "birthdate_verified": null,
            "phone": "56-999222992",
            "phone_verified": null,
            "ssn": "6789",
            "ssn_verified": null,
            "created_at": "2020-08-25T06:39:25.000Z",
            "updated_at": "2020-08-25T06:39:25.000Z",
            "employee_email": "johndoe@gmail.com"
        },
        "employee_verification": null,
        "employee_limit": {
            "id": "2e7155b1-947b-4fc7-aca8-7e81d19a7a2e",
            "employment": 2,
            "education": 1,
            "professional_license": 0,
            "all_county_criminal_search": true,
            "county_criminal_search": 0,
            "civil_court": 1,
            "driving_license": 0,
            "package_id": null,
            "created_at": "2020-08-25T06:31:19.000Z",
            "updated_at": "2020-08-25T06:31:19.000Z",
            "employee_invite_group_id": "f219ea9b-e028-417f-871c-cec7d542be64",
            "employee_invite_group": {
                "id": "f219ea9b-e028-417f-871c-cec7d542be64",
                "package": "diamond",
                "active": true,
                "created_at": "2020-08-25T06:31:19.000Z",
                "updated_at": "2020-08-25T06:31:19.000Z",
                "company_created_by": "zed@max.com",
                "package_id": "5"
            }
        },
        "kbaqna": null,
        "employee_flow": {
            "id": 288,
            "employment_flow": null,
            "education_flow": null,
            "professional_license_flow": null,
            "created_at": "2020-08-25T06:39:23.000Z",
            "updated_at": "2020-08-25T06:39:23.000Z",
            "employee_email": "johndoe@gmail.com"
        },
        "s3_files": [],
        "criminal_statuses": [],
        "sjv_criminal_reports": []
    }
}
```

This API records the information of the USER whose details will be verified. It is utmost essential that the information provided is absolutely accurate. The email provided here should be same as the one on which verification request was received.

### URL Parameters

Parameter | Type | Description
--------- | ------- | -----------
first_name|string|First name of the employee.
middle_name|string|Middle name of the employee.
last_name|string|Last name of the employee.
dob|string|Date of birth of the employee in DD-MM-YYYY.
ssn|string|SSN of the employee.
email|string|Email of the employee.
house_number|integer|House number of the employee.
street_name|string|Street name of the employee.
address|string|Address of the employee.
city|string|City of the employee.
state|string|State of the employee.
zip_code|string|Zip code of the employee.
phone|string|Phone number of the employee.


## Provide Consent

```shell
curl --location --request POST 'https://api.us.springverify.com/employee/consent' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer JWT_TOKEN' \
--header 'Content-Type: text/plain' \
--data-raw '{
	 "summary_of_rights":true,
     "background_check":true,
     "report_checked":true,
     "full_name":"John James Doe"
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
summary_of_rights|boolean|summary_of_rights flag can be true or false.
background_check|boolean|background_check flag can be true or false.
report_checked|boolean|report_checked flag can be true or false.
full_name|string|Full name of the employee.


## Knowledge Based Quiz

```shell
curl --location --request GET 'https://api.us.springverify.com/employee/kba/questions' \
--header 'Authorization: Bearer JWT_TOKEN'
```

> Success Response

```json
{
  "success": true,
  "data": {
    "data": {
      "idm_session_id": "916bac98cc38defd",
      "idmkba_response": {
        "kba_question": [
            {
                "question_id": 1,
                "question": "Which of the following addresses have you lived at?",
                "question_type": "StreetAddress",
                "options": [
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
                "question_id": 2,
                "question": "Which of the following streets have you lived in?",
                "question_type": "StreetName",
                "options": [
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
                "question_id": 3,
                "question": "Which of these cities are you associated with?",
                "question_type": "Cities",
                "options": [
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
                "question_id": 4,
                "question": "Which of these phone numbers have you ever used previously?",
                "question_type": "PhoneNumbers",
                "options": [
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
                "question_id": 5,
                "question": "Which of these businesses are you associated with?",
                "question_type": "Businesses",
                "options": [
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
        "kba_status": {
          "questions_answered": 0,
          "no_of_questions": 5
        },
        "is_kba_enabled": "1"
      }
    },
    "attempt_number": 1,
    "success": true
  }
}
```

This API is used to fetch a Knowledge based quiz. The users will give their answers to the multiple choice questions and this will score their results.


## Submit KBA Quiz

```shell
curl --location --request POST 'https://api.us.springverify.com/employee/kba/verify' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer JWT_TOKEN' \
--header 'Content-Type: text/plain' \
--data-raw '{
"qna":[
    {
      "question_id": 1,
      "answer_id": 5
    },{
      "question_id": 2,
      "answer_id": 5
    },{
      "question_id": 3,
      "answer_id": 5
    },{
      "question_id": 4,
      "answer_id": 5
    },{
      "question_id": 5,
      "answer_id": 5
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
  "message": "Your answers didn't seem to be correct, please try uploading IDs.",
   "stack": "message"
}
```

This API is used to submit the answers to the Knowledge based quiz.


### URL Parameters

Parameter | Type | Description
--------- | ------- | -----------
qna|array|Contains the array of question id and their respective answer id.


## Upload and Verify ID

```shell
curl --location --request POST 'https://api.us.springverify.com/employee/upload/id' \
--header 'Authorization: Bearer JWT_TOKEN' \
--form 'front=@/path/to/file' \
--form 'back=@/path/to/file' \
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
front|image|Front side of the ID.
back|image|Back side of the ID.


## Upload and Verify Passport

```shell
curl --location --request POST 'https://api.us.springverify.com/employee/upload/passport' \
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
curl --location --request GET 'https://api.us.springverify.com/employee/id/manual-review' \
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
curl --location --request GET 'https://api.us.springverify.com/employee/id/try-count' \
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
                "s3_gov_id": "https://spring-verify-us.s3.amazonaws.com/employee-id/fuspuzumlu%40desoz.com/passport.jpg",
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
