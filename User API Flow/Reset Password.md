# cURL

curl --location --request POST 'https://api.us.springverify.com/employee/reset-password' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer JWT_TOKEN' \
--header 'Content-Type: text/plain' \
--data-raw '{
    "password":"ab0365e1c40ea17c8d1e7819c45a477ac836080b3b5fd1305ccb281acf24c62e"
}'

# node.js

var request = require('request');

var headers = {
    'Content-Type': 'text/plain',
    'Authorization': 'Bearer JWT_TOKEN'
};

var dataString = '{ "password":"ab0365e1c40ea17c8d1e7819c45a477ac836080b3b5fd1305ccb281acf24c62e" }';

var options = {
    url: 'https://api.us.springverify.com/employee/reset-password',
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
    'Content-Type' => 'text/plain',
    'Authorization' => 'Bearer JWT_TOKEN'
);
$data = '{ "password":"ab0365e1c40ea17c8d1e7819c45a477ac836080b3b5fd1305ccb281acf24c62e" }';
$response = Requests::post('https://api.us.springverify.com/employee/reset-password', $headers, $data);
