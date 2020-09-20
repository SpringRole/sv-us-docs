# cURL

curl --location --request POST 'https://api.us.springverify.com/employee/kba/verify' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer JWT_TOKEN' \
--header 'Content-Type: text/plain' \
--data-raw '{
"qna":[
    {
      "question_id": 1,
      "answer_id": 5
    },{
      "question_id": 2,
      "answer_id": 5
    },{
      "question_id": 3,
      "answer_id": 5
    },{
      "question_id": 4,
      "answer_id": 5
    },{
      "question_id": 5,
      "answer_id": 5
    }
  ]
}'

# node.js

var request = require('request');

var headers = {
    'Content-Type': 'text/plain',
    'Authorization': 'Bearer JWT_TOKEN'
};

var dataString = '{ "qna":[ { "question_id": 1, "answer_id": 5 },{ "question_id": 2, "answer_id": 5 },{ "question_id": 3, "answer_id": 5 },{ "question_id": 4, "answer_id": 5 },{ "question_id": 5, "answer_id": 5 } ] }';

var options = {
    url: 'https://api.us.springverify.com/employee/kba/verify',
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
$data = '{ "qna":[ { "question_id": 1, "answer_id": 5 },{ "question_id": 2, "answer_id": 5 },{ "question_id": 3, "answer_id": 5 },{ "question_id": 4, "answer_id": 5 },{ "question_id": 5, "answer_id": 5 } ] }';
$response = Requests::post('https://api.us.springverify.com/employee/kba/verify', $headers, $data);
