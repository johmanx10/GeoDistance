# Introduction

Calculate the distance between coordinates.

# Installation

```
composer require hylianshield/geolocation
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

// On Mars, the same coordinates give a distance of only 192 kilometers.
echo $marsDistanceCalculator->calculate(
    Position::create(50.0, 5.0),
    Position::create(53.0, 3.0)
) . PHP_EOL;
```