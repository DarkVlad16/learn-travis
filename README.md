# Изучение [Travis CI](https://travis-ci.com)


<div align="center">

[![Build Status](https://travis-ci.org/DarkVlad16/learn-travis.svg?branch=master)](https://travis-ci.org/DarkVlad16/learn-travis)

</div>

##  Разделы
1.  [Связывание Travis с Github](#authorize-app)
2.  [Тестовый пример](#tutorial)
    1.  [Создание проектных файлов](#create-the-project-files)
    2.  [Исправление кода](#correct-code-to-pass-build)
3.  [Использование `Переменных` `среды` с Travis!](#environment-variables)
    1. [Include Environment Variables in your `.travis.yml` file](#environment-variables-travis.yml)
    2. [Add environment Variables in the Web Interface](#environment-variables-web-interface)

<a name="authorize-app"></a>
## **Связывание Travis с Github**

1. Перейдите на сайт: https://travis-ci.com/ и кликните на "**Sign in with GitHub**".

2. Вы будете перенаправлены в GitHub, где вам нужно нажать "**Авторизовать приложение**"

> **Примечание**: Если вы когда-либо захотите *запретить* Travis, доступ к вашей учетной записи GitHub, просто посетите: https://github.com/settings/applications и нажмите «*Отменить*».  

3. После того как вы разрешили доступ, вы вернетесь в Трэвис, где вам нужно будет включить конкретный репозиторий Git.   
Вы также можете сделать это в своем профиле Travis: https://travis-ci.com/profile


<a name="tutorial"></a>
## **Тестовый пример**

<a name="create-the-project-files"></a>
### `Создание проектных файлов`

Предварительные условия:

+ **компьютер** *с* ***установленным*** **node.js**
+ любой **текстовый редактор**

В этом примере структура проекта и файлы, которые вам понадобятся, следующие:

```
project_folder
|_.travis.yml
|_hello.js
|_package.json
|_other_files
```
Как только вы подключили свой проект к Travis, каждый раз, когда вы нажимаете новую версию на GitHub
Travis будет искать всю папку проекта для файлов, строить ваш проект и
автоматически запускайте все тесты. Таким образом, вам не нужно указывать, какие папки
Тревису нужно проверить - он всегда **проверяет все**!

+ Создайте файл `.travis.yml` и вставьте следующий код:
```yml
language: node_js
node_js:
 - "node"
```
+ Создайте файл `hello.js` и вставьте следующий код:

```javascript
var http = require('http');
http.createServer(function (req, res) {
  res.writeHead(200, {'Content-Type': 'text/plain'});
  res.end('Hello Travis!\n')
}).listen(1337, '127.0.0.1');
console.log('Server running at http://127.0.0.1:1337/');
```

+ Создайте файл `package.json` и вставьте следующий код:

```javascript
{
  "name": "learn-travis-YOURNAME",
  "description": "Simple Travis-CI check for JSHint (Code Linting)",
  "author": "your name here :-)",
  "version": "0.0.1",
  "devDependencies": {
    "jshint": "^2.6.0"
  },
  "scripts": {
    "test": "jshint hello.js"
  }
}
```
Этот файл сообщает Travis, чтобы он запускал `jshint` в нашем файле `hello.js`.  
`jshint` - это программа, которая анализирует качество кода, поэтому идеально подходит для тестирования!

Чтобы запустить тестовую команду, нам нужно будет установить модуль узла jshint из NPM:

```sh
npm install jshint --save-dev
```

Теперь вы можете запустить команду `test` *локально*, введя `npm test` в свой терминал.

Если вы это сделаете, вы увидите, что тест завершился неудачно.

+ Сделайте комит всех изменений и отправьте его на репозиторий.

```javascript
var http = require('http');
http.createServer(function (req, res) {
  res.writeHead(200, {'Content-Type': 'text/plain'});
  res.end('Hello Travis!\n')  // Тест упадет здесь!
}).listen(1337, '127.0.0.1');
console.log('Server running at http://127.0.0.1:1337/');
```

**Ошибка:** Не хватает "`;`" в 4-й строчке.

<a name="correct-code-to-pass-build"></a>
### `Исправление кода`

+ Измените файл `hello.js` вставив следующий код:

```javascript
var http = require('http');
http.createServer(function (req, res) {
  res.writeHead(200, {'Content-Type': 'text/plain'});
  res.end('Hello Travis!\n'); // Теперь тест пройдет успешно!
}).listen(1337, '127.0.0.1');
console.log('Server running at http://127.0.0.1:1337/');
```

<a name="environment-variables"></a>
## **Использование `Переменных` `среды` с Travis!**

Часто ваше приложение будет использовать **переменные среды** для хранения
ключей, паролей или другие конфиденциальные данные, которые вы не хотите жестко кодировать в своем коде; Travis-CI делает это **легким**:

Есть **2 способа** сказать Travis-CI о ваших переменных среды:

<a name="environment-variables-travis.yml"></a>
### 1. Добавление переменных среды в `.travis.yml`

Самый простой и самый явный способ перечисления переменных среды
заключается в том, чтобы добавить их в ваш файл `.travis.yml`:

```yml
language: node_js
node_js:
 - "node"
env:
- MY_VAR=EverythignIsAwesome
- NODE_ENV=TEST
```
Интересная часть - ключ `env:`, где вы можете перечислить
ваши переменные среды и их соответствующие значения.

<a name="environment-variables-web-interface"></a>
### 2. Добавление переменных среды в веб-интерфейс

Другой способ рассказать Travis-CI о вашей переменной среды (ей) заключается в их добавлении в пользовательский интерфейс веб-интерфейса (веб-интерфейс) на странице настроек вашего проекта:

![Инструкция](https://i.imgur.com/AbUQPKR.png)

> *Обратите внимание* как в случае добавления переменных среды в веб-интерфейс Travis они скрыты (*из журнала построения*) по умолчанию. Это *не* предотвращает случайное использование `console.log` их и раскрытия ключа / пароля.  
> Поэтому будьте осторожны, когда используете console.log!
