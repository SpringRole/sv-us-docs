# cURL

curl --location --request GET 'https://api.us.springverify.com/employee/id/manual-review' \
--header 'Authorization: Bearer JWT_TOKEN'

# node.js

var request = require('request');

var headers = {
    'Authorization': 'Bearer JWT_TOKEN'
};

var options = {
    url: 'https://api.us.springverify.com/employee/id/manual-review',
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
$response = Requests::get('https://api.us.springverify.com/employee/id/manual-review', $headers);
