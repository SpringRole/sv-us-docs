Appendex
========

Appendex 1
----------
Understanding Government Responses

Driving License:
****************

**success Response**

.. code::
	  
			{
			    "result": "id_found",
			    "message": "Id was found in the source",
			    "success": true,
			    "ocr": {
			        "address": "M-150, BLK-I NAGAR, PASCHIM VIHAR,DELHI GURUHARKISHAN",
			        "date_of_birth": "1991-08-24",
			        "date_of_validity": null,
			        "fathers_name": "SANJAY AHUJADOB",
			        "id_number": "DL-0420150346015",
			        "is_scanned": "false",
			        "issue_dates": {
			            "LMV": null,
			            "MCWG": null,
			            "TRANS": null
			        },
			        "name_on_card": "KARAN AHUJA",
			        "pincode": "110087",
			        "state": "Delhi",
			        "street_address": "BLK-I NAGAR, PASCHIM VIHAR, GURUHARKISHAN",
			        "type": [
			            "LMV-NT",
			            "MCWG"
			        ],
			        "validity": {
			            "NT": "2035-01-30",
			            "T": null
			        }
			    },
			    "matched_information": {
			        "message": "OCR Data has been verified with government source",
			        "source": "SARATHI",
			        "dl_status": "Active",
			        "status": "id_found",
			        "name_match": 98.33333333333333,
			        "dob_match": true,
			        "ocr_id_match": true
			    }
			}

**dl_status** is status of the Driving License found with source, look for Active/Inactive.

**ocr_id_match** will be true if OCR scanned ID and Government pulled ID were same.

**name_match** will be percentage match  of scanned Name and government retrieved name.

**dob_match** will bebe true if OCR scanned date of birth and Government pulled date of birth were same.

PAN CARD
********

**success response**

.. code::

				{
				    "result": "id_found",
				    "message": "PAN is Active and the details are matching with PAN database.",
				    "success": true,
				    "ocr": {
				        "age": 28,
				        "date_of_birth": "1991-08-24",
				        "date_of_issue": null,
				        "fathers_name": "Sanjay Kumar Ahuja",
				        "id_number": "BILPA4762R",
				        "is_scanned": false,
				        "minor": false,
				        "name_on_card": "Karan Arilia",
				        "pan_type": "Individual"
				    },
				    "matched_information": {
				        "message": "PAN format is correct and PAN number is correct matching against DOB. However, Name is Incorrect.",
				        "id_match": true,
				        "name_match": false,
				        "dob_match": true
				    }
				}

**id_match** if id ocr and govt matched.

**name_match** if name ocr and govt matched.

**dob_match** if date of birth ocr and govt matched.

Voter ID
********

**success response**

.. code::
	  
	  {
		    "result": "id_found",
		    "message": "Id was found in the source",
		    "success": true,
		    "ocr": {
		        "address": "150, BLOCK- M, GURU HAR KISHAN NAGAR PASCHIM VIHAR, DELHI",
		        "age": null,
		        "date_of_birth": null,
		        "district": "DELHI",
		        "fathers_name": "Sanjay Kumar Ahuja",
		        "gender": "MALE",
		        "house_number": "150",
		        "id_number": "UBF2101764",
		        "is_scanned": null,
		        "name_on_card": "Karan Ahuja",
		        "pincode": null,
		        "state": "Delhi",
		        "street_address": "Block- M, Guru Har Kishan Nagar Paschim Vihar, Delhi",
		        "year_of_birth": null
		        },
		    "matched_information": {
		        "message": "OCR Data has been verified with government source",
		        "gender": "M",
		        "source": "NVSP",
		        "status": "id_found",
		        "name_match": 100,
		        "father_name_match": 100,
		        "street_address_match": 64.57571524439514,
		        "ocr_id_match": true
		    }
	  }

**gender** will be Government retrieved gender of person.

**name_match** will be percentage match  of scanned Name and government retrieved name.

**father_name_match** will be percentage match of scanned Name (father/husband or guardian) and government retrieved name.

**street_address_match** will be percentage match  of scanned address and government retrieved address. closer it is to 100, better result. Will rarely be 100.

