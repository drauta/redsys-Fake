Redsys Fake
=====

Esta utilidad te permitirá crear tu propio servicio de pagos y validación de compras para pruebas de integración de la pasarela de pago de Redsys (antes Sermepa / Servired).

## Instalación

Instala la aplicación en un directorio accesible vía web. Te recomiendo crear el entorno bajo un subdominio propio (o un alias) para poder recibir peticiones para todos los entornos que necesites probar.

```bash
git clone https://github.com/eusonlito/redsys-Fake.git /var/www/redsys-fake

cd redsys-fake

composer update
```

Configura el acceso en Apache o Nginx

```
<VirtualHost *:80>
  ServerName redsys-fake.mydomain.com

  DocumentRoot /var/www/redsys-fake/public

  ErrorLog /var/log/apache2/redsys-fake.mydomain.com-error.log
  CustomLog /var/log/apache2/redsys-fake.mydomain.com-access.log combined

  <Directory /var/www/redsys-fake>
    Options -Indexes
    AllowOverride all
  </Directory>
</VirtualHost>
```

## Integración

Para una sencilla integración de la pasarela, te recomiendo instalar https://github.com/eusonlito/redsys-TPV en tu desarrollo.

## Configuración

Crea un duplicado del fichero `config.php` como `config.local.php`.

Configura el fichero con los parámetros necesarios:

```php
return array(
    // Signature Key
    'Key' => 'qwertyasdf0123456789',

    // Basic auth to attach to Ds_Merchant_MerchantURL response if your environment is protected
    'basic_auth' => array(
        'user' => '',
        'password' => ''
    )
);
```

Ahora ya puedes enviar las peticiones a eses entorno como si fuera el entorno real.

## Configuración de la librería Redsys/TPV

Si estás usando la librería https://github.com/eusonlito/redsys-TPV en tu entorno, puedes configurarla de la siguiente manera para que se conecte a este nuevo entorno:

```php
$TPV = new Redsys\Tpv\Tpv(array(
    'environments' => array(
        'local' => 'http://redsys-fake.mydomain.com'
    ),

    'Environment' => 'local',
    'Key' => 'qwertyasdf0123456789', // Debe coincidir con el valor de Key del entorno de pruebas

    ....
));
```

Listo!
