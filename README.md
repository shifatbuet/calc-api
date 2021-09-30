# RestAPI - Based on Laravel with Docker

Laravel 6.0 Restful API with:
<small>
 - api;
 - json responses;
 - phpunit tests;
 </small>

## Requirements

As it is build on the Laravel framework, it has a few system requirements.<br>
Of course, all of these are satisfied by the Docker, so it's highly recommended that you use Docker as
 your local Laravel development environment.
 
However, if you are not using Docker, you will need to make sure your server meets the following requirements:
- PHP >= 7.1.3
- Composer
- OpenSSL PHP Extension
- PDO PHP Extension
- Mbstring PHP Extension
- Tokenizer PHP Extension
- XML PHP Extension
- Ctype PHP Extension
- JSON PHP Extension
- BCMath PHP Extension

You can check all the laravel related dependecies [here](https://laravel.com/docs/5.7/installation#server-requirements).

## Install application.

1. Clone repository and setup.<br>
`git clone git@github.com:shifatbuet/rest-api.git`<br>
`cd rest-api`<br>
`cp .env.example .env`<br>
2. Start docker.<br>
`docker-compose up -d`
3. Install needed packages.<br>
`docker exec php-fpm composer install`<br>

4. Storage log permission (sometimes an issue). <br> 
`sudo chmod -R 777 storage/logs/ `<br>
5. Generate key.<br>
`docker exec php-fpm php artisan key:generate`<br>

<small>This way is to setup app with docker, but if you want use it without docker just skip second step and replace
 from commands `docker exec php-fpm` part. For example 3 step without Docker should look like:<br>
 `composer install`</small>
<br>

## API Endpoints and Routes

```
+-----------+----------------------------+-----------------+--------------------------------------------------+--------------+
| Method    | URI                        | Name      | Action                                                | Middleware    |
+-----------+----------------------------+-----------------+--------------------------------------------------+--------------+
| GET|HEAD  | /                          |           | Closure                                               | web           |
+-----------+----------------------------+-----------------+--------------------------------------------------+--------------+
| POST      | api/calculate              |           | App\Http\Controllers\CalculatorController@calculate   | api           |
+-----------+----------------------------+-----------------+--------------------------------------------------+--------------+
```

#### Example calculate request

- For Linux : 
```bash
curl -d "input_1=1&input_2=2&operation=add" -X POST http://localhost:88/api/calculate
```

#### Example calculate response

```json
{
"success":true,
"data":3 
}
```

#### Example authorized request with token

```bash
curl --location --request GET "http://localhost:8080/api/profile" \
  --header "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImp0aSI6IjM1ZjQ5NTU1N2QzYmEyNjM5OTQzYmQzY2JlMTU1MTI0NmIyZmJiMzc4NGY2ZTJmNTgwOGZlM2Y3YjNiMGM0ZjlkZTM2MzY2ZGNkZTFhOWJjIn0.eyJhdWQiOiIxIiwianRpIjoiMzVmNDk1NTU3ZDNiYTI2Mzk5NDNiZDNjYmUxNTUxMjQ2YjJmYmIzNzg0ZjZlMmY1ODA4ZmUzZjdiM2IwYzRmOWRlMzYzNjZkY2RlMWE5YmMiLCJpYXQiOjE1NjYyNjQ2NjksIm5iZiI6MTU2NjI2NDY2OSwiZXhwIjoxNTk3ODg3MDY5LCJzdWIiOiIyIiwic2NvcGVzIjpbXX0.Xf7HGFczhfbXNMtycA3k9O-0FYYkH4Oj-LLJQm4P5br5DwJOf0ScQ0Gb5wKA2k6T1QuYNy6D4h_wGzCUICuvmFHyQ0pTlkUL_2RjefjHy1mIuTPCUkgxFVjAn0qUIBgakrb_I2OXGgRZlO_00eyYRiSaMBtcuEkRljVpJG8EL8JDdTFFEqgoSXrGDfQqubBEW0_IjDw33NXrFVtK-aJQdkTpGNUwr0aSVMT_GcX7u7vJjvCa3Jc50unGXZI6VDpwxsvndAyuuvu8AbRhmN7TNJhCNynT55m4X0ZY9xLH_WAEoT7uI5ei5DdBAfH1_Ux0nJxHLFGaXI15N_OAw8noVgPSPps8Bbn9fsWu7ZqGvj-2gUupVrWS1FVk5qBYzKnV4Osdsl0pRjVfY9yghIRTCGonxCU_A2Fl-i8OUVPUl6iqGxiko2KBo8qqLWQbT3IkUXyB578DAIqgqfZ9gr122B0J5ukyIYtLLHKo0HqYc9NjB2K11ntfB6SPXJBEY6Xrts8xm_0uT1fJ2pIXuvICtC-cUBDioUZ842ijeOJm4h_vESzsqJXQ4Xp32PCvcjHxO9X9EJe5JnDdHD5nlVDeEl4ZtpBZ4zZeef3yXsdMPxwGGiTrwNMZyLHnzWdiNAhZfo13Pk4z9XswDK8omIdtKuEPAIQXT2Z9Gu4keVaI0fQ" \
  --header "Content-Type: application/json" \
  --header "X-Requested-With: XMLHttpRequest" \
  --data ""
```

## Tests

Used a library for printing the result in pretty format `codedungeon/phpunit-result-printer`.

- All the success, failure test cases checked in phpunit: 

```bash
./vendor/phpunit/phpunit/phpunit tests/Feature/CalculatorTest.php
```
- Output will be something like followings: 

```
PHPUnit 8.5.21 by Sebastian Bergmann and contributors.

 ==> CalculatorTest             ✔  ✔  ✔  ✔  ✔  ✔  ✔  ✔  ✔  ✔  ✔  ✔  ✔  

Time: 632 ms, Memory: 20.00 MB

OK (13 tests, 26 assertions)

```

## Logs

- For checking logs: 

```bash
tail -f storage/logs/laravel-"`date +'%Y-%m-%d'`".log
```

- Log output at a glimpse without errors

```
[2021-09-30 05:38:10] testing.ERROR: Exception occurred in operation . {"input_1":1,"input_2":0,"operator":null} 
[2021-09-30 05:38:35] testing.INFO: Calculating the inputs based on operation . {"input_1":1,"input_2":2,"operator":"add"} 
[2021-09-30 05:38:35] testing.INFO: Executing addition . {"input_1":1,"input_2":2} 
[2021-09-30 05:38:35] testing.INFO: Calculating the inputs based on operation . {"input_1":1,"input_2":1,"operator":"add"} 
[2021-09-30 05:38:35] testing.INFO: Executing addition . {"input_1":1,"input_2":1} 
[2021-09-30 05:38:35] testing.INFO: Calculating the inputs based on operation . {"input_1":10,"input_2":1,"operator":"subtract"} 
[2021-09-30 05:38:35] testing.INFO: Executing subtraction . {"input_1":10,"input_2":1} 
[2021-09-30 05:38:35] testing.INFO: Calculating the inputs based on operation . {"input_1":1,"input_2":1,"operator":"subtract"} 
[2021-09-30 05:38:35] testing.INFO: Executing subtraction . {"input_1":1,"input_2":1} 
[2021-09-30 05:38:35] testing.INFO: Calculating the inputs based on operation . {"input_1":10,"input_2":1,"operator":"multiply"} 
[2021-09-30 05:38:35] testing.INFO: Executing multiplication . {"input_1":10,"input_2":1} 
[2021-09-30 05:38:35] testing.INFO: Calculating the inputs based on operation . {"input_1":1,"input_2":1,"operator":"multiply"} 
[2021-09-30 05:38:35] testing.INFO: Executing multiplication . {"input_1":1,"input_2":1} 
[2021-09-30 05:38:35] testing.INFO: Calculating the inputs based on operation . {"input_1":10,"input_2":1,"operator":"divide"} 
[2021-09-30 05:38:35] testing.INFO: Executing division . {"input_1":10,"input_2":1} 
[2021-09-30 05:38:35] testing.INFO: Calculating the inputs based on operation . {"input_1":1,"input_2":1,"operator":"divide"} 
[2021-09-30 05:38:35] testing.INFO: Executing division . {"input_1":1,"input_2":1} 
[2021-09-30 05:38:35] testing.ERROR: Exception occurred in operation . {"input_1":"wrong_data","input_2":2,"operator":null} 
[2021-09-30 05:38:35] testing.ERROR: Exception occurred in operation . {"input_1":1,"input_2":"wrong_data","operator":null} 
[2021-09-30 05:38:35] testing.ERROR: Exception occurred in operation . {"input_1":10,"input_2":1,"operator":null} 
[2021-09-30 05:38:35] testing.ERROR: Exception occurred in operation . {"input_1":1,"input_2":1,"operator":null} 
```

## Format code

- Find out the files that needs to be fixed :
```sudo docker exec php-fpm composer sniff ```

- Fix all the code format : 
```sudo docker exec php-fpm composer lint ```
