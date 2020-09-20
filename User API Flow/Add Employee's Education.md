# cURL

curl --location --request POST 'https://api.us.springverify.com/employee/education' \
--header 'Content-Type: application/json' \
--data-raw '{
    "school_name": "MIT",
    "school_type": "University",
    "school_campus": "Boston",
    "address": "Asdasd",
    "city":"Boston",
    "state":"Massachusetts",
    "zip_code": "126112"
    "country":"USA",
    "start_date":"28-12-1991",
    "end_date":"12-28-1996",
    "degree":"Engineering",
    "currently_attending":"0",
    "completed_successfully":"1",
    "major":"Biotech"
}'

# node.js

var request = require('request');

var headers = {
    'Content-Type': 'application/json'
};

var dataString = '{ "school_name": "MIT", "school_type": "University", "school_campus": "Boston", "address": "Asdasd", "city":"Boston", "state":"Massachusetts", "zip_code": "126112" "country":"USA", "start_date":"28-12-1991", "end_date":"12-28-1996", "degree":"Engineering", "currently_attending":"0", "completed_successfully":"1", "major":"Biotech" }';

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
$data = '{ "school_name": "MIT", "school_type": "University", "school_campus": "Boston", "address": "Asdasd", "city":"Boston", "state":"Massachusetts", "zip_code": "126112" "country":"USA", "start_date":"28-12-1991", "end_date":"12-28-1996", "degree":"Engineering", "currently_attending":"0", "completed_successfully":"1", "major":"Biotech" }';
$response = Requests::post('https://api.us.springverify.com/employee/education', $headers, $data);
