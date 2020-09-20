# cURL

curl --location --request PUT 'https://api.us.springverify.com/employee/personal-details' \
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
    "street_name": "A clear street"
    "address":"236 A clear street",
    "city":"Gotham",
    "state":"CA",
    "zip_code":"33433",
    "phone":"56-999222992"
}'

# node.js

var request = require('request');

var headers = {
    'Content-Type': 'text/plain',
    'Authorization': 'Bearer JWT_TOKEN'
};

var dataString = '{ "first_name":"John", "middle_name":"David", "last_name":"Doe", "dob":"12-11-1980", "ssn":"123456789", "email":"johndoe@gmail.com", "house_number": "239", "street_name": "A clear street" "address":"236 A clear street", "city":"Gotham", "state":"CA", "zip_code":"33433", "phone":"56-999222992" }';

var options = {
    url: 'https://api.us.springverify.com/employee/personal-details',
    method: 'POST',
    headers: headers,
    body: dataString
};

function callback(error, response, body) {
    if (!error && response.statusCode == 200) {
        console.log(body);
    }
}

request(options, callback);

# PHP

<?php
include('vendor/rmccue/requests/library/Requests.php');
Requests::register_autoloader();
$headers = array(
    'Content-Type' => 'text/plain',
    'Authorization' => 'Bearer JWT_TOKEN'
);
$data = '{ "first_name":"John", "middle_name":"David", "last_name":"Doe", "dob":"12-11-1980", "ssn":"123456789", "email":"johndoe@gmail.com", "house_number": "239", "street_name": "A clear street" "address":"236 A clear street", "city":"Gotham", "state":"CA", "zip_code":"33433", "phone":"56-999222992" }';
$response = Requests::post('https://api.us.springverify.com/employee/personal-details', $headers, $data);
