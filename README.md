PHP Simple HTML DOM Parser" Yii2 extension (Sereja3578/project-yii2-simple-html-dom)
===============
"Simple HTML Dom"(PHP Simple HTML DOM Parser) http://simplehtmldom.sourceforge.net"

Installation
------------

The preferred way to install this extension is through [composer](http://getcomposer.org/download/).
Either run
```
php composer.phar require --prefer-dist Sereja3578/project-yii2-simple-html-dom "dev-master"
```
or (if composer installed)
```
composer require --prefer-dist Sereja3578/project-yii2-simple-html-dom "dev-master"
```
OR add(code below) to the require section of your `composer.json` file and run command Install(Composer)
```
"Sereja3578/project-yii2-simple-html-dom": "dev-master"
```

Usage
-----

Once the extension is installed, simply use it in your code by  :

```php
<?php
use Sereja3578\simplehtmldom\simple_html_dom;
use Sereja3578\simplehtmldom\SimpleHTMLDom;
use yii\base\Component;
use yii\httpclient\Exception;
use yii\httpclient\Client;
use yii\httpclient\Request;
use yii\httpclient\Response;
use Yii;

class Parser extends Component
{
    /**
     * @var string
     */
    const BASE_URL = "https://mail.ru";

    /**
     * @var $request Request
     */
    private $request;
    /**
     * @var $request Response
     */
    private $response;
    /**
     * @var $client Client
     */
    private $client;

    /**
     * @return void
     */
    public function init()
    {
        parent::init();

        $this->client = new Client([
            "transport" => "yii\httpclient\CurlTransport",
            "baseUrl" => self::BASE_URL
        ]);
    }

    /**
     * @param string $url
     * @return bool|simple_html_dom|Exception
     */
    public function getPage(string $url)
    {
        try {
            $this->setRequest($url);
            $this->setResponse();

            if ($this->response->getStatusCode() != "200") {
                return Yii::t("http-errors", "Ресурс не доступен");
            }

            return SimpleHTMLDom::str_get_html($this->response->content);
        } catch (Exception $e) {
            return $e;
        }
    }

    /**
     * @param $url string
     * @return void
     */
    public function setRequest(string $url)
    {
        $this->request = $this->client->get($url);
    }

    public function setResponse()
    {
        $this->response = $this->request->send();
    }
?>
```
About the functional read on the official website: http://simplehtmldom.sourceforge.net/
