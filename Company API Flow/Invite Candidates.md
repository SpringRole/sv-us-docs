# cURL

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

# node.js

var request = require('request');

var headers = {
    'Content-Type': 'application/json',
    'Authorization': 'Bearer JWT_TOKEN'
};

var dataString = '{ "email_list": ["johndoe@gmail.com"], "package": "diamond", "add_ons": { "employment": 2, "education": 1, "license": 0, "driving_license": 0, "civil_court": 1, "all_county_criminal_search": true, "county_criminal_search": 0 }, coupon_code:"" }';

var options = {
    url: 'localhost:3080/employee/invite',
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
    'Content-Type' => 'application/json',
    'Authorization' => 'Bearer JWT_TOKEN'
);
$data = '{ "email_list": ["johndoe@gmail.com"], "package": "diamond", "add_ons": { "employment": 2, "education": 1, "license": 0, "driving_license": 0, "civil_court": 1, "all_county_criminal_search": true, "county_criminal_search": 0 }, coupon_code:"" }';
$response = Requests::post('localhost:3080/employee/invite', $headers, $data);
