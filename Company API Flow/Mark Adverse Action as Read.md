# cURL

curl --location --request PUT 'http://localhost:3080/company/action/adverse/mark-read' \
--header 'Authorization: Bearer JWT_TOKEN' \
--header 'Content-Type: application/json' \
--data-raw '{
    "IDs":["2504d7c7-3f68-47a7-b9dc-be2adb99936e"]
}'

# node.js

var request = require('request');

var headers = {
    'Authorization': 'Bearer JWT_TOKEN',
    'Content-Type': 'application/json'
};

var dataString = '{ "IDs":["2504d7c7-3f68-47a7-b9dc-be2adb99936e"] }';

var options = {
    url: 'http://localhost:3080/company/action/adverse/mark-read',
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
$data = '{ "IDs":["2504d7c7-3f68-47a7-b9dc-be2adb99936e"] }';
$response = Requests::post('http://localhost:3080/company/action/adverse/mark-read', $headers, $data);
