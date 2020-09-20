# cURL

curl --location --request POST 'https://api.us.springverify.com/auth/resend-email' \
--header 'Content-Type: application/json' \
--data-raw '{
    "email":"john@wick.com"
}'

# node.js

var request = require('request');

var headers = {
    'Content-Type': 'application/json'
};

var dataString = '{ "email":"john@wick.com" }';

var options = {
    url: 'https://api.us.springverify.com/auth/resend-email',
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
$data = '{ "email":"john@wick.com" }';
$response = Requests::post('https://api.us.springverify.com/auth/resend-email', $headers, $data);
