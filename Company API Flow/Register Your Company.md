# cURL

curl --location --request POST 'https://api.us.springverify.com/company' \
--header 'Authorization: Bearer JWT_TOKEN' \
--header 'Content-Type: application/json' \
--data-raw '{
    "name": "SmartBrains",
    "address": "901, Downtown Manhattan",
    "city": "NY",
    "state": "NY",
    "zipcode": "91319",
    "tax_id_number":"1234567890"
}'

# node.js

var request = require('request');

var headers = {
    'Authorization': 'Bearer JWT_TOKEN',
    'Content-Type': 'application/json'
};

var dataString = '{ "name": "SmartBrains", "address": "901, Downtown Manhattan", "city": "NY", "state": "NY", "zipcode": "91319", "tax_id_number":"1234567890" }';

var options = {
    url: 'https://api.us.springverify.com/company',
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
    'Authorization' => 'Bearer JWT_TOKEN',
    'Content-Type' => 'application/json'
);
$data = '{ "name": "SmartBrains", "address": "901, Downtown Manhattan", "city": "NY", "state": "NY", "zipcode": "91319", "tax_id_number":"1234567890" }';
$response = Requests::post('https://api.us.springverify.com/company', $headers, $data);
