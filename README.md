# Laravel Influxdb

A service made to provide, set up and use [influxdb-php](https://github.com/influxdata/influxdb-php/) in Laravel 5.6

## Installation

Follow this steps to use this package on your Laravel installation

### 1. Require it on composer

```bash
composer require power-data-hub/laraflux
```

The package will automatically register it's service provider.

For Laravel <= 5.4 add the provider manually

### 2. Load service provider (optional Laravel <= 5.4 only)

You need to update your `config/app.php` configuration file to register our service provider, adding this line on `providers` array:

```php
PowerDataHub\Laraflux\ServiceProvider::class
```

```
'aliases' => [
//  ...
    'Laraflux' => PowerDataHub\Laraflux\Facades\Laraflux::class,
]
```

* Define `.env` variables to connect to InfluxDB

```ini
INFLUXDB_HOST=localhost
INFLUXDB_PORT=8086
INFLUXDB_USER=some_user
INFLUXDB_PASSWORD=some_password
INFLUXDB_SSL=false
INFLUXDB_VERIFYSSL=false
INFLUXDB_TIMEOUT=0
INFLUXDB_DBNAME=some_database
```

* Write this into your terminal inside your project

```ini
php artisan vendor:publish
```

## Reading Data

```php
<?php

// executing a query will yield a resultset object
$result = Laraflux::query('select * from test_metric LIMIT 5');

// get the points from the resultset yields an array
$points = $result->getPoints();
```

## Writing Data

```php
<?php

// create an array of points
$points = [
    new \InfluxDB\Point(
        'test_metric', // name of the measurement
        null, // the measurement value
        ['host' => 'server01', 'region' => 'us-west'], // optional tags
        ['cpucount' => 10], // optional additional fields
        time() // Time precision has to be set to seconds!
    ),
    new \InfluxDB\Point(
        'test_metric', // name of the measurement
        null, // the measurement value
        ['host' => 'server01', 'region' => 'us-west'], // optional tags
        ['cpucount' => 10], // optional additional fields
        time() // Time precision has to be set to seconds!
    )
];

$result = Laraflux::writePoints($points, \InfluxDB\Database::PRECISION_SECONDS);
```

License
----

This project is licensed under the MIT License
