# cURL

curl --location --request POST 'https://api.us.springverify.com/auth/signup' \
--header 'Content-Type: application/json' \
--data-raw '{
    "first_name": "John",
    "last_name": "Wick",
    "phone": "999999999",
    "email": "john@wick.com",
    "password": "daaad6e5604e8e17bd9f108d91e26afe6281dac8fda0091040a7a6d7bd9b43b5",
    "confirm_password":"daaad6e5604e8e17bd9f108d91e26afe6281dac8fda0091040a7a6d7bd9b43b5",
    "domain": "wick.com"
}'

# node.js

var request = require('request');

var headers = {
    'Content-Type': 'application/json'
};

var dataString = '{ "first_name": "John", "last_name": "Wick", "phone": "999999999", "email": "john@wick.com", "password": "daaad6e5604e8e17bd9f108d91e26afe6281dac8fda0091040a7a6d7bd9b43b5", "confirm_password":"daaad6e5604e8e17bd9f108d91e26afe6281dac8fda0091040a7a6d7bd9b43b5", "domain": "wick.com" }';

var options = {
    url: 'https://api.us.springverify.com/auth/signup',
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
$data = '{ "first_name": "John", "last_name": "Wick", "phone": "999999999", "email": "john@wick.com", "password": "daaad6e5604e8e17bd9f108d91e26afe6281dac8fda0091040a7a6d7bd9b43b5", "confirm_password":"daaad6e5604e8e17bd9f108d91e26afe6281dac8fda0091040a7a6d7bd9b43b5", "domain": "wick.com" }';
$response = Requests::post('https://api.us.springverify.com/auth/signup', $headers, $data);
