# cURL

curl --location --request POST 'https://api.us.springverify.com/employee/consent' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer JWT_TOKEN' \
--header 'Content-Type: text/plain' \
--data-raw '{
     "summary_of_rights":true,
     "background_check":true,
     "report_checked":true,
     "full_name":"John James Doe"
}'

# node.js

var request = require('request');

var headers = {
    'Content-Type': 'text/plain',
    'Authorization': 'Bearer JWT_TOKEN'
};

var dataString = '{ "summary_of_rights":true, "background_check":true, "report_checked":true, "full_name":"John James Doe" }';

var options = {
    url: 'https://api.us.springverify.com/employee/consent',
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
$data = '{ "summary_of_rights":true, "background_check":true, "report_checked":true, "full_name":"John James Doe" }';
$response = Requests::post('https://api.us.springverify.com/employee/consent', $headers, $data);
