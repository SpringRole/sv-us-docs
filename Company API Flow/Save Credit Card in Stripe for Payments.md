# cURL

curl --location --request POST 'https://api.stripe.com/v1/tokens' \
--header 'Authorization: Bearer STRIPE_TOKEN' \
--header 'Content-Type: application/x-www-form-urlencoded' \
--data-urlencode 'card[number]=4242424242424242' \
--data-urlencode 'card[exp_month]=12' \
--data-urlencode 'card[exp_year]=2020' \
--data-urlencode 'card[cvc]=123'

# node.js

var request = require('request');

var headers = {
    'Authorization': 'Bearer STRIPE_TOKEN',
    'Content-Type': 'application/x-www-form-urlencoded'
};

var options = {
    url: 'https://api.stripe.com/v1/tokens',
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
    'Authorization' => 'Bearer STRIPE_TOKEN',
    'Content-Type' => 'application/x-www-form-urlencoded'
);
$response = Requests::get('https://api.stripe.com/v1/tokens', $headers);
