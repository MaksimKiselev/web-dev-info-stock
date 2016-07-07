### Автоматический парсинг JSON параметров переданных клинетом

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
