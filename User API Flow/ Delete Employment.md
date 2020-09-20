# cURL

curl --location --request DELETE 'https://api.us.springverify.com/employee/employment?id=ef9e7612-3789-405a-be37-c7bc1871c1f7' \
--header 'Authorization: Bearer JWT_TOKEN'

# node.js

var request = require('request');

var headers = {
    'Authorization': 'Bearer JWT_TOKEN'
};

var options = {
    url: 'https://api.us.springverify.com/employee/employment?id=ef9e7612-3789-405a-be37-c7bc1871c1f7',
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
$response = Requests::get('https://api.us.springverify.com/employee/employment?id=ef9e7612-3789-405a-be37-c7bc1871c1f7', $headers);
