# NodeJS

## Парсеры на NodeJS

Парсеры на NodeJS писать очень приятно и легко. Асинхронно-событийная JS модель позволяет без лишних хлопот сделать парсер асинхронным.

Для получения данных в большинстве случаев достаточно библиотеки [request](https://github.com/request/request).

Для разбора HTML советую взять [cheerio](https://cheerio.js.org/).
Cheerio это порт многим известного JQuery, что позволит _сразу_ делать выборки данных.

Для многопоточности конечно же [eachOfLimit](https://caolan.github.io/async/docs.html#eachOfLimit) из [async](https://caolan.github.io/async/).

#### Как распарсить Win-1251 кодировку?

Подключаете библиотеку [iconv](https://www.npmjs.com/package/iconv).

`request` вызвааете с `encoding`: `null` или `'binary'` и потом, используя библиотеку iconv уже разбираете: 

```js
const request = require('request');
const iconv = require('iconv');

request({
    uri: 'http://example.com/',
    encoding: 'binary'
}, function (err, res, body) {
    let buffer = new Buffer(body, 'binary');
    let translator = new iconv.Iconv('windows-1251', 'utf8');
    $ = cheerio.load(translator.convert(buffer).toString());
});

```


#### Как выполнить запрос синхронно?

Бывет, что необходимо спарсить дополнительные данные для текущего документа синхронными запросами, для этого отлично подойдет библиотека [urllib-sync](https://www.npmjs.com/package/urllib-sync).

#### Загрузка файлов

Простой пример из практики, загрузка изображений(т.к. сайтов было несколько, то для сохранения уникальности имени было решено взять md5 от полного URL до изображения):

```js
const request = require('request');
const md5 = require('md5');

let uploadImage = function (url, callback) {
    let path = __dirname + '/images/';

    if (!fs.existsSync(path)) {
        fs.mkdirSync(path);
    }

    download(url, path + md5(url), (state) => {
        callback(state);
    });
};


let download = function (url, filename, callback) {
    request.head(url, function (err, res) {

        if (fs.existsSync(filename)) {
            if (fs.statSync(filename).size == res.headers['content-length']) {
                return callback('exist');
            }
        }

        if (!err) {
            request(url, {encoding: 'binary'}, function(err, response, body) {
                if(err) {
                    console.error(err);
                    return callback('error on download image');
                }

                fs.writeFile(filename, body, 'binary', function (err) {
                    if(err) {
                        console.error(err);
                        return callback('error on write image');
                    } else {
                        return callback('downloaded and saved');
                    }
                });
            });
        } else {
            console.error(err);
            return callback('error on try get header ' + url);
        }
    });
};
```

