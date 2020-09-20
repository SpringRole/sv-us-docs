# cURL

curl --location --request GET 'https://api.us.springverify.com/company/action/adverse?adverse_action_id=2504d7c7-3f68-47a7-b9dc-be2adb99936e' \
--header 'Authorization: Bearer JWT_TOKEN' \
--header 'Content-Type: application/json'

# node.js

var request = require('request');

var headers = {
    'Authorization': 'Bearer JWT_TOKEN',
    'Content-Type': 'application/json'
};

var options = {
    url: 'https://api.us.springverify.com/company/action/adverse?adverse_action_id=2504d7c7-3f68-47a7-b9dc-be2adb99936e',
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
    'Authorization' => 'Bearer JWT_TOKEN',
    'Content-Type' => 'application/json'
);
$response = Requests::get('https://api.us.springverify.com/company/action/adverse?adverse_action_id=2504d7c7-3f68-47a7-b9dc-be2adb99936e', $headers);
