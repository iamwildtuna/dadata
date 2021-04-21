[![Latest Stable Version](https://poser.pugx.org/iamwildtuna/dadata/v/stable)](https://packagist.org/packages/iamwildtuna/dadata)
[![Total Downloads](https://poser.pugx.org/iamwildtuna/dadata/downloads)](https://packagist.org/packages/iamwildtuna/dadata)
[![License](https://poser.pugx.org/iamwildtuna/dadata/license)](https://packagist.org/packages/iamwildtuna/dadata)

# !!! Принято решения развивать SDK в отдельном [проекте](https://github.com/iamwildtuna/dadata-sdk) он будет реализовывать все функции API DaData.ru !!!  

SDK для работы с API DaData.ru (Fork [gietos/dadata](https://github.com/gietos/dadata))
=================  
Посмотреть все проекты или поддержать автора можно [тут](https://lapay.group/opensource).  

[API документация](https://dadata.ru/api/clean/)

<a name="links"><h1>Changelog</h1></a>

- 1.2 - Добавлена поддержа [составной записи](https://dadata.ru/api/clean/#request-record);  
- 1.3 - Добавлена пакетная очистка данных;    
- 1.4 - Добавлена поддержка Guzzle 7;  
- 1.5 - Исправлена ошибка в функции cleanAddress, добавлена поддержка Guzzle 7.2;
- 1.5.1 - Добавлена поддержка Guzzle 7.3;

# Установка  
Для установки можно использовать менеджер пакетов Composer

    composer require iamwildtuna/dadata

## Использование



``` php
$client = new Dadata\Client(new \GuzzleHttp\Client(), [
    'token' => '...',
    'secret' => '...',
]);
```

### Очистка данных

Методы на вход могут принимать как строку, так и массив до 50 элементов (ограничение API DaData).  

Пример очистки данных строками
``` php
$response = $client->cleanAddress('мск сухонска 11/-89');
$response = $client->cleanAddressSuggestion('мск сухонска 11/-89'); // Очистка адреса через API подсказок
/*
Dadata\Response\Address Object
(
    [source] =>
    [result] =>
    [postal_code] => 127642
    [country] => Россия
    [region_fias_id] => 0c5b2444-70a0-4932-980c-b4dc0d3f02b5
    [region_kladr_id] => 7700000000000
    [region_with_type] => г Москва
    [region_type] => г
    [region_type_full] => город
    [region] => Москва
    [area_fias_id] =>
    [area_kladr_id] =>
    [area_with_type] =>
    [area_type] =>
    [area_type_full] =>
    [area] =>
    [city_fias_id] => 0c5b2444-70a0-4932-980c-b4dc0d3f02b5
    [city_kladr_id] => 7700000000000
    [city_with_type] => г Москва
    [city_type] => г
    [city_type_full] => город
    [city] => Москва
    [city_area] => Северо-восточный
    [city_district_fias_id] =>
    [city_district_kladr_id] =>
    [city_district_with_type] => р-н Северное Медведково
    [city_district_type] => р-н
    [city_district_type_full] => район
    [city_district] => Северное Медведково
    [settlement_fias_id] =>
    [settlement_kladr_id] =>
    [settlement_with_type] =>
    [settlement_type] =>
    [settlement_type_full] =>
    [settlement] =>
    [street_fias_id] => 95dbf7fb-0dd4-4a04-8100-4f6c847564b5
    [street_kladr_id] => 77000000000283600
    [street_with_type] => ул Сухонская
    [street_type] => ул
    [street_type_full] => улица
    [street] => Сухонская
    [house_fias_id] => 5ee84ac0-eb9a-4b42-b814-2f5f7c27c255
    [house_kladr_id] => 7700000000028360004
    [house_type] => д
    [house_type_full] => дом
    [house] => 11
    [block_type] =>
    [block_type_full] =>
    [block] =>
    [flat_type] => кв
    [flat_type_full] => квартира
    [flat] => 89
    [flat_area] => 0
    [square_meter_price] =>
    [flat_price] =>
    [postal_box] =>
    [fias_id] => 5ee84ac0-eb9a-4b42-b814-2f5f7c27c255
    [fias_level] => 8
    [kladr_id] => 7700000000028360004
    [capital_marker] => 0
    [okato] => 45280583000
    [oktmo] => 45362000
    [tax_office] => 7715
    [tax_office_legal] => 7715
    [timezone] =>
    [geo_lat] => 55.8782557
    [geo_lon] => 37.65372
    [beltway_hit] =>
    [beltway_distance] => 0
    [qc_geo] => 0
    [qc_complete] => 0
    [qc_house] => 0
    [unparsed_parts] =>
    [metro] =>
    [qc] => 0
)
*/
$response = $client->cleanPhone('тел 7165219 доб139');
$response = $client->cleanPassport('4509 235857');
$response = $client->cleanName('Срегей владимерович иванов');
$response = $client->cleanEmail('serega@yandex/ru');
$response = $client->cleanDate('24/3/12');
$response = $client->cleanVehicle('форд фокус')
```

Пример очистки списка данных
``` php
$data[] = '9261123934';
$data[] = '8 (903) 126-12-33';

$response = $client->cleanPhone($data);
```

Ответ
```
Array
(
    [0] => Dadata\Response\Phone Object
        (
            [source] => 9261123934
            [type] => Мобильный
            [phone] => +7 926 112-39-34
            [country_code] => 7
            [city_code] => 926
            [number] => 1123934
            [extension] => 
            [provider] => ПАО "МегаФон"
            [region] => Москва и Московская область
            [timezone] => UTC+3
            [qc_conflict] => 0
            [qc] => 0
        )

    [1] => Dadata\Response\Phone Object
        (
            [source] => 8 (903) 126-12-33
            [type] => Мобильный
            [phone] => +7 903 126-12-33
            [country_code] => 7
            [city_code] => 903
            [number] => 1261233
            [extension] => 
            [provider] => ПАО "Вымпел-Коммуникации"
            [region] => Москва и Московская область
            [timezone] => UTC+3
            [qc_conflict] => 0
            [qc] => 0
        )

)
```

Пример составного запроса
``` php
$data['structure'] = ['AS_IS', 'NAME', 'ADDRESS', 'PHONE'];

$item1 = [1, 'Ианов Иван Питрович', 'Сухонская улица, 11 кв 89', '89262223344'];
$item2 = [2, 'Петров Еван Алегович', 'мск сухонска 11/-89', '89221234356'];

$data['data'][] = $item1;
$data['data'][] = $item2;

$response = $client->cleanStructure($data); // Возвращает данные в виде ассоциативного массива
```