Aadhaar
*******

**success response**

.. code::
		
	  {
	    "result": "id_found",
	    "message": "found and verified by UIDAI portal",
	    "success": true,
	    "information": "{\"Gender\": \"MALE\", \"State\": \"Delhi\", \"Mobile Number\": \"xxxxxxx447\", \"Age Band\": \"20-30\"}",
	    "ocr": {
	        "address": "S/O Sanjay Ahuja, House No-M 150, Guru karkishan Nagar, Paschim Vihar, Jwala Puri, We st Delhi, Delhi - 110087",
	        "date_of_birth": "1991-08-24",
	        "district": "DELHI",
	        "fathers_name": "Sanjay Ahuja",
	        "gender": "MALE",
	        "house_number": "M 150",
	        "id_number": "319*4429*458",
	        "is_scanned": "false",
	        "name_on_card": "Karan Ahuja",
	        "pincode": "110087",
	        "state": "Delhi",
	        "street_address": "Guru karkishan Nagar, Paschim Vihar, Jwala Puri, We st",
	        "year_of_birth": "1991"
	    },
	    "matched_information": {
	        "message": "OCR Data has been verified with government source",
	        "gender_match": true,
	        "state_match": true,
	        "age_match": true
	    }
	  }

**gender_match** will be true if gender from ocr and goverment are same.

**state_match** will be true if state from ocr and goverment are same.

**age_match** will be true if age from ocr and goverment are same.

Appendex 2
----------

**success response**

.. code::
		
	{  "person": 
	    {
	        "name": {
	            "first": "Karan",
	            "last": "Ahuja",
	            "middle": "Ahuja"
	        },
	        "documents": {
	          "ind_aadhaar": {
	            "result": {
	              "address": "S/O Sanjay Ahuja, House No-M 150, Guru karkishan Nagar, Paschim Vihar, Jwala Puri, We st Delhi, Delhi - 110087",
	              "date_of_birth": "1991-08-24",
	              "district": "DELHI",
	              "fathers_name": "Sanjay Ahuja",
	              "gender": "MALE",
	              "house_number": "M 150",
	              "id_number": "319644293458",
	              "is_scanned": "false",
	              "name_on_card": "Karan Ahuja",
	              "pincode": "110087",
	              "state": "Delhi",
	              "street_address": "Guru karkishan Nagar, Paschim Vihar, Jwala Puri, We st",
	              "year_of_birth": "1991"
	            },
	              "manualObj": null,
	              "status": "completed",
	              "faceMatched": false,
	              "matchResult": null,
	              "govResult": null,
	              "request_id": "1b247fa9-df84-463e-aa3f-899758ab6019",
	              "provider": "idfy",
	              "_id": "5df9fdfb71b57d2c188ebc63",
	              "docType": "ind_aadhaar",
	              "document1": "https://pdf-reports-springrole.s3.amazonaws.com/adhaar.png",
	              "belongsTo": "5df9fdf971b57d2c188ebc62",
	              "createdAt": "2019-12-18T10:22:59.917Z",
	              "updatedAt": "2019-12-18T10:23:01.167Z",
	              "__v": 0
	            },
	            "ind_driving_license": null,
	            "ind_pan": null,
	            "ind_voter_id": null,
	            "ind_passport": null
	        },
	        "selfie": {
	            "url": "https://pdf-reports-springrole.s3.amazonaws.com/face_image_1573552012776.jpg"
	        },
	        "hasConsent": false,
	        "phone": null,
	        "city": null,
	        "gender": null,
	        "s3_paths": null,
	        "_id": "5df9fdf971b57d2c188ebc62",
	        "addedBy": "5df88fc19ef1c83484090fb0",
	        "createdAt": "2019-12-18T10:22:49.563Z",
	        "updatedAt": "2019-12-18T10:23:00.299Z",
	        "__v": 0
	    }
	}

**Person** object will contain name and document objects

**document** object will contain one of the 5 supported document type (table in beginning of this document.

**result** object inside ind_*  object is the ==OCR result==

**govResult** is government result object, filled after government verification (5) api is called

**document1** is the link of document