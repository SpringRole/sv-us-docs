# cURL

curl --location --request GET 'http://localhost:3080/company/criminal/sjv/report?sjv_id=0db2b92a-ec0d-4506-a356-fcb97f31e0c3' \
--header 'Authorization: Bearer JWT_TOKEN'

# node.js

var request = require('request');

var headers = {
    'Authorization': 'Bearer JWT_TOKEN'
};

var options = {
    url: 'http://localhost:3080/company/criminal/sjv/report?sjv_id=0db2b92a-ec0d-4506-a356-fcb97f31e0c3',
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
$response = Requests::get('http://localhost:3080/company/criminal/sjv/report?sjv_id=0db2b92a-ec0d-4506-a356-fcb97f31e0c3', $headers);
