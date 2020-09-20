# cURL

curl --location --request POST 'https://api.us.springverify.com/auth/forgot-password' \
--header 'Content-Type: application/json' \
--data-raw '{
    "email":"johndoe@gmail.com"
}

# node.js

var request = require('request');

var headers = {
    'Content-Type': 'application/json'
};

var dataString = '{ "email":"johndoe@gmail.com" }';

var options = {
    url: 'https://api.us.springverify.com/auth/forgot-password',
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
$data = '{ "email":"johndoe@gmail.com" }';
$response = Requests::post('https://api.us.springverify.com/auth/forgot-password', $headers, $data);
