[![GitHub tag (latest by date)](https://img.shields.io/github/v/tag/pparga/transbank-plugin-magento2-webpay-rest)](https://github.com/pparga/transbank-plugin-magento2-webpay-rest/releases/tag/1.1.8)
[![GitHub](https://img.shields.io/github/license/transbankdevelopers/transbank-plugin-magento2-webpay-rest)](LICENSE)
[![GitHub contributors](https://img.shields.io/github/contributors/transbankdevelopers/transbank-plugin-magento2-webpay-rest)](https://github.com/TransbankDevelopers/transbank-plugin-magento2-webpay-rest/graphs/contributors)
[![Build Status](https://api.travis-ci.com/TransbankDevelopers/transbank-plugin-magento2-webpay-rest.svg?branch=master)](https://app.travis-ci.com/github/TransbankDevelopers/transbank-plugin-magento2-webpay-rest)

# Transbank Magento2 Webpay Plugin
Plugin oficial de Webpay para Magento2

## Descripción

Este plugin **oficial** de Transbank te permite integrar Webpay fácilmente en tu sitio Magento2. Está desarrollado en base al [SDK oficial de PHP](https://github.com/TransbankDevelopers/transbank-sdk-php)

### ¿Cómo instalar?
Puedes ver las instrucciones de instalación y su documentación completa en [transbankdevelopers.cl/plugin/magento/](https://www.transbankdevelopers.cl/plugin/magento/)


### Paso a producción
Al instalar el plugin, este vendrá configurado para funcionar en modo '**integración**'(en el ambiente de pruebas de Transbank). Para poder operar con dinero real (ambiente de **producción**), debes:

1. Tener tu propio código de comercio. Si no lo tienes, solicita Webpay Plus en [transbank.cl](https://publico.transbank.cl) 
2. Completar el [formulario de validación](https://www.transbankdevelopers.cl/documentacion/como_empezar#el-proceso-de-validacion). Debes selecciona el link **Para integración con plugins**.
3. Configurar la API Key que te entregará Transbank en la configuración del Plugin.
4. Debes hacer una compra de $50 en el ambiente de producción para confirmar el correcto funcionamiento. 

Puedes ver más información sobre este proceso en [este link](https://www.transbankdevelopers.cl/documentacion/como_empezar#puesta-en-produccion).

# Desarrollo
A continuación, encontrarás información necesaria para el desarrollo de este plugin. 

## Dependencias

- Requiere [Composer](https://getcomposer.org)

El plugin depende de las siguientes librerías:

* transbank/transbank-sdk
* tecnickcom/tcpdf

## Nota  
- La versión del sdk de php se encuentra en el archivo `composer.json`
- La versión del plugin se encuentra en los archivos `composer.json` y `etc/module.xml`
- Recomendamos utilizar el docker de desarrollo si vas a modificar el código

## Instalación 

**NOTA**: El plugin se puede instalar de dos formas desde packagist.org o directamente desde el repositorio git.

1. Ir a la carpeta base de Magento2

2. [Opción 1] Ejecutar los siguientes comandos para instalar el plugin directamente desde packagist.org:

    ```bash
	composer require transbank/webpay-magento2-rest
    ```
   Esperar mientras las dependencias son actualizadas.

3. [Opción 2] Ejecutar los siguientes comandos para instalar el plugin directamente desde git:

    ```bash
    composer config repositories.transbankwebpay vcs https://github.com/TransbankDevelopers/transbank-plugin-magento2-webpay-rest.git
	composer require transbank/webpay-magento2-rest:dev-master
    ```
   Esperar mientras las dependencias son actualizadas.

4. Ejecutar los siguientes comandos para habilitar el modulo:

    ```bash
    magento module:enable Transbank_Webpay --clear-static-content
	magento setup:upgrade && magento setup:di:compile && magento setup:static-content:deploy
    ```
5. Habilitar y configurar el plugin Webpay en la sección de administración de magento2 bajo  Stores/Configuration/Payment Methods/Webpay

6. Configurar los certificados necesarios para que funcione el plugin de Webpay.

## Actualización

1. Ir a la carpeta base de Magento2

2. Ejecutar los siguientes comandos para actualizar el plugin

```bash
magento module:disable Transbank_Webpay --clear-static-content
composer update
magento module:enable Transbank_Webpay --clear-static-content
magento setup:upgrade && magento setup:di:compile && magento setup:static-content:deploy
```

# Otras Notas

Webpay solo trabaja con CLP! Si CLP no es tu moneda principal, no podrás usar este plugin en el proceso de checkout. Esto se encuentra en duro en [payment model](https://github.com/TransbankDevelopers/transbank-plugin-magento2-webpay/blob/master/Model/Webpay.php)

Si no sabes como realizar esta configuracion puedes verlo en [este documento](docs/INSTALLATION.md)

## Reinstalación

1. Ir a la carpeta base de Magento2

2. Ejecutar los siguientes comandos para deshabilitar y eliminar el plugin:

```bash
magento module:disable Transbank_Webpay --clear-static-content
magento module:uninstall Transbank_Webpay
```
** Debes aceptar la eliminación de tablas y código asociado al plugin.

3. Seguir el proceso de instalación descrito anteriormente.

## Ambiente de Desarrollo

Para apoyar el levantamiento rápido de un ambiente de desarrollo, hemos creado la especificación de contenedores a través de Docker Compose.

Para usarlo seguir el siguiente [README Magento 2](./docker-magento2)

### Crear el instalador del plugin

    ./package.sh

## Generar una nueva versión

Para generar una nueva versión, se debe crear un PR (con un título "Prepare release X.Y.Z" con los valores que correspondan para `X`, `Y` y `Z`). Se debe seguir el estándar semver para determinar si se incrementa el valor de `X` (si hay cambios no retrocompatibles), `Y` (para mejoras retrocompatibles) o `Z` (si sólo hubo correcciones a bugs).

En ese PR deben incluirse los siguientes cambios:

1. Modificar el archivo `CHANGELOG.md` para incluir una nueva entrada (al comienzo) para `X.Y.Z` que explique en español los cambios.
2. Modificar el archivo `etc/module.xml` y cambiar el valor de `setup_version` por el `X.Y.Z` nuevo.

Luego de obtener aprobación del pull request, debes mezclar a master e inmediatamente generar un release en GitHub con el tag `vX.Y.Z`. En la descripción del release debes poner lo mismo que agregaste al changelog.

Con eso Travis CI generará automáticamente una nueva versión del plugin y actualizará el Release de Github con el zip del plugin.
