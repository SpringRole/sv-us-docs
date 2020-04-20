SpringVerify-US REST APIs
=========================

Sign UP
-------

This API is used to register a company ADMIN. Once registered an verification email is sent to the registered email.

.. note::
	 If email is not recieved in inbox check spam folder. Till you have not verified email further APIs will not work.


**Path** : /signup

**Method** : POST

**Body** : { email, password, confirmPassword, firstName, lastName, phone, domain }

**Example Request**
 	.. code::
		
		curl --location --request POST 'http://localhost:3080/signup' \
			--header 'Content-Type: application/json' \
			--header 'Postman-Token: 01a06cc3-6752-4981-9a30-66039db3b59e' \
			--header 'cache-control: no-cache' \
			--data-raw '{
			    "email": "joyolep547@emailnube.com",
			    "password": "daaad6e5604e8e17bd9f108d91e26afe6281dac8fda0091040a7a6d7bd9b43b5",
			    "confirmPassword": "daaad6e5604e8e17bd9f108d91e26afe6281dac8fda0091040a7a6d7bd9b43b5",
			    "firstName": "arnav",
			    "lastName": "chaudhary",
			    "phone": "123456789",
			    "domain": "emailnube.com"
			}'

**Example Response**
	.. code::

		{
			"message": "Account created successfully."
		}

**Query Parameters**
	
	* email (string): Email registered in springverify. 
	* password (string): Password registered in springverify.
	* confirmPassword (string): Confirm the password.
	* firstName (string): First name of the user.
	* lastName (string): Last name of the user.
	* phone (string): phone number of the user.
	* domain (string): domain of the company. Must be same as the email domain.

.. note::
	 Password HASH (SHA256) should be given in this API and not the real password.



Login
-----

Aim is to generate a JWT token , which will be used for all further API calls. 

Client needs to send JWT token in the header successfully Call other APIs.

**Path** : /login

**Method** : POST

**Body** : { email, password, role }

**Example Request**
 	.. code::
		
		curl --location --request POST 'http://localhost:3080/login' \
			--header 'Content-Type: application/json' \
			--data-raw '{
			    "email": "ajitesh.tiwari@springrole.com",
			    "password": "daaad6e5604e8e17bd9f108d91e26afe6281dac8fda0091040a7a6d7bd9b43b5",
			    "role":"admin"
			}'

**Example Response**
	.. code::

			{
			    "token": "eyJhbGciOiJIUzI1NiJ9.eyJlbWFpbCI6ImFqaXRlc2gudGl3YXJpQHNwcmluZ3JvbGUuY29tIiwiZG9tYWluIjoic3ByaW5ncm9sZS5jb20iLCJleHBpcmVzIjoxNTg0NTIxNzQ3MjkwfQ.Dzlg-O0TujXELtSRrpjXNqYWtf6fRLn49W6EUjl7Z5U",
			    "profile": {
			        "firstName": "Ajitesh",
			        "lastName": "Tiwari",
			        "email": "ajitesh.tiwari@springrole.com",
			        "domain": "springrole.com",
			        "company": {
			            "id": 1,
			            "name": "abcde",
			            "address": "xyza",
			            "city": "123",
			            "state": "ME",
			            "zipcode": "122330",
			            "taxIdNumber": "12-3456789",
			            "s3Logo": "https://pdf-reports-springrole.s3.amazonaws.com/Address%20Proof.jpg"
			        }
			    }
			}

**Query Parameters**
	
	* email (string): Email registered in springverify. 
	* password (string): Password registered in springverify.
	* role (string): It is the role of the login entity (use 'admin' for company login).

.. note::
	 Password HASH (SHA256) should be given in this API and not the real password.

**Response Parameters**

	* token (string): JWT which will be used in further APIs.
	* profile (Object): This is contains the user profile.


Register Company
----------------

This API is used to register a company. JWT token retrieved will be used for authentication.

**Path** : /company

**Method** : POST

**Body** : { name, address, city, state, zipcode, taxIdNumber }

**Example Request**
 	.. code::
		
		curl --location --request POST 'http://localhost:3080/company' \
			--header 'Authorization: Bearer eyJhbGciOiJIUzI1NiJ9.eyJlbWFpbCI6ImFqaXRlc2gudGl3YXJpQHNwcmluZ3JvbGUuY29tIiwiZXhwaXJlcyI6MTU1OTE0MTcwNDE4Mn0.hMCXlXXWFP408z1_WnFl2kQo-OR6mGsk1lSHmKhsNCk,Bearer eyJhbGciOiJIUzI1NiJ9.eyJlbWFpbCI6ImFqaXRlc2gudGl3YXJpQHNwcmluZ3JvbGUuY29tIiwiZXhwaXJlcyI6MTU1OTIwOTEyMjA2Mn0.y0gXfpo8Wmqt3JeuLjUCW3fbsxPQBMt7BeJOTjCskk4' \
			--header 'Content-Type: application/json' \
			--data-raw '{
			    "name": "Google",
			    "address": "Address",
			    "city": "reqbodycity",
			    "state": "reqbodystate",
			    "zipcode": "12345",
			    "taxIdNumber": "reqbodytaxIdNumber"
			}'

**Example Response**
	.. code::

		{
			"message": "Company updated successfully"
		}

