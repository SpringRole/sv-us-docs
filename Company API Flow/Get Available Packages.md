# cURL

curl --location --request GET 'http://api-stage.us.springverify.com/company/package' \
        --header 'Authorization: Bearer JWT_TOKEN'

# node.js

var request = require('request');

var headers = {
    'Authorization': 'Bearer JWT_TOKEN'
};

var options = {
    url: 'http://api-stage.us.springverify.com/company/package',
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
$response = Requests::get('http://api-stage.us.springverify.com/company/package', $headers);
