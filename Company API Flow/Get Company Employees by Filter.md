# cURL

curl --location --request POST 'https://api.us.springverify.com/company/employees' \
--header 'Authorization: Bearer JWT_TOKEN' \
--header 'Content-Type: application/json' \
--data-raw '{
    "limit":1,
    "offset":0,
    "filter":"NULL"
}'

# node.js

var request = require('request');

var headers = {
    'Authorization': 'Bearer JWT_TOKEN',
    'Content-Type': 'application/json'
};

var dataString = '{ "limit":1, "offset":0, "filter":"NULL" }';

var options = {
    url: 'https://api.us.springverify.com/company/employees',
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
$data = '{ "limit":1, "offset":0, "filter":"NULL" }';
$response = Requests::post('https://api.us.springverify.com/company/employees', $headers, $data);
