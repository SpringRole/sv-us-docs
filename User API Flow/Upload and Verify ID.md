# cURL

curl --location --request POST 'https://api.us.springverify.com/employee/upload/id' \
--header 'Authorization: Bearer JWT_TOKEN' \
--form 'front=@/path/to/file' \
--form 'back=@/path/to/file' \

# node.js

var request = require('request');

var headers = {
    'Authorization': 'Bearer JWT_TOKEN'
};

var options = {
    url: 'https://api.us.springverify.com/employee/upload/id',
    method: 'POST',
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
    'Authorization' => 'Bearer JWT_TOKEN'
);
$response = Requests::post('https://api.us.springverify.com/employee/upload/id', $headers);
