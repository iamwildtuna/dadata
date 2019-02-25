SDK для работы с API DaData.ru (Fork [gietos/dadata](https://github.com/gietos/dadata))
=================

[API документация](https://dadata.ru/api/clean/)

<a name="links"><h1>Changelog</h1></a>

- 1.2 - Добавлена поддержа [составной записи](https://dadata.ru/api/clean/#request-record);  

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

``` php
$response = $client->cleanAddress('мск сухонска 11/-89');
$response = $client->cleanPhone('тел 7165219 доб139');
$response = $client->cleanPassport('4509 235857');
$response = $client->cleanName('Срегей владимерович иванов');
$response = $client->cleanEmail('serega@yandex/ru');
$response = $client->cleanDate('24/3/12');
$response = $client->cleanVehicle('форд фокус')

$data['structure'] = ['AS_IS', 'NAME', 'ADDRESS', 'PHONE'];

$item1 = [1, 'Ианов Иван Питрович', 'Сухонская улица, 11 кв 89', '89262223344'];
$item2 = [2, 'Петров Еван Алегович', 'мск сухонска 11/-89', '89221234356'];

$data['data'][] = $item1;
$data['data'][] = $item2;

$response = $client->cleanStructure($data); // Возвращает данные в виде ассоциативного массива
```
