### Автоматический парсинг JSON параметров переданных клиентом

Если необходимо автоматически распарсить JSON переданный от клиента, то самый простой способ это воспользоваться [yii\web\JsonParser](http://www.yiiframework.com/doc-2.0/yii-web-jsonparser.html)


В случае, когда необходимо определить парсер только для некоторых контроллеров/экшенов, можно переопределить метод beforeAction следующим образом:

```php
public function beforeAction($action)
{
    \Yii::$app->request->parsers = [
        'application/json' => 'yii\web\JsonParser',
    ];
    return parent::beforeAction($action);
}
```


### Отказ от fxp/composer-asset-plugin в пользу hiqdev/asset-packagist

Устали ждать пока fxp/composer-asset-plugin обновит все зависимости? Решение есть, просто начните использовать [asset-packagist](https://asset-packagist.org/).

Установка очень простая, сначала добавьте в ваш ```composer.json```:
```json
"repositories": [
    {
        "type": "composer",
        "url": "https://asset-packagist.org"
    }
]
```

Для сохранения обратной совместимости с fxp/composer-asset-plugin в секции **extra -> asset-installer-paths** отредактируйте пути:
```
vendor/npm -> vendor/npm-asset
vendor/bower -> vendor/bower-asset
```

Финальным шагом задайте алиасы в конфигурации вашего приложения:
```php
$config = [
    ...
    'aliases' => [
        '@bower' => '@vendor/bower-asset',
        '@npm'   => '@vendor/npm-asset',
    ],
    ...
];
```

Чтобы одновременно поддерживать проекты и с fxp/composer-asset-plugin и с hiqdev/asset-packagist не удаляя глобально fxp/composer-asset-plugin используйте опцию ```--no-plugins``` при выполнении команды update/install

Известные проблемы:
Первую установку всё же придется провести используя плагин fxp/composer-asset-plugin, т.к. файл vendor/yiisoft/extensions.php не будет создан без плагина. В этом случае перестанут работать алиасы, можно пофиксить указывая путь относительно алиаса @vendor.
