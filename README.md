# Muslim Data Android

Muslim Data for Android is a library that brings Islamic data to your Android applications. It unifies features into a single API so you can easily integrate functionalities such as:

- Fixed or Calculated Prayer Times
- Offline Geocoding and Reverse Geocoding
- Location Search
- Azkars (Hisnul Muslim) with translations
- 99 Names of Allah with translations

## Available on Other Platforms

This library is also available for other platform integration:

- [Muslim Data for iOS](https://github.com/my-prayers/muslim-data-ios)
- [Muslim Data for Flutter](https://github.com/my-prayers/muslim-data-flutter)

The other platform libraries share the same concepts and data structure, making it easy to develop similar applications across different platforms.

# Features

✅ **Prayer Times**: Most cities around the world find their prayer times by using some calculations which is based on location (longitude and latitude) but some other cities have fixed time table for their prayer times. This library contains most fixed and calculated prayer times. Now you can contribute it to improve it and also you can use it in Muslim communities or Muslim apps.  
✅ **Location Services**: Search for locations offline, geocode by city name, and reverse geocode using latitude and longitude.  
✅ **Azkars (Hisnul Muslim)**: Retrieve categorized azkars by (Category, Chapter, Item) in various languages.  
✅ **Names of Allah**: Access 99 Names of Allah along with translations in supported languages.

# Usage

## Installation

```
implementation 'dev.kosrat:muslimdata:2.6.0'
```

## Migration Guide

If you're upgrading from version 1.x to version 2.x of `muslim-data-ios`, please refer to the [Migration Guide](MIGRATION_GUIDE.md) for detailed instructions on updating your code to accommodate the changes in the latest release.

## Location Services

There are some location helper methods in the MuslimRepository that provides **offline Location Search**, **Geocoding**, and **Reverse Geocoding** and also each of one will return `Location` object or list of `Location`. `Location` object contains (`countryCode`, `countryName`, `cityName`, `latitude`, `longitude`, and `hasFixedPrayerTime`).

### Search for a location

You can search for any cities or places around the world and this is useful when a user doesn't have internet connection or user's location is turned off so that you can search here:

```kotlin
lifecycleScope.launch {
    val repository = MuslimRepository(this@MainActivity)
    val locationList = repository.searchLocation("erb")
    Log.i("searchLocation", "$locationList")
}
```

### Geocode a location

Use geocoder method to find a location by country code and city name.

```kotlin
lifecycleScope.launch {
    val repository = MuslimRepository(this@MainActivity)
    val location = repository.geocoder("iq", "erbil")
    Log.i("geocoder", "$location")
}
```

### Reverse Geocode a location

Use reverseGeocoder method to find a location by latitude and longitude.

```kotlin
lifecycleScope.launch {
    val repository = MuslimRepository(this@MainActivity)
    val location = repository.reverseGeocoder(36.0901, 43.0930)
    Log.i("reverseGeocoder", "$location")
}
```

## Prayer Times

You can easily get prayer times for a location just by passing (`Location`, `PrayerAttribute`, and `Date`) objects to `getPrayerTimes` method.

```kotlin
lifecycleScope.launch {
    val repository = MuslimRepository(this@MainActivity)

    val attribute = PrayerAttribute(
        CalculationMethod.MAKKAH,
        AsrMethod.SHAFII,
        HigherLatitudeMethod.ANGLE_BASED,
        intArrayOf(0, 0, 0, 0, 0, 0)
    )
    val prayerTime = repository.getPrayerTimes(
        location,
        Date(),
        attribute
    )
    Log.i("Prayer times ", "$prayerTime")
    Log.i(
        "formatPrayerTime ",
        prayerTime.formatPrayerTime(TimeFormat.TIME_12).contentToString()
    )
    Log.i("nextPrayerTimeIndex", "${prayerTime.nextPrayerTimeIndex()}")
    Log.i("nextPrayerTimeInterval", "${prayerTime.nextPrayerTimeInterval()}")
    Log.i("nextPrayerTimeRemaining", prayerTime.nextPrayerTimeRemaining())
}
```

## Azkars (Hisnul Muslim)

Get all azkars from (Hisnul Muslim book) that is categorized by (`AzkarCategory`, `AzkarChapter`, and `AzkarItem`) and also the azkars are available for these languages (`en`, `ar`, `ckb`, `ckb_BADINI`, `fa`, and `ru`)

### Azkar Category

Get all azkar categories with its translation for the given language.

```kotlin
lifecycleScope.launch {
    val repository = MuslimRepository(this@MainActivity)
    val azkarCategories = repository.getAzkarCategories(Language.EN)
    Log.i("azkarCategories", "$azkarCategories")
}
```

### Azkar Chapters

Get azkar chapters with its translation for the given language.

```kotlin
lifecycleScope.launch {
    val repository = MuslimRepository(this@MainActivity)
    val azkarChapters = repository.getAzkarChapters(Language.EN)
    Log.i("azkarChapters", "$azkarChapters")
}
```

Get azkar chapters for a specific category with its translation for the given language.

```kotlin
lifecycleScope.launch {
    val repository = MuslimRepository(this@MainActivity)
    val azkarChapters = repository.getAzkarChapters(Language.EN, 1)
    Log.i("azkarChapters", "$azkarChapters")
}
```

Get azkar chapters by chapter ids. This method is particularly useful for implementing a favorites feature on azkar. By just saving the azkar ids, you can later retrieve the full details when needed using this method, simplifying management and synchronization of your favorite azkar entries.

```kotlin
lifecycleScope.launch {
    val repository = MuslimRepository(this@MainActivity)
    val azkarChapters = repository.getAzkarChapters(Language.EN, [12, 15])
    Log.i("azkarChapters", "$azkarChapters")
}
```

### Azkar Items

Get azkar items for a specific chapter and it is localized for the given language.

```kotlin
lifecycleScope.launch {
    val repository = MuslimRepository(this@MainActivity)
    val azkarItems = repository.getAzkarItems(1, Language.EN)
    Log.i("azkarItems", "$azkarItems")
}
```

## Names of Allah

Get 99 Names of Allah with its translation and it is available for these languages (`en`, `ar`, `ckb`, `ckb_BADINI`, `fa`, and `ru`)

```kotlin
lifecycleScope.launch {
    val repository = MuslimRepository(this@MainActivity)
    val names = repository.getNamesOfAllah(Language.EN)
    Log.i("Names", "$names")
}
```

# Author

Kosrat D. Ahmed, kosrat.d.ahmad@gmail.com

# License

**Muslim Data for Android** is available under the MIT license. See the [LICENSE](LICENSE) file for details.
