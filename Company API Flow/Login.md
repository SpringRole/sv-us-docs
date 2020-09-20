# cURL

curl --location --request POST 'https://api.us.springverify.com/auth/login' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer JWT_TOKEN' \
--data-raw '{
    "email": "john@wick.com",
    "password": "daaad6e5604e8e17bd9f108d91e26afe6281dac8fda0091040a7a6d7bd9b43b5",
    "role":"admin"
}'

# node.js

var request = require('request');

var headers = {
    'Content-Type': 'application/json',
    'Authorization': 'Bearer JWT_TOKEN'
};

var dataString = '{ "email": "john@wick.com", "password": "daaad6e5604e8e17bd9f108d91e26afe6281dac8fda0091040a7a6d7bd9b43b5", "role":"admin" }';

var options = {
    url: 'https://api.us.springverify.com/auth/login',
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
$data = '{ "email": "john@wick.com", "password": "daaad6e5604e8e17bd9f108d91e26afe6281dac8fda0091040a7a6d7bd9b43b5", "role":"admin" }';
$response = Requests::post('https://api.us.springverify.com/auth/login', $headers, $data);
