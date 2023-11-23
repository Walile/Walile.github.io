---
layout: post
title: Laravel 支援多資料庫存取設定
date: 2018-01-22 11:39:17
tags: Laravel, PHP
---

Laravel 5.5 (或以上) 設定多資料庫設定需要處理幾個地方。
<!--more-->

1. .env的部分可以把database的相關全部註解掉(欸

2. 在config/database.php裡面，先設定要連的db有幾個：
``` 
connections' => [

        'db1' => [
            'driver' => 'mysql',
            'host' => env('DB_HOST', '127.0.0.1'),
            'port' => env('DB_PORT', '3306'),
            'database' => env('DB_DATABASE', 'db1'),
            'username' => env('DB_USERNAME', 'db1'),
            'password' => env('DB_PASSWORD', 'db1'),
            'unix_socket' => env('DB_SOCKET', ''),
            'charset' => 'utf8mb4',
            'collation' => 'utf8mb4_unicode_ci',
            'prefix' => '',
            'strict' => true,
            'engine' => 'InnoDB',
        ],

        'db2' => [
            'driver' => 'mysql',
            'host' => env('DB_HOST', '127.0.0.1'),
            'port' => env('DB_PORT', '3306'),
            'database' => env('DB_DATABASE', 'db2'),
            'username' => env('DB_USERNAME', 'db2'),
            'password' => env('DB_PASSWORD', 'db2'),
            'unix_socket' => env('DB_SOCKET', ''),
            'charset' => 'utf8mb4',
            'collation' => 'utf8mb4_unicode_ci',
            'prefix' => '',
            'strict' => true,
            'engine' => 'InnoDB',
        ],
        'db3' => [
            'driver' => 'mysql',
            'host' => env('DB_HOST', '127.0.0.1'),
            'port' => env('DB_PORT', '3306'),
            'database' => env('DB_DATABASE', 'db3'),
            'username' => env('DB_USERNAME', 'abc'),
            'password' => env('DB_PASSWORD', 'abc'),
            'unix_socket' => env('DB_SOCKET', ''),
            'charset' => 'utf8mb4',
            'collation' => 'utf8mb4_unicode_ci',
            'prefix' => '',
            'strict' => true,
            'engine' => 'InnoDB',
        ],


    ]
```

3. 在config/database.php中設定預設會連線的database是哪一個：
```
'default' => 'db1',
``` 
原則上這樣的處理方式就可以使用了。不過如果是設定利用Eloquent撈資料的話，要注意一下在建立model後要先做一些設定：

```
class Room extends Model
{
    private $connection = 'db1';

    protected $table = "table1";
    protected $fillable = [];
}
```

這樣就可以在使用Eloquent的時候連到對的table上了。


