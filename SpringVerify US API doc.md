# Welcome!

Hello and welcome to SpringVerify.

This is the API documentation section of SpringVerify for customers and users located in the USA. It is divided into three sections for ease of navigation:
1. Introductions - _where entities, environments and authentication protocals are detailed._
2. Company API flow - _where functions available to users logged in as company admins are detailed._
3. User API flow - _where functions available to users logged in as employees are detailed._

---

## Introduction

SpringVerify currently has three environments -- stage, acceptance and production. All three environments have independent databases. While in the stage phase, it is mandatory to use the base URL of the stage environment.

The language bindings are in cURL presently with node.js and PHP bindings in development. The API documentation is set up with a display area on the right to show examples.

### Internal objects

| User Roles | Response |
| ---------- | -------- |
| Company | An entity that has users. These users can be employees who are **performing the verification** or are **being verified**. |
| Admin | A user who is performing the verification. |
| Employee | A user whose details are being verified on the platform. |

### Environment URLs
 
**Stage**: https://api-stage.us.springverify.com
**Acceptance**: https://api-acceptance.us.springverify.com
**Production**: https://api.us.springverify.com
 
>Currently we have two types of coupons -- "open" and "closed". Please contact sales@springverify.com regarding coupon codes for your company.

### Authentication

Authenticating in SpringVerify is done on the basis of JSON Web Tokens. Once registered, a user will receive a token to be used for subsequent API requests. If misplaced, the token can be retrieved by logging in again.

>The user must replace `JWT_TOKEN` in the examples below with their personal token.

---

## Company

This section covers the API details available for users logged in as _admins_, i.e., users who wish to conduct background verification checks on employees (or prospective employees).

### Signup

The signup API is used to register an admin -- a user that performs verifications -- in a company. Once a user is registered, a verification email is sent to the registered address.

>The `Password` and `Confirm Password` fields shouldbe hashed using SHA256 beforehand.

**Query Parameters**

| Parameter | Type | Description | Mandatory |
| --- | --- | --- | --- |
| email | `string` | The email address used to register with SpringVerify | Yes |
| password | `string` | A strong, hashed password of minimum 8 characters | Yes |
| confirm_password | `string` | Repeat the password to confirm | Yes |
| first_name | `string` | The user's first name | Yes |
| last_name | `string` | The user's last name | No |
| phone | `string` | The user's preferred phone number | No |
| domain | `string` | The company's domain. Must be the same as the email domain | Yes |

With this, the user is successfully registered into a company's domain as an admin.

### Verify an Admin's Email address

This API is used to verify a company admin's email address. If this user is the first user for a company, then the company will be created as an entity on the admin being successfully verified.

### Resend the verification email to a company admin

If an admin requires the verification email to be sent again, this API should be used.

### Logging in

For logging in, the aim is to generate a JSON web token that is to be used in all subsequent API calls. The JWT generated will be valid for one hour.

To call the subsequent APIs, the user will need to send the JWT successfully in the header of those APIs.

>The password should be hashed using SHA256 beforehand.

**URL Parameters**

| Parameter | Type | Description |
| --- | --- | --- |
| email | `string` | The email address used to register with SpringVerify |
| password | `string` | A strong, hashed password of minimum 8 characters |
| role | `string` | The role of the user being logged in - in this case, `admin` |

### Forgot password

