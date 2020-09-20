# cURL

curl --location --request POST 'https://api.us.springverify.com/profile/reset-password' \
--header 'Authorization: Bearer JWT_TOKEN' \
--header 'Content-Type: application/json' \
--data-raw '{
    "password":"17f80754644d33ac685b0842a402229adbb43fc9312f7bdf36ba24237a1f1ffb"
}'

# node.js

var request = require('request');

var headers = {
    'Authorization': 'Bearer JWT_TOKEN',
    'Content-Type': 'application/json'
};

var dataString = '{ "password":"17f80754644d33ac685b0842a402229adbb43fc9312f7bdf36ba24237a1f1ffb" }';

var options = {
    url: 'https://api.us.springverify.com/profile/reset-password',
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
$data = '{ "password":"17f80754644d33ac685b0842a402229adbb43fc9312f7bdf36ba24237a1f1ffb" }';
$response = Requests::post('https://api.us.springverify.com/profile/reset-password', $headers, $data);
