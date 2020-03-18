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