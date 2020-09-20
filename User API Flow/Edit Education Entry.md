# cURL

curl --location --request POST 'https://api.us.springverify.com/employee/education' \
--header 'Content-Type: application/json' \
--data-raw '{
    "id":"de359912-1223-4338-87c4-0783a0ea495b",
    "school_name": "MIT",
    "school_type": "University",
    "school_campus": "Boston",
    "address":"New street east block",
    "city":"Boston",
    "state":"Massachusetts",
    "zip_code": "129012",
    "country":"USA",
    "start_date": "28-12-1991",
   "end_date": "12-28-1996",
    "degree":"Engineering",
    "currently_attending":"0",
    "completed_successfully":"1",
    "major":"Biotech"
}'

# mode.js

var request = require('request');

var headers = {
    'Content-Type': 'application/json'
};

var dataString = '{ "id":"de359912-1223-4338-87c4-0783a0ea495b", "school_name": "MIT", "school_type": "University", "school_campus": "Boston", "address":"New street east block", "city":"Boston", "state":"Massachusetts", "zip_code": "129012", "country":"USA", "start_date": "28-12-1991", "end_date": "12-28-1996", "degree":"Engineering", "currently_attending":"0", "completed_successfully":"1", "major":"Biotech" }';

var options = {
    url: 'https://api.us.springverify.com/employee/education',
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
    'Content-Type' => 'application/json'
);
$data = '{ "id":"de359912-1223-4338-87c4-0783a0ea495b", "school_name": "MIT", "school_type": "University", "school_campus": "Boston", "address":"New street east block", "city":"Boston", "state":"Massachusetts", "zip_code": "129012", "country":"USA", "start_date": "28-12-1991", "end_date": "12-28-1996", "degree":"Engineering", "currently_attending":"0", "completed_successfully":"1", "major":"Biotech" }';
$response = Requests::post('https://api.us.springverify.com/employee/education', $headers, $data);