In case of a misplaced password, this API should be used to send an email to the user to reset their password. The email will contain a token-embedded URL which will be used to call the [Reset Password](https://docs.us.springverify.com/#reset-password) API.

### Reset Password

When a user requests declares a forgotten password, an email will be sent using the [Forgot Password](https://docs.us.springverify.com/#forgot-password) API. This email will consist of a token-embedded URL that is to be passed in this API along with the new password.

**URL Parameters**

| Parameter | Type | Description |
| --- | --- | --- |
| password | `string` | A strong, hashed password of minimum 8 characters |

### Register your company

A user with the role of an admin can create a company once their profile is [verified](https://docs.us.springverify.com/#verify-company-admin-email). A valid JWT will be needed to authenicate the user in this API.

**URL Parameters**

| Parameter | Type | Description |
| --- | --- | --- |
| name | `string` | Name of the company being registered |
| address | `string` | Address of the company being registered |
| city | `string` | The city where the company being registered is located |
| state | `string` | The state where the company being registered is located |
| zipcode | `string` | The ZIP code of the postal district in which the company is located |
| tax_id_number | `string` | Tax number of the company that is being registered |

### Upload a logo

Upload the company's logo using this API.

**URL Parameters**

| Parameter | Type | Description |
| --- | --- | --- |
| logo | `file` | Raw file of the logo |

### Get company details

For a registered admin with a valid JWT, this API can be used to get the details of the company. This is different from the company profile. A company's profile contains details of the company given during registration as well as the count of the employees whose profiles were verified, failed or is pending.

### Get company profile

For a registered admin with a valid JWT, this API can be used to get the company's profile. This is different from the company details. A company's profile contains details of the company given during registration as well as the count of the employees whose profiles were verified, failed or is pending.

### Get available packages

This API is used to get the available pricing plans and packages.

### Invite candidates

This API is used to invite existing and/or prospective employees to get their profiles verified. It can be used to invite employees in bulk. However, emails will be sent to the employees only once the payment is successfully completed.

>This API is to be used only after using the [Get available packages](https://docs.us.springverify.com/#get-available-packages) API.



**URL Parameters**

| Parameter | Type | Description |
| --- | --- | --- |
| email_list | `array` | Contains a list of the email addresses of the employees to be contacted |
| package | `string` | Name of the package as retrieved from the [Get available packages](https://docs.us.springverify.com/#get-available-packages) API |
| add_ons | `object` | To add additional services into the retrieved package |
| coupon_code | `string` | Any coupon code that the admin has for a discounted pricing |

**Response Parameters**

| Parameter | Type | Description |
| --- | --- | --- |
| price | `number` | The price (in cents) that is to be paid |
| id | `string` | The reference ID against which the payment is being made |
| count | `number` | The total number of candidates that are being verified |

### Save Credit Card in Stripe for Payments

This API is used to register the credit card with Stripe for payments. If this API call is successful, the response generated will include an `id` parameter that is used in the [Payment for Invites](https://docs.us.springverify.com/#payment-for-invites) API.

| Parameter | Type | Environment | Value |
| --- | --- | --- | --- |
| STRIPE_TOKEN | `string` | Development | pk_test_51H1niLFq1aDrrzKzoQEG7espm3z3HirSoy5IJl8tWbxJk18pZDx67Y70mCynyLKOrEJFWarCiVSigW9RuW1bqdeU00lRNETXxe |
| STRIPE_TOKEN | `string` | Production | pk_live_51H1niLFq1aDrrzKztsRsWduNwtnBIIuRWSdeAJtIsgFefyWukEuqx8J6T8djCLiHAMDNNvdKpYkdiqq7iP9hwtCK00VdWazMPg |

### Save payment info

To save the token received from the [Save Credit Card in Stripe for Payments](https://docs.us.springverify.com/#save-credit-card-in-stripe-for-payments), use this API.

### Payment For Invites

Once a user has been invited to get themselves verified, the verification must be paid for. This API is used for the payment purpose and must be called immediately after the [Invite Candidates](https://docs.us.springverify.com/#invite-candidates) API. The reference ID retrieved from the "Invite Candidates" API is used here. Emails to employees (or prospective employees) requesting verification will be sent only after the payment is successful. If the `send_email` field is set to `false`, the API will return the verification links of the employees in the response.

**URL Parameters**

| Parameter | Type | Description |
| --- | --- | --- |
| id | `string` | The reference ID retrieved from the [Invite Candidates](https://docs.us.springverify.com/#invite-candidates) API. |
| send_email | `boolean` | `true` -- sends emails to the candidates. |
| | | `false` -- returns the verifications links of the candidates. |

### Get Single Candidate (or employee)

When the details of a specific user that is already verified -- a candidate or an employee -- is to be retrieved, use this API. This API retrivies the details of the user that have been verified, as well as the pricing package in which the user's profile was verified, along with the date of the most recent verification.

**URL Parameters**

| Parameter | Type | Description |
| --- | --- | --- |
|id | `uuid` | Contains the unique ID of the user. |

### Search Employee

This API searches and retrieves the profile of a specific employee that is already verified.


**URL Parameters**

| Parameter | Type | Description |
| --- | --- | --- |
| search | `string`| Contains the search term |

### Get company action

This API informs an admin/s of the activities of a specific employee or a candidate in the following scenarios:
* The candidate has been invited to provide their information to initiate the verification.
* The candidate has entereed their personal information.
* The candidate has successfully uploaded their driver's license.
* The candidate has successfully verified their identity using their driver's license.
* The candidate has failed to verify their identity using their driver's license.

---

# Question - Why are there repetitions in this API cURL? Can this section be merged with the next section?

---

### Get Company Actions Pertaining to an employee

This API informs an admin/s of the activities of a specific employee or a candidate in the following scenarios:
* The candidate has been invited to provide their information to initiate the verification.
* The candidate has entereed their personal information.
* The candidate has successfully uploaded their driver's license.
* The candidate has successfully verified their identity using their driver's license.
* The candidate has entered their education information.
* The candidate has entered their employment information.
* Verification for the education information has been initiated.
* Verification for the employment information has been initiated.

### Get Company Employees by Filter

**URL Parameters**

| Parameter | Type | Description |
| --- | --- | --- |
| limit | `integer` | Page limit |
| offset | `integer` | Page offset |
| filter | `string` | ALL/COMPLETED/VERIFIED/PENDING/NULL(AWAITING EMPLOYEE INPUT)/FAILED.

### Get Company Adverse Action Counts

This API gives the count of the total adverse actions that are/have been:
* closed -- the employee's response has been accepted or rejected and the verification is completed.
* pending -- the employee is to be informed of the issue; or the employee's response is to be received.
* being reviewed -- the employee's response is being reviewed.
* sent a notice -- the employee has been informed of an issue with their verification.
* taken a final call on -- a decision has been taken by the company on the adverse action and is to be informed to the employee.

### Get Company Adverse Actions

This API can be used to get a list of the adversities found across all the profiles that have been processed.

---

# 1. Verification needed
# 2. URL Parameters Description - Accurate?

---

**URL Parameters**

| Parameter | Type | Description |
| --- | --- | --- |
| limit | `integer` | _Raw file of the logo._ |
| offset | `integer` | _Raw file of the logo._ |
| status | `string` | Current status of the action: (PENDING/NOTICE_SENT/IN_REVIEW/FINAL_CALL/CLOSED)

### Adverse Action of Single Employee

For a specific employee whose verification has resulted in an adverse action, this API can be used to get a list of the adversities found in the profile.

**URL Parameters**

| Parameter | Type | Description |
| --- | --- | --- |
| adverse_action_id | `uuid` | The ID of the adverse action. |

### Mark Adverse Action as Read

Once an admin has been notified of a list of adversity (of a single employee's profile or of multiple profiles), this API is used to let the admin mark the notification as "read".

**URL Parameters**

| Parameter | Type | Description |
| --- | --- | --- |
| IDs | `array` | An array of the IDs that are to be marked as read. |

### Clear or Send Adverse Action Notice

Once notified, an admin can decide to take an action on the adversity notified or ignore it. This is the initial action particular adverse action lifecycle for the Admin.

**URL Parameters**

| Parameter | Type | Description |
| --- | --- | --- |
| status | `string` | Current status of the action: NOTICE_SENT/IGNORED. |
| adverse_action_id | `string` | The ID of the adverse action whose status is being updated. |

### Set Final Adverse Action Status

On an employee profile that is being processed for adversities, an admin can set a final adverse action using this API. This is the final action in a particular adverse action lifecycle for the Admin.

**URL Parameters**

| Parameter | Type | Description |
| --- | --- | --- |
| status | `string` | Current status of the action: CLEAR/REJECT. |
| adverse_action_id | `string` | The ID of the adverse action whose status is being updated. |

### Get Criminal Report

On an employee profile that is being processed for adversities, an admin can get the criminal report using the SJV ID in the adverse action object of this API. This is a sample JSON response.

**URL Parameters**

| Parameter | Type | Description |
| --- | --- | --- |
| sjv_id | `string` | The SJV ID of the criminal report. |

---

## User

This section covers the API details available for users logged in as _employees_ (or candidates), i.e., users who wish to get background verification checks performed on their profiles.

### Submit Personal Details

An employee can submit their personal details using this API. It is of the utmost importance that the details entered here are absolutely accurate. For creating the profile, the same email ID must be used to which the verification request was sent.

**URL Parameters**

| Parameter | Type | Description |
| --- | --- | --- |
| first_name | `string` | The employee's first name. |
| middle_name | `string` | The employee's middle name. |
| last_name | `string` | The employee's last name. |
| dob | `string` | The employee's date of birth in the format DD-MM-YYYY. |
| ssn | `string` | The employee's SSN. |
| email | `string` | The employee's email address - must be the same address to which the verification request was sent. |
| house_number | `integer` | The employee's house number. |
| street_name | `string` | The name of the street where the employee's house is located. |
| address | `string` | The employee's residential address. |
| city | `string` | The city where the employee's house is located. |
| state | `string` | The state where the employee's house is located. |
| zip_code | `string` | The ZIP code of the postal district where the employee's house is located. |
| phone | `string` | The employee's phone number. |

### Update Personal Details

An employee who has created a profile can use this API tp update their persona details. The updates can be made until the verification process is complete.

**URL Parameters**

| Parameter | Type | Description |
| --- | --- | --- |
| first_name | `string` | The employee's first name. |
| middle_name | `string` | The employee's middle name. |
| last_name | `string` | The employee's last name. |
| dob | `string` | The employee's date of birth in the format DD-MM-YYYY. |
| ssn | `string` | The employee's SSN. |
| email | `string` | The employee's email address - must be the same address to which the verification request was sent. |
| house_number | `integer` | The employee's house number. |
| street_name | `string` | The name of the street where the employee's house is located. |
| address | `string` | The employee's residential address. |
| city | `string` | The city where the employee's house is located. |
| state | `string` | The state where the employee's house is located. |
| zip_code | `string` | The ZIP code of the postal district where the employee's house is located. |
| phone | `string` | The employee's phone number. |

### Provide Consent

After creating their profile, an employee needs to explicitly provide consent to allow background verification using this API. The name provided here should match the full name provided in the Submit Personal Details API.

**URL Parameters**

| Parameter | Type | Description |
| --- | --- | --- |
| summary_of_rights | `boolean` | Indicates that the employee has read the summary of rights. This flag can be "true" or "false". |
| background_check | `boolean` | Indicates that the employee has agreed for a background check. This flag can be "true" or "false". |
| report_checked | `boolean` | Indicates that the employee's report has been checked. This flag can be "true" or "false". |
| full_name | `string` | The employee's full name. |

### Knowledge Based Quiz

This API is used to generate a knowledge-based quiz about the employee to verify authenticity. A series of questions relating to the employee's profile will be asked to which the employee is required to answer. Based on these answers, a score will be generated.

### Submit KBA Quiz

This API is used to submit the employee's answers of the [Knowledge Based Quiz](https://docs.us.springverify.com/#knowledge-based-quiz).

**URL Parameters**

| Parameter | Type | Description |
| --- | --- | --- |
| qna | `array` | This is an array that contains the IDs of the question as well as the ID of the responses to those questions. |

### Upload and Verify ID

This API records the ID of the user and verifies its authenticity.

**Important Notes:**

1. The maximum allowed size of the image uploaded (per image) is capped at 2MB. Uploading photos larger than 2MB will throw an exception.
2. If using a mobile device, capturing a clear, non-blurry image is extremely important so that the ID can be processed accurately.
3. At a minimum, a 5MP camera should be used to capture the images for the UploadID parameter.
4. It is mandatory to have a contrast between the ID and the background. The background must be solid colored.
5. It must be ensured that all the edges of the ID are visible and none of the edges are cutoff.

**URL Parameters**

| Parameter | Type | Description |
| --- | --- | --- |
| front | `image` | The image of the front of the ID. |
| back | `image` | The image of the back of the ID. |

### Upload and Verify Passport

This API records the Passport of the user and verifies it authenticity.

This API is used to upload an image of the employee's passport that will be scanned and parsed. The image should have the employee's information on it on the first or the second page. It must be a single picture that captures both pages of the passport. This follows the same logic as the other uploadId endpoints -- the picture needs to be clear with minimal glare and must have sufficient lighting.

There are several variables that are more likely to cause a document to fail:
1. The quality of the captured image - blurred images or images with reflections.
2. Personalization of the document such as documents with especially long names.
3. Variations in manufacturing techniques - for example, a card printed on the wrong side or slight variations in the printing location.
4. Wear and aging of the document - a worn or dirty card can cause failure.
5. Tampering and counterfeiting - unlawful changes or reproduction of documents.

**URL Parameters**

| Parameter | Type | Description |
| --- | --- | --- |
| front | `image` | The image of the passport. |

### Request Manual Review of the ID or Passport

If the uploading of an ID _and_ of the passport fails more than once, a manual review can be requested using this API.

### Get ID verification tries

This API will provide the number of tries that an employee has made for uploading the Driving License ID , the Passport and also the number of tries an employee has made on the KBA. The maximum allowed limit is 2 per method per candidate.

### Get Candidate Info

This API returns all info pertaining to a candidate including all types of verification.

### Add Candidate Employment

This API will be used to submit the Employment records for the Employee.

**URL Parameters**

| Parameter | Type | Description |
| --- | --- | --- |
| employer_name | `string` | Name of the employer. |
| employer_address | `string` | Address of the employer. |
| employer_phone | `string` | Phone number of the employer. |
| employer_town | `string` | Town where the employer is located. |
| state | `string` | State where the employer is located. |
| zip_code | `string` | Zip Code of the postal district where the employer is located. |
| country | `string` | Country where the employer is located. |
| job_title | `string` | Job title of the latest employment. |
| start_date | `string` | Start Date of the employment. |
| end_date | `string` | End Date of the employment. |
| supervisor_name | `string` | The name of the employee's supervisor in this employment. |
| current_employment | `string` | Is this the employee's current employment? |
| job_type | `string` | Is this a contract or a full-time employment? |
| reason_for_leaving | `string` | Reason for leaving the job (optional). |
| supervisor_contact | `string` | Contact details of the supervisor. |

### Delete Employment

This API is used to delete the employment submitted in the "Add Candidate Employment" API before it goes for verification.

**URL Parameters**

| Parameter | Type | Description |
| --- | --- | --- |
| id | `uuid` | UUID of the employment record. |

### Edit Employment

By adding the ID received in the "Submit Employment" response in this API, it can be used to edit the employment data.

**URL Parameters**

| Parameter | Type | Description |
| --- | --- | --- |
| id | `uuid` | UUID of the employment record. |
| employer_name | `string` | Name of the employer. |
| employer_address | `string` | Address of the employer. |
| employer_phone | `string` | Phone number of the employer. |
| employer_town | `string` | Town where the employer is located. |
| state | `string` | State where the employer is located. |
| zip_code | `string` | Zip Code of the postal district where the employer is located. |
| country | `string` | Country where the employer is located. |
| job_title | `string` | Job title of the latest employment. |
| start_date | `string` | Start Date of the employment. |
| end_date | `string` | End Date of the employment. |
| supervisor_name | `string` | The name of the employee's supervisor in this employment. |
| current_employment | `string` | Is this the employee's current employment? |
| job_type | `string` | Is this a contract or a full-time employment? |
| reason_for_leaving | `string` | Reason for leaving the job (optional). |
| supervisor_contact | `string` | Contact details of the supervisor. |

### Trigger Employment Verification

Once the employment records have been submitted and finalized, an Employment Verification request can be triggered using this API.

### Add Employee's Education

This API will be used to submit the education records of the employee.

**URL Parameters**

| Parameter | Type | Description |
| --- | --- | --- |
| school_name | `string` | The name of the employee's school. |
| school_type | `string` | The type of the employee's school. |
| school_campus | `string` | The campus of the employee's school. |
| address | `string` | The address of the employee's school. |
| city | `string` | The city where the employee's school is located. |
| state | `string` | The state where the employee's school is located. |
| zip_code | `string` | The ZIP code of the postal district where the employee's school is located. |
| country | `string` | The country where the employee's school is located. |
| start_date | `string` | Start date of the course. |
| end_date | `string` | End date of the course. |
| degree | `string` | Official degree of the course. |
| currently_attending | `string` | This flag will be set to 1 if the employee is currently attending the course, else it will be set to 0. |
| completed_successfully | `string` | This flag will be set to 1 if the employee has successfully completed the course, else it will be set to 0. |
| major | `string` | The field in which the employee has majored. |

### Edit Education Entry

This API will be used to edit the education records for the Employee. The id parameter passed will be the same as received at the time of Education Entry submission.

**URL Parameters**

| Parameter | Type | Description |
| --- | --- | --- |
| id | `uuid` | UUID of the education entry. |
| school_name | `string` | The name of the employee's school. |
| school_type | `string` | The type of the employee's school. |
| school_campus | `string` | The campus of the employee's school. |
| address | `string` | The address of the employee's school. |
| city | `string` | The city where the employee's school is located. |
| state | `string` | The state where the employee's school is located. |
| zip_code | `string` | The ZIP code of the postal district where the employee's school is located. |
| country | `string` | The country where the employee's school is located. |
| start_date | `string` | Start date of the course. |
| end_date | `string` | End date of the course. |
| degree | `string` | Official degree of the course. |
| currently_attending | `string` | This flag will be set to 1 if the employee is currently attending the course, else it will be set to 0. |
| completed_successfully | `string` | This flag will be set to 1 if the employee has successfully completed the course, else it will be set to 0. |
| major | `string` | The field in which the employee has majored. |

### Delete Education

This API is used to delete the education entry submitted in the previous API before it goes for verification.

**URL Parameters**

| Parameter | Type | Description |
| --- | --- | --- |
| id | `uuid` | UUID of the education entry. |

### Trigger Education Verification

Once the education records have been submitted and finalized, an Education Verification request can be triggered using this API.

### Create Password

After all the previously mentioned checks have been successfully submitted and triggered, the employee can create a password for their profile using this API.

>The `Password` fields shouldbe hashed using SHA256 beforehand.

**URL Parameters**

| Parameter | Type | Description |
| --- | --- | --- |
| token | `string` | JWT_TOKEN |
| password_hash | `string` | Hash of the password. (8 characters minimum). |

### Reset Password

This API is used to reset the employee profile password.

>The `Password` fields shouldbe hashed using SHA256 beforehand.

**URL Parameters**

| Parameter | Type | Description |
| --- | --- | --- |
| password | `string` | Hash of the password. (8 characters minimum). |

### Complete Employee Flow

This API is used to let the system know that employee form has been submitted successfully.

### Employee Login

For logging in, the aim is to generate a JSON web token that is to be used in all subsequent API calls. The JWT generated will be valid for one hour.

To call the subsequent APIs, the user will need to send the JWT successfully in the header of those APIs.

>The password should be hashed using SHA256 beforehand.

**URL Parameters**

| Parameter | Type | Description |
| --- | --- | --- |
| email | `string` | The email address used by the employee to register with SpringVerify. |
| password | `string` | The password used by the employee to register with SpringVerify. (Hashed, 8 characters minimum). |
| role | `string` | The role of the user being logged in - in this case, `employee`. |

---