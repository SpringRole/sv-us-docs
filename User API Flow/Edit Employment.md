# cURL

curl --location --request POST 'https://api.us.springverify.com/employee/employment' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer JWT_TOKEN' \
--header 'Content-Type: text/plain' \
--data-raw '{
    "id": "ef9e7612-3789-405a-be37-c7bc1871c1f7",
    "employer_name": "Stark Industries pvt ltd",
    "employer_address": "12, Manhattan Street",
    "employer_phone": "9911991199",
    "employer_town": "New York",
    "state": "New York",
    "zip_code": "129012",
    "country": "USA",
    "job_title": "Senior Manager",
    "start_date": "19-11-2000",
    "end_date": "19-11-2002",
    "supervisor_name": "Nick Fury",
    "current_employment": "0",
    "job_type": "Contract",
    "reason_for_leaving": "xyz",
    "supervisor_contact": "nickfury@starkindustries.com"
}'

# node.js

var request = require('request');

var headers = {
    'Content-Type': 'text/plain',
    'Authorization': 'Bearer JWT_TOKEN'
};

var dataString = '{ "id": "ef9e7612-3789-405a-be37-c7bc1871c1f7", "employer_name": "Stark Industries pvt ltd", "employer_address": "12, Manhattan Street", "employer_phone": "9911991199", "employer_town": "New York", "state": "New York", "zip_code": "129012", "country": "USA", "job_title": "Senior Manager", "start_date": "19-11-2000", "end_date": "19-11-2002", "supervisor_name": "Nick Fury", "current_employment": "0", "job_type": "Contract", "reason_for_leaving": "xyz", "supervisor_contact": "nickfury@starkindustries.com" }';

var options = {
    url: 'https://api.us.springverify.com/employee/employment',
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
$data = '{ "id": "ef9e7612-3789-405a-be37-c7bc1871c1f7", "employer_name": "Stark Industries pvt ltd", "employer_address": "12, Manhattan Street", "employer_phone": "9911991199", "employer_town": "New York", "state": "New York", "zip_code": "129012", "country": "USA", "job_title": "Senior Manager", "start_date": "19-11-2000", "end_date": "19-11-2002", "supervisor_name": "Nick Fury", "current_employment": "0", "job_type": "Contract", "reason_for_leaving": "xyz", "supervisor_contact": "nickfury@starkindustries.com" }';
$response = Requests::post('https://api.us.springverify.com/employee/employment', $headers, $data);
