yii2-vault-client
==========================

This extension client for Hashicorp Vault.

## Installation

The preferred way to install this extension through [composer](http://getcomposer.org/download/).

You can set the console

```
~$ composer require "nohnaimer/yii2-vault-client" --prefer-dist
```

or add

```
"require": {
    "nohnaimer/yii2-vault-client": "0.1.*"
}
```

in ```require``` section in `composer.json` file.

## Configuration

For store php-fpm environment variables from system (macOS, Linux, Unix) need to uncomment clear_env = no string in /etc/php/php-fpm.d/www.conf 

Need add environment variables:
```yaml
VAULT_ADDR=https://vault.url/
VAULT_TOKEN=token
VAULT_KV_PATH=/kv
```

docker-compose example:
```yaml
...
php:
  image: php:latest
  container_name: php
  restart: on-failure
  working_dir: /var/www
  environment:
    VAULT_ADDR: https://127:0:0:1:8200/
    VAULT_TOKEN: hvs.hrpvk3rEpD2HaHckeb976Ppw
  volumes:
    - .:/var/www:cached
  depends_on:
    - postgres
...
```

### Use yii2 migrations

```php
class m221103_161325_vault_init extends Migration
{
    /**
     * {@inheritdoc}
     */
    public function safeUp()
    {
        $client = new Client([
            'url' => 'url',
            'token' => 'token',
        ]);

        $kv = new KVv1([
            'path' => '/kv',
            'client' => $client,
        ]);
        
        //add
        $kv->post('/my/secret', ['key' => 'value']);
        
        //delete
        $kv->delete('/my/secret/key');
    }
}
```

## License

**yii2-vault-client** it is available under a BSD 3-Clause License. Detailed information can be found in the `LICENSE.md`.
