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
- 1.3 - Добавлена пакетная очистка данных.

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
