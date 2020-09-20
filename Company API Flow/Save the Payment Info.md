# cURL

curl --location --request POST 'https://api.us.springverify.com/payment/save-card' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer JWT_TOKEN' \
--data-raw '{
    "source":"tok_1EkXOa4wweuFtc0nQd0eOOhs"
}'

# node.js

var request = require('request');

var headers = {
    'Content-Type': 'application/json',
    'Authorization': 'Bearer JWT_TOKEN'
};

var dataString = '{ "source":"tok_1EkXOa4wweuFtc0nQd0eOOhs" }';

var options = {
    url: 'https://api.us.springverify.com/payment/save-card',
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
$data = '{ "source":"tok_1EkXOa4wweuFtc0nQd0eOOhs" }';
$response = Requests::post('https://api.us.springverify.com/payment/save-card', $headers, $data);
