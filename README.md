# influxdb-bundle

Integration bundle for writing to influxdb

[![PHP Version](https://img.shields.io/badge/PHP-%3E%3D7.0-blue.svg)](https://img.shields.io/badge/PHP-%3E%3D7.0-blue.svg) [![Build Status](https://travis-ci.org/Algatux/influxdb-bundle.svg?branch=master)](https://travis-ci.org/Algatux/influxdb-bundle) [![Scrutinizer Code Quality](https://scrutinizer-ci.com/g/Algatux/influxdb-bundle/badges/quality-score.png?b=master)](https://scrutinizer-ci.com/g/Algatux/influxdb-bundle/?branch=master) [![Code Coverage](https://scrutinizer-ci.com/g/Algatux/influxdb-bundle/badges/coverage.png?b=master)](https://scrutinizer-ci.com/g/Algatux/influxdb-bundle/?branch=master)
[![Latest Stable Version](https://poser.pugx.org/algatux/influxdb-bundle/v/stable)](https://packagist.org/packages/algatux/influxdb-bundle) [![Latest Unstable Version](https://poser.pugx.org/algatux/influxdb-bundle/v/unstable)](https://packagist.org/packages/algatux/influxdb-bundle) [![License](https://poser.pugx.org/algatux/influxdb-bundle/license)](https://packagist.org/packages/algatux/influxdb-bundle)

### Install using Composer

    composer require algatux/influxdb-bundle dev-master

### Configuration

in your config.yml add:
    
    influx_db:
      host: '{your influxdb host address}' (default localhost)
      database: '{your influxdb host address}' (default udp)
      udp_port: '{your influxdb udp port}' (default 4444)
      http_port: '{your influxdb http port}' (default 8086)
    

### Sending data to influx db

assuming this collection to send:

```php
$time = new \DateTime();

$points = new PointsCollection([new Point(
    'test_metric', // name of the measurement
    0.64, // the measurement value
    ['host' => 'server01', 'region' => 'italy'], // optional tags
    ['cpucount' => rand(1,100), 'memory' => memory_get_usage(true)], // optional additional fields
    $time->getTimestamp()
)]);

```

###### Service

```php

// UDP
$container
    ->get('algatux_influx_db.services_clients.udp.writer_client')
    ->write($points, Database::PRECISION_SECONDS);

// HTTP
$container
    ->get('algatux_influx_db.services_clients.http.writer_client')
    ->write($points, Database::PRECISION_SECONDS);

```

###### Event Dispatcher

```php

// UDP
$container
    ->get('event_dispatcher')
    ->dispatch(InfluxDbEvent::NAME, new InfluxDbEvent($points, Database::PRECISION_SECONDS, ClientInterface::UDP_CLIENT));

// HTTP
$container
    ->get('event_dispatcher')
    ->dispatch(InfluxDbEvent::NAME, new InfluxDbEvent($points, Database::PRECISION_SECONDS, ClientInterface::HTTP_CLIENT));

```

### Todo:

- add support for credentials login
- add support to select database
- ...

