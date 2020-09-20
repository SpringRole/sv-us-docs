# cURL

curl --location --request POST 'https://api.us.springverify.com/company/actions' \
--header 'Authorization: Bearer JWT_TOKEN' \
--header 'Content-Type: application/json' \
--data-raw '{
    "limit":10,
    "offset":0
}'

# node.js

var request = require('request');

var headers = {
    'Authorization': 'Bearer JWT_TOKEN',
    'Content-Type': 'application/json'
};

var dataString = '{ "limit":10, "offset":0 }';

var options = {
    url: 'https://api.us.springverify.com/company/actions',
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
$data = '{ "limit":10, "offset":0 }';
$response = Requests::post('https://api.us.springverify.com/company/actions', $headers, $data);
