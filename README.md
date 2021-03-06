# Introduction

Calculate the distance between coordinates.

[![Build Status](https://scrutinizer-ci.com/g/ZeroConfig/GeoDistance/badges/build.png?b=master)](https://scrutinizer-ci.com/g/ZeroConfig/GeoDistance/build-status/master)
[![Scrutinizer Code Quality](https://scrutinizer-ci.com/g/ZeroConfig/GeoDistance/badges/quality-score.png?b=master)](https://scrutinizer-ci.com/g/ZeroConfig/GeoDistance/?branch=master)
[![Code Coverage](https://scrutinizer-ci.com/g/ZeroConfig/GeoDistance/badges/coverage.png?b=master)](https://scrutinizer-ci.com/g/ZeroConfig/GeoDistance/?branch=master)
[![SensioLabsInsight](https://insight.sensiolabs.com/projects/1cf0e362-252c-421d-8bfb-73c6db10cdc4/mini.png)](https://insight.sensiolabs.com/projects/1cf0e362-252c-421d-8bfb-73c6db10cdc4)

This package uses the `UnitAngle` and `UnitLength` classes from
[`nmarfurt/measurements`](https://packagist.org/packages/nmarfurt/measurements).
Further, resulting measurements, like distance, are represented by a `Measurement`.
To make full use of this library, it is recommended to familiarize oneself with
that package first.

# Installation

```
composer require zero-config/geo-distance
```

# Procedural approach

To use the data models for coordinates and distance calculation in a procedural
manner, try the following:

```php
<?php
use function ZeroConfig\GeoDistance\coordinates;
use function ZeroConfig\GeoDistance\distance;

// Returns a distance of approximately 361 kilometers.
// The distance function uses the distance calculator under the hood.
// It assumes earth as base sphere for calculations.
echo distance(
    coordinates(50.0, 5.0),
    coordinates(53.0, 3.0)
) . PHP_EOL;
```

## coordinates

The coordinates function accepts:

- `float $latitude`
- `float $longitude`

Both are a number representing an angle in degrees.

It returns an instance of:

`ZeroConfig\GeoDistance\PositionInterface`

```php
<?php
use function ZeroConfig\GeoDistance\coordinates;

// This outputs: 50.6 °
echo coordinates(50.6, 5.0)->getLatitude() . PHP_EOL;
```

## distance

The distance function accepts:

- `ZeroConfig\GeoDistance\PositionInterface $start`
- `ZeroConfig\GeoDistance\PositionInterface $end`

Both are positions on a sphere.

It returns an instance of:

`Measurements\Measurement`

```php
<?php
use function ZeroConfig\GeoDistance\coordinates;
use function ZeroConfig\GeoDistance\distance;
use Measurements\Units\UnitLength;

// Outputs: 36113.471850273 dam
echo distance(
    coordinates(50.0, 5.0),
    coordinates(53.0, 3.0)
)->convertTo(UnitLength::decameters()) . PHP_EOL;
```

# Object oriented approach

The following data models exist:

* Position, combined latitude and longitude
* Sphere, the object on which coordinates are plotted when calculating distance

```php
<?php
use ZeroConfig\GeoDistance\ConvertedDistanceCalculator;
use ZeroConfig\GeoDistance\DistanceCalculator;
use ZeroConfig\GeoDistance\Position;
use ZeroConfig\GeoDistance\Sphere\CelestialBody\Mars;
use Measurements\Units\UnitLength;

$marsDistanceCalculator = new ConvertedDistanceCalculator(
    new DistanceCalculator(new Mars()),
    UnitLength::kilometers()
);

// On Mars, the same coordinates as on earth, give a distance of only 192 kilometers.
echo $marsDistanceCalculator->calculate(
    Position::create(50.0, 5.0),
    Position::create(53.0, 3.0)
) . PHP_EOL;
```
