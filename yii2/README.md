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



### Перевод plural сообщения

messages.php
```php
'{n, plural, one{bedroom} few{bedrooms} other{bedrooms}}' => '{n, plural, one{спальня} few{спальни} other{спален}}',
```

view.php
```php
<?= Yii::$app->i18n->translate(
    'messages',
    '{n, plural, one{bedroom} few{bedrooms} other{bedrooms}}',
    ['n' => $model->bedrooms_count],
    Yii::$app->language
) ?>
```



### Отказ от fxp/composer-asset-plugin в пользу hiqdev/asset-packagist
С версии 2.0.13 шаблоны приложений Yii2 используют hiqdev/asset-packagist вместо fxp/composer-asset-plugin, используйте инструкцию ниже, чтобы мигрировать.

Удалите fxp/composer-asset-plugin из системы
```bash
composer global remove fxp/composer-asset-plugin
```

Удалите из секции ```config``` файла ```composer.json```:
```json
"fxp-asset":{
    "installer-paths": {
        "npm-asset-library": "vendor/npm",
        "bower-asset-library": "vendor/bower"
    }
}
```

Добавьте в ваш ```composer.json```:
```json
"repositories": [
    {
        "type": "composer",
        "url": "https://asset-packagist.org"
    }
]
```

В итоге ваш ```composer.json``` должен иметь примерно следующий вид:
```json
{
    "minimum-stability": "stable",
    "require": {
        "dependencies"
    },
    "require-dev": {
        "dev dependencies"
    },
    "config": {
        "process-timeout": 1800
    },
    "repositories": [
        {
            "type": "composer",
            "url": "https://asset-packagist.org"
        }
    ]
}
```
Задайте алиасы в конфигурации вашего приложения(```common/config/main.php``` для advanced шаблона ```config/web.php``` для basic):
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

Последним шагом вы можете просто удалить ```./vendor``` и выполнить ```composer update```, либо переместите ```./vendor/bower``` в ```./vendor/bower-asset```, ```./vendor/npm``` в ```./vendor/npm-asset```,



### Как создать миграции на существующую структуру
Есть несколько расширений, советую [Insolita/yii2-migrik](https://github.com/Insolita/yii2-migrik) т.к. сам его проверял.

Так же есть [bizley/yii2-migration](https://github.com/bizley/yii2-migration), но данным расширением не пользовался.



### Как сделать фикстуры из существующих в БД данных
Есть несколько расширений, советую [Insolita/yii2-fixturegii](https://github.com/Insolita/yii2-fixturegii) т.к. сам его проверял.

Так же есть [ElisDN/yii2-gii-fixture-generator](https://github.com/ElisDN/yii2-gii-fixture-generator), но данным расширением не пользовался.


### Как повесить глобальный AccessControl для приложения
Для глобального запрета доступа к приложению зачастую применяется антипаттерн GodObject в виде создания базового контроллера и наследования от него. В Yii2 правильным решением данной задачи будет подключение `AccessControl` фильтра в конфигурации приложения:
```php
<?php
// other code

return [
    // other code
    
    'as access' => [
        'class' => 'yii\filters\AccessControl',
        'except' => ['auth/login', 'site/error'],
        'rules' => [
            [
                'allow' => true,
                'roles' => ['admin'],
            ],
        ],
    ],
    
    // other code
];
```
Данный пример разрешает пользователям с ролью `admin` доступ ко всем маршрутам приложения. Маршруты `'site/error'` и `user/security/login` доступны всем пользователям.


### Как сделать login screen
Чтобы сделать так называемый login screen, необходимо подменить layout, самый простой и элегантный способ это сделать, повесить обработчик на событие `beforeAction`:
```php
'on beforeAction' => function (\yii\base\Event $e) {
    /** @var \yii\web\Application $app */
    $app = $e->sender;
    // Set login layout for guest users
    if ($app->getUser()->getIsGuest()) {
        $app->layout = 'login';
    }
},
```