**Query Parameters**
	
	* name (string): Name of the registered company. 
	* address (string): Address of the company.
	* city (string): City of the company.
	* state (string): State of the company.
	* zipcode (string): Zipcode of the company.
	* taxIdNumber (string): Tax number of the company


Get Packages
------------

This API is used to get the currently available packages and prices.

**Path** : /company/package

**Method** : GET

**Example Request**
 	.. code::
		
 		curl --location --request GET 'http://api-stage.us.springverify.com/company/package' \
			--header 'Authorization: Bearer eyJhbGciOiJIUzI1NiJ9.eyJlbWFpbCI6ImFqaXRlc2gudGl3YXJpQHNwcmluZ3JvbGUuY29tIiwiZG9tYWluIjoic3ByaW5ncm9sZS5jb20iLCJleHBpcmVzIjoxNTg0NTIyNjcwNjkwfQ.HBW5pvC3vZLl_eL1fnC-ujceiqonbGmzAP9idRsnK4s'

**Example Response**
	.. code::

		{
		    "success": true,
		    "successMsg": "actions retrived successfuly",
		    "data": {
		        "package": [
		            {
		                "id": "1",
		                "name": "bronze",
		                "employement": "1",
		                "education": "0",
		                "professional_license": "0",
		                "civil_court": "0",
		                "driving_license": "0",
		                "price": "1000",
		                "created_at": "2019-07-03T11:07:27.000Z",
		                "updated_at": "2019-11-11T11:53:38.000Z"
		            },
		            {
		                "id": "2",
		                "name": "silver",
		                "employement": "2",
		                "education": "0",
		                "professional_license": "0",
		                "civil_court": "0",
		                "driving_license": "0",
		                "price": "2000",
		                "created_at": "2019-07-03T11:07:27.000Z",
		                "updated_at": "2019-11-11T11:53:38.000Z"
		            },
		            {
		                "id": "3",
		                "name": "gold",
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
		                "id": "4",
		                "name": "platinum",
		                "employement": "2",
		                "education": "1",
		                "professional_license": "0",
		                "civil_court": "0",
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

Invite Candidates
-----------------

AIM of this API is used to inveite employee/employees. It can be used to invite employee in bulk. Once payment is done sucessfully then email is sent to the employee.

.. note::
	 This API should only be used after getting the packages in previous API.

**Path** : /employee/invite

**Method** : POST

**Example Request**
 	.. code::
		
		curl --location --request POST 'http://localhost:3080/employee/invite' \
		--header 'Content-Type: application/json' \
		--data-raw '{
			"emailList":["xifehoc349@link3mail.com"],
			"package":"gold",
			"addOns":{
		    	"employement":1,
		    	"education":1,
		    	"license":1,
		    	"drivingLicense":1,
		    	"civilCourt":1
		    },
		    "couponCode":"awew"
		}'

**Example Response**
	.. code::

		{
		    "success": true,
		    "successMsg": "invites sent successfuly",
		    "data": {
		        "price": 3950,
		        "id": "d78a273d-b045-4dc7-8325-8fa7b585ff81",
		        "count": 1
		    }
		}

**Query Parameters**
	
	* emailList (array): Contains the list of employee emails. 
	* package (string): Name of the package (retrieved in get packages api).
	* addOns (Object): Extra checks can be added in a package. 
	* CouponCode (string): This coupon Code is to avail discounts.

**Response Parameters**

	* price (number): It gives price in cents that has to be payed.
	* id (string): It is the reference id against which payment has to be made.
	* count (number): It gives the number of candidates.

Payment For Invites
-------------------

This API is called right after inveite API. The reference id retrieved in previous api is used in this api.
Once the payment is done sucessfully the email is sent out to the candidate. 

**Path** : /payment/chargeuser

**Method** : POST

**Example Request**
 	.. code::
		
		curl --location --request POST 'http://localhost:3080/payment/chargeuser' \
			--header 'Content-Type: application/json' \
			--data-raw '{
				"id": "72586cfc-2b46-468a-8f80-af64de0106e0"
			}'

**Example Response**
	.. code::

		{
			"success": true,
			"successMsg": "money added successfully",
			"data": {}
		}

**Query Parameters**
	
	* id (string): Reference id retreived in invite employee API.

Get Candidate
-------------

AIM of this API is used to get employee details.

.. note::
	 This API should only be used after getting the packages in previous API.

**Path** : /company/employee

**Method** : GET

**Example Request**
 	.. code::
		
		curl --location --request GET 'http://localhost:3080/company/employee?id
		=5d9f192f-446d-4576-926c-289863d6dbf6' \
		--header 'Authorization: Bearer eyJhbGciOiJIUzI1NiJ9.eyJlbWFpbCI6ImFqaXRlc2gudGl3YXJpQHNwcmluZ3JvbGUuY29tIiwiZG9tYWluIjoic3ByaW5ncm9sZS5jb20iLCJleHBpcmVzIjoxNTg3Mzk1Mjg0OTM5fQ.121Niu--6hEtBJ35_-1Plug96oYY63kG8mVPcQPX-7E' \


**Example Response**
	.. code::

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
		            "CompanyCreatedBy": "ajitesh.tiwari@springrole.com",
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
		                    "CompanyCreatedBy": "ajitesh.tiwari@springrole.com",
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

**Query Parameters**
	
	* employeeId (uuid): Contains the id of the employee.
