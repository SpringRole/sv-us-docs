# cURL

curl --location --request POST 'https://api.us.springverify.com/company/employee/search' \
--header 'Authorization: Bearer JWT_TOKEN' \
--header 'Content-Type: application/json' \
--data-raw '{
    "search":"johndoe@gmail.com"
}'

# node.js

var request = require('request');

var headers = {
    'Authorization': 'Bearer JWT_TOKEN',
    'Content-Type': 'application/json'
};

var dataString = '{ "search":"johndoe@gmail.com" }';

var options = {
    url: 'https://api.us.springverify.com/company/employee/search',
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
$data = '{ "search":"johndoe@gmail.com" }';
$response = Requests::post('https://api.us.springverify.com/company/employee/search', $headers, $data);
