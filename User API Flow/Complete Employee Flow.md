# cURL

curl --location --request GET 'https://api.us.springverify.com/employee/flow-completed' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer JWT_TOKEN' \
--header 'Content-Type: text/plain' \

# node.js

var request = require('request');

var headers = {
    'Content-Type': 'text/plain',
    'Authorization': 'Bearer JWT_TOKEN'
};

var options = {
    url: 'https://api.us.springverify.com/employee/flow-completed',
    headers: headers
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
$response = Requests::get('https://api.us.springverify.com/employee/flow-completed', $headers);
