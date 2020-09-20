# cURL

curl --location --request POST 'https://api.us.springverify.com/payment/charge-user' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer JWT_TOKEN' \
--data-raw '{
    "id": "2612d112-5827-4b1c-a177-03a85eabd03d",
    "send_email": true
}'

# node.js

var request = require('request');

var headers = {
    'Content-Type': 'application/json',
    'Authorization': 'Bearer JWT_TOKEN'
};

var dataString = '{ "id": "2612d112-5827-4b1c-a177-03a85eabd03d", "send_email": true }';

var options = {
    url: 'https://api.us.springverify.com/payment/charge-user',
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
$data = '{ "id": "2612d112-5827-4b1c-a177-03a85eabd03d", "send_email": true }';
$response = Requests::post('https://api.us.springverify.com/payment/charge-user', $headers, $data);
