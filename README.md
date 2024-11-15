# Guía de uso para el API  
## Índice

* [Trabajar con el API.](#trabajar-con-el-api)
    * [Identificarse para obtener un token](#identificarse-para-obtener-un-token)
* [Métodos de consulta](#métodos-de-consulta-disponibles)
    * [Búsqueda de una víctima por curp.](#búsqueda-de-una-víctima-por-curp)
    * [Búsqueda de una víctima por hecho_id.](#búsqueda-de-una-víctima-por-hecho_id)
    * [Búsqueda de agresores por hecho_id.](#búsqueda-de-agresores-por-hecho_id)
    * [Búsqueda de seguimientos por folio EUV y el hecho_id.](#búsqueda-de-seguimientos-por-folio-euv-y-el-hecho_id)
    * [Búsqueda de seguimientos por folio EUV.](#búsqueda-de-seguimientos-por-folio-euv)
    * [Búsqueda de folios por enlace estatal en un período de tiempo.](#búsqueda-de-folios-por-enlace-estatal-en-un-período-de-tiempo)
    * [Búsqueda de folios euv por estado en un período de tiempo.](#búsqueda-de-folios-euv-por-estado-en-un-período-de-tiempo)
    * [Búsqueda de hechos por id.](#búsqueda-de-hechos-por-id)
    * [Búsqueda de hechos por entidad y rango de fecha](#búsqueda-de-hechos-por-entidad-y-rango-de-fecha)
* [Métodos de registro disponibles](#métodos-de-registro-disponibles)
    * [Registro de víctima con CURP](#registro-de-víctima-con-curp)
    * [Registro de víctima con Datos](#registro-de-víctima-con-datos)
    * [Registro de víctima sin CURP](#registro-de-víctima-sin-curp)
    * [Registro de un hecho de violencia](#registro-de-un-hecho-de-violencia)
    * [Registro de los datos de una víctima](#registro-o-edición-de-los-datos-de-una-víctima) <sub><small>nuevo</small></sub>
    * [Registro de un dependiente de una víctima por hecho de violencia](#registro-de-un-dependiente-de-una-víctima-por-hecho-de-violencia) <sub><small>nuevo</small></sub>
    * [Registro de la red de apoyo de una víctima por hecho de violencia](#registro-de-la-red-de-apoyo-de-una-víctima-por-hecho-de-violencia) <sub><small>nuevo</small></sub>
    * [Registro del seguimiento de un hecho de violencia.](#registro-del-seguimiento-de-un-hecho-de-violencia)
    * [Registro de mujeres en prisión para un hecho de violencia](#registro-de-mujeres-en-prisión-para-un-hecho-de-violencia)
    * [Registro de mujeres víctimas de trata para un hecho de violencia](#registro-de-mujeres-víctimas-de-trata-para-un-hecho-de-violencia)
    * [Registro de mujeres desaparecidas para un hecho de violencia](#registro-de-mujeres-desaparecidas-para-un-hecho-de-violencia)
    * [Registro una canalización para un hecho de violencia](#registro-una-canalización-para-un-hecho-de-violencia)
    * [Registro de un agresor para un hecho de violencia](#registro-de-un-agresor-para-un-hecho-de-violencia)
* [Métodos de edición disponibles](#métodos-de-edición-disponibles)
    * [Editar hecho de violencia ](#editar-hecho-de-violencia) <sub><small>nuevo</small></sub>
    * [Editar los datos de una víctima](#registro-o-edición-de-los-datos-de-una-víctima) <sub><small>nuevo</small></sub>
    * [Editar registro de mujeres en prisión para un hecho de violencia](#editar-registro-de-mujeres-en-prisión-para-un-hecho-de-violencia)
    * [Editar registro de mujeres víctimas de trata](#editar-registro-de-mujeres-víctimas-de-trata)
* [Método de consulta personalizados](#métodos-de-consulta-personalizados-disponibles)
    * [Infoname](#infoname)
* [Métodos disponibles únicamente en el api de prueba](#métodos-de-prueba)
    * [Eliminar CURP mediante CURP](#eliminar-curp-mediante-curp)
    * [Eliminar CURP mediante EUV](#eliminar-curp-mediante-euv)
    * [Restaurar CURP mediante EUV](#restaurar-curp-mediante-euv)
--------------------------------------------------------------------------
--------------------------------------------------------------------------    
### Trabajar con el API
Para trabajar con el API, es fundamental realizar un proceso controlado que garantice que las integraciones funcionen correctamente antes de operar en un entorno real. Para lograrlo, se utilizan dos entornos diferenciados:

 - Entorno de Pruebas (IP Pruebas): Donde se llevan a cabo ensayos y validaciones sin afectar datos reales ni operaciones en curso.
 - Entorno de Producción: El entorno final en el que la API interactúa con datos reales y soporta la operación diaria.

La transición del entorno de pruebas al de producción es un paso clave, ya que asegura que la API esté lista para usarse en condiciones reales sin errores ni interrupciones. Este cambio se realiza únicamente al finalizar todas las pruebas necesarias, garantizando que la implementación en producción sea exitosa.
Utilizaremos variables de entorno para facilitar la gestión de dominios.
Ejemplo:
```text
API_URL=http://208.113.132.37:80                # Pruebas
API_URL=https://banavimnacional.segob.gob.mx    # Producción
```
La transición al entorno de producción **debe hacerse solo después de finalizar las pruebas** y garantizar que no haya errores. Esto asegura que la API funcione de manera eficiente en operaciones reales sin interrupciones por lo que durante la guia se utilizará el entorno de prubas.

----------------------------------------------------------------
### Identificarse para obtener un token.
----------------------------------------------------------------
Los tokens se generan con un algoritmo de combinación única de caracteres que determinan su duración y ámbito de acción. Para obtenerlo es necesario realizar una petición de tipo post con los parámetros de email y password, que son utilizados como credenciales y de ser correctas regresan como respuesta un json con un token generado. El tiempo de vida de un token es de 24 horas. La api usa el método bearer token por lo que deberá llevar un header de ese tipo.

#### POST
```
    API_URL/api/tokens/create
```
#### Body raw (json)
```json
{
    "email" : "CORREO",
    "password" : "CONTRASEÑA"
}
```
#### Campos Obligatorios

|   Campo  | Obligatorio | Tipo de dato |
|:--------:|:-----------:|:------------:|
| email    |      SI     |    string    |
| password |      SI     |    string    |

#### Ejemplo de solicitud.
con curl.
```bash
    curl --location 'API_URL/api/tokens/create' \
    --header 'Accept: application/json' \
    --header 'Content-Type: application/json' \
    --data-raw '{
        "email" : "CORREO",
        "password" : "CONTRASEÑA"
    }'
```
con PHP - cURL.
```php
    $curl = curl_init();

    curl_setopt_array($curl, array(
    CURLOPT_URL => 'API_URL/api/tokens/create',
    CURLOPT_RETURNTRANSFER => true,
    CURLOPT_ENCODING => '',
    CURLOPT_MAXREDIRS => 10,
    CURLOPT_TIMEOUT => 0,
    CURLOPT_FOLLOWLOCATION => true,
    CURLOPT_HTTP_VERSION => CURL_HTTP_VERSION_1_1,
    CURLOPT_CUSTOMREQUEST => 'POST',
    CURLOPT_POSTFIELDS =>'{
        "email" : "CORREO",
        "password" : "CONTRASEÑA"
    }',
    CURLOPT_HTTPHEADER => array(
        'Accept: application/json',
        'Content-Type: application/json'
    ),
    ));

    $response = curl_exec($curl);

    curl_close($curl);
    echo $response;    
```
con JavaScrip - fetch
```JavaScript
    const myHeaders = new Headers();
    myHeaders.append("Accept", "application/json");
    myHeaders.append("Content-Type", "application/json");
    const raw = JSON.stringify({
    "email": "CORREO",
    "password": "CONTRASEÑA"
    });
    const requestOptions = {
    method: "POST",
    headers: myHeaders,
    body: raw,
    redirect: "follow"
    };
    fetch("API_URL/api/tokens/create", requestOptions)
    .then((response) => response.text())
    .then((result) => console.log(result))
    .catch((error) => console.error(error));
```
#### La respuesta que se recibirá después de la petición.
con estatus 200  en formato json.
```json
    {
        "token": "cadena_de_caracteres_token"
    }
```
con estatus 422. Email incorrecto en formato json.
```json
    {
        "message": "El formato del dirección de correo electrónico no es válido.",
        "errors": {
            "email": [
                "El formato del dirección de correo electrónico no es válido."
            ]
        }
    }
```
con estatus 422. Password incorrecto en formato json.
```json
    {
        "message": "El password es incorrecto",
        "errors": {
            "email": [
                "El password es incorrecto"
            ]
        }
    }
```
Los métodos siguientes utilizan el Bearer token para autenticar al usuario que consume los servicios.

-----------------------------------------------------------------------------
### Búsqueda de una víctima por curp
-----------------------------------------------------------------------------
El curp es una clave única e intransferible que identifica a una persona que habita nuestro país, así como mexicanos que radican en el extranjero. La búsqueda por curp permite tener mayor certeza de identificación de los datos de la víctima.

#### POST
```
    API_URL/api/curp
```
#### Body raw (json)
```json
    {
        "curp" : "CURP000000XXXXXXX0"
    }
```
#### Campos Obligatorios

|   Campo  | Obligatorio | Tipo de dato |
|:--------:|:-----------:|:------------:|
| curp     |      SI     |    string    | 

#### Ejemplo de solicitud.
con curl.
```bash
    curl --location 'API_URL/api/curp' \
    --header 'Accept: application/json' \
    --header 'Content-Type: application/json' \
    --header 'Authorization: Bearer TOKEN' \
    --data '{
        "curp" : "CURP000000XXXXXXX0"
    }'
```
con PHP - cURL.
```php
    $curl = curl_init();

    curl_setopt_array($curl, array(
    CURLOPT_URL => 'API_URL/api/curp',
    CURLOPT_RETURNTRANSFER => true,
    CURLOPT_ENCODING => '',
    CURLOPT_MAXREDIRS => 10,
    CURLOPT_TIMEOUT => 0,
    CURLOPT_FOLLOWLOCATION => true,
    CURLOPT_HTTP_VERSION => CURL_HTTP_VERSION_1_1,
    CURLOPT_CUSTOMREQUEST => 'POST',
    CURLOPT_POSTFIELDS =>'{
        "curp" : "CURP000000XXXXXXX0"
    }',
    CURLOPT_HTTPHEADER => array(
        'Accept: application/json',
        'Content-Type: application/json',
        'Authorization: Bearer TOKEN'
    ),
    ));

    $response = curl_exec($curl);

    curl_close($curl);
    echo $response;
```
con JavaScrip - fetch.
```javascript
    const myHeaders = new Headers();
    myHeaders.append("Accept", "application/json");
    myHeaders.append("Content-Type", "application/json");
    myHeaders.append("Authorization", "Bearer TOKEN");

    const raw = JSON.stringify({
    "curp": "CURP000000XXXXXXX0"
    });

    const requestOptions = {
    method: "POST",
    headers: myHeaders,
    body: raw,
    redirect: "follow"
    };

    fetch("API_URL/api/curp", requestOptions)
    .then((response) => response.text())
    .then((result) => console.log(result))
    .catch((error) => console.error(error));
```
#### La respuesta que se recibirá después de la petición.
con estatus 200  en formato json.
```json
    {
        "curp": "CURP000000XXXXXXX0",
        "folio_euv": "FOLIOOO-EUV6-381f-a2cb-33372d4ad034"
    }
```
con estatus 422. Curp no está registrado.
```json
{
    "message": "El CURP no está registrado en el sistema",
    "errors": {
        "curp": [
            "El CURP no está registrado en el sistema"
        ]
    }
}
```
---------------------------------------------------------------
### Búsqueda de una víctima por hecho_id.
---------------------------------------------------------------
El método permite identificar a la víctima por el hecho de violencia. La búsqueda se realiza a través de envíar por la url el hecho_id.

#### GET
```
    API_URL/api/datos-victima-por-hecho/{hecho_id}
```
#### Ejemplo de solicitud.
con curl.
```bash
    curl --location 'API_URL/api/datos-victima-por-hecho/10' \
    --header 'Authorization: Bearer TOKEN'
```
con PHP - cURL.
```php
    $curl = curl_init();

    curl_setopt_array($curl, array(
    CURLOPT_URL => 'API_URL/api/datos-victima-por-hecho/10',
    CURLOPT_RETURNTRANSFER => true,
    CURLOPT_ENCODING => '',
    CURLOPT_MAXREDIRS => 10,
    CURLOPT_TIMEOUT => 0,
    CURLOPT_FOLLOWLOCATION => true,
    CURLOPT_HTTP_VERSION => CURL_HTTP_VERSION_1_1,
    CURLOPT_CUSTOMREQUEST => 'GET',
    CURLOPT_HTTPHEADER => array(
        'Authorization: Bearer TOKEN'
    ),
    ));

    $response = curl_exec($curl);

    curl_close($curl);
    echo $response;
```
con JavaScrip - fetch
```javascript
    const myHeaders = new Headers();
    myHeaders.append("Authorization", "Bearer TOKEN");

    const requestOptions = {
    method: "GET",
    headers: myHeaders,
    redirect: "follow"
    };

    fetch("API_URL/api/datos-victima-por-hecho/10", requestOptions)
    .then((response) => response.text())
    .then((result) => console.log(result))
    .catch((error) => console.error(error));
```
#### La respuesta que se recibirá después de la petición.
con estatus 200  en formato json.
```json
    {
        "id": 10,
        "hechos_id": 10,
        "users_id": 226,
        "edad": 84,
        "telefono": "+1-680-943-4840",
        "correo_electronico": "kop.mpetz@hotmail.com",
        "calle": "Jaylan Point",
        "num_exterior": "657",
        "num_interior": "81658",
        "cve_ent": 11,
        "cve_mun": 4,
        "cve_loc": 4,
        "colonias_id": null,
        "entre_calle_uno": "Gerhold Flats",
        "entre_calle_dos": "Swift Oval",
        "referencia": "Voluptas eum moluptasnde corporis aut sunt voluptatem.",
        "estado_conyugal": {
            "id": 8,
            "pk_catalogo": 3,
            "descripcion_mujer": "Viuda",
            "descripcion_hombre": "Viudo",
            "orden": 4,
            "es_activo": true,
            "created_at": "2023-12-01T17:04:35.000000Z",
            "updated_at": "2023-12-01T17:04:35.000000Z"
        },
        "pais": 238,
        "codigo_postal": "77220",
        "escolaridad_id": 8,
        "ingreso_economico_id": 5,
        "ocupacion_id": 30,
        "presenta_discapacidad": false,
        "discapacidad": "[4]",
        "otra_discapacidad": "Unde minima qui error magni. Provident  beatae.",
        "enfermedad": true,
        "cual_enfermedad": "Ipsum omnis possimus ut aepe.",
        "enfermedad_psiquiatrica": true,
        "cual_enfermedad_psiquiatrica": "Suscipit sequi fuga et est.m beatae.",
        "tratamiento": true,
        "cual_tratamiento": "Rem ut esse em illum magni ex cupiditate quo earum.",
        "situacion_calle": false,
        "migrante": false,
        "pueblo_indigena": true,
        "pueblo_indigena_id": 53,
        "lgbtti": true,
        "identidad_genero_id": 3,
        "orientacion_sexual_id": 4,
        "embarazada": false,
        "numero_semanas_emb": null,
        "dependientes": false,
        "red_apoyo": false,
        "adiccion": true,
        "created_at": "2023-10-31T19:51:47.000000Z",
        "updated_at": "2023-10-31T19:51:47.000000Z",
        "cual_adiccion": "[2]",
        "deleted_at": null,
        "persona_afrodescendiente": null,
        "caso_name": null,
        "realiza_mas_actividades": null,
        "discapacidad_por_violencia": null,
        "gestacion_id": null,
        "clave_centro_escolar": null,
        "numero_seguro_social": null,
        "ocupacion": {
            "id": 30,
            "descripcion_hombre": "VETERINARIO",
            "descripcion_mujer": "VETERINARIA",
            "esactivo": true,
            "created_at": "2023-12-01T17:04:36.000000Z",
            "updated_at": "2023-12-01T17:04:36.000000Z"
        },
        "escolaridad": {
            "id": 8,
            "pk_catalogo": 9,
            "descripcion": "No identificado",
            "orden": 11,
            "es_activo": true,
            "created_at": "2023-12-01T17:04:35.000000Z",
            "updated_at": "2023-12-01T17:04:35.000000Z"
        },
        "ingreso_economico": {
            "id": 5,
            "descripcion": "$25,846.09 A $43,076.80 (HASTA 10 SALARIOS MÍNIMOS)",
            "esactivo": true,
            "created_at": "2023-12-01T17:04:35.000000Z",
            "updated_at": "2023-12-01T17:04:35.000000Z"
        },
        "colonia": null,
        "identidad_genero": {
            "id": 3,
            "descripcion": "TRANSGÉNERO",
            "esactivo": true,
            "created_at": "2023-12-01T17:04:36.000000Z",
            "updated_at": "2023-12-01T17:04:36.000000Z"
        },
        "orientacion_sexual": {
            "id": 4,
            "descripcion": "LESBIANA",
            "esactivo": true,
            "created_at": "2023-12-01T17:04:36.000000Z",
            "updated_at": "2023-12-01T17:04:36.000000Z"
        },
        "semanas_gestacion": {
            "id": null
        }
    }
```
----------------------------------------------------------------
### Búsqueda de agresores por hecho_id.
----------------------------------------------------------------
Los agresores que realizaron un hecho de violencia están registrados y pueden ser consultados por este método con el hecho_id.

#### GET
```
    API_URL/api/datos-agresor-por-hecho/{hecho_id}
```
#### Ejemplo de solicitud.
con curl.
```bash
    curl --location 'API_URL/api/datos-agresor-por-hecho/3' \
    --header 'Authorization: Bearer TOKEN'
```
con PHP - cURL.
```php
    <?php

    $curl = curl_init();

    curl_setopt_array($curl, array(
    CURLOPT_URL => 'API_URL/api/datos-agresor-por-hecho/3',
    CURLOPT_RETURNTRANSFER => true,
    CURLOPT_ENCODING => '',
    CURLOPT_MAXREDIRS => 10,
    CURLOPT_TIMEOUT => 0,
    CURLOPT_FOLLOWLOCATION => true,
    CURLOPT_HTTP_VERSION => CURL_HTTP_VERSION_1_1,
    CURLOPT_CUSTOMREQUEST => 'GET',
    CURLOPT_HTTPHEADER => array(
        'Authorization: TOKEN'
    ),
    ));

    $response = curl_exec($curl);

    curl_close($curl);
    echo $response;
```
con JavaScrip - fetch
```javascript
    const myHeaders = new Headers();
    myHeaders.append("Authorization", "Bearer TOKEN");

    const requestOptions = {
    method: "GET",
    headers: myHeaders,
    redirect: "follow"
    };

    fetch("API_URL/api/datos-agresor-por-hecho/3", requestOptions)
    .then((response) => response.text())
    .then((result) => console.log(result))
    .catch((error) => console.error(error));
```
#### La respuesta que se recibirá después de la petición.
con estatus 200  en formato json.
```json
[
    {
        "id": 20,
        "hechos_id": 3,
        "users_id": 1,
        "nombre": "nombre",
        "primer_apellido": "primer_apellido",
        "segundo_apellido": "segundo_apellido",
        "curp": "CURP000000XXXXXXX0",
        "sexo": "masculino",
        "edad": 40,
        "mismo_domicilio_victima": false,
        "acceso_a_armas": false,
        "vinculo_victima_id": 5,
        "identidad_genero_id": 1,
        "orientacion_sexual_id": 2,
        "calle": "X",
        "num_exterior": "3",
        "num_interior": "7",
        "cve_ent": 21,
        "cve_mun": 114,
        "cve_loc": 501,
        "codigo_postal": "72550",
        "colonias_id": null,
        "entre_calle_uno": null,
        "entre_calle_dos": null,
        "referencia": null,
        "escolaridad_id": 10,
        "ingreso_economico_id": 3,
        "ocupacion_id": 9,
        "created_at": "2023-12-13T23:50:30.000000Z",
        "updated_at": "2023-12-13T23:50:30.000000Z",
        "conoce_persona": true,
        "deleted_at": null,
        "sexo_id": null,
        "tipos_armas": "[]",
        "consume_drogas": null,
        "tipos_drogas": "[]"
    }
]
```
----------------------------------------------------------------
### Búsqueda de seguimientos por folio EUV y el hecho_id.
----------------------------------------------------------------
Esté método permite buscar seguimientos con el folio EUV y el hecho_id 
#### POST
```
    API_URL/api/seguimientos-por-hecho
```
#### Body raw (json)
```json
    {
        "folio_euv" : "folio_euv-31ec-3455-a838-f34fb5c53dca",
        "hecho_id" : 1382
    }
```
#### Campos Obligatorios

|   Campo  | Obligatorio | Tipo de dato |
|:--------:|:-----------:|:------------:|
|folio_euv |      SI     |    string    |
|hecho_id  |      SI     |    string    |

#### Ejemplo de solicitud.
con curl.
```bash
    curl --location 'API_URL/api/seguimientos-por-hecho' \
    --header 'Content-Type: application/json' \
    --header 'Authorization: Bearer TOKEN' \
    --data '{
        "folio_euv" : "folio_euv-31ec-3455-a838-f34fb5c53dca",
        "hecho_id" : 1382
    }'
```
con PHP - cURL.
```php
    <?php

    $curl = curl_init();

    curl_setopt_array($curl, array(
    CURLOPT_URL => 'API_URL/api/seguimientos-por-hecho',
    CURLOPT_RETURNTRANSFER => true,
    CURLOPT_ENCODING => '',
    CURLOPT_MAXREDIRS => 10,
    CURLOPT_TIMEOUT => 0,
    CURLOPT_FOLLOWLOCATION => true,
    CURLOPT_HTTP_VERSION => CURL_HTTP_VERSION_1_1,
    CURLOPT_CUSTOMREQUEST => 'POST',
    CURLOPT_POSTFIELDS =>'{
        "folio_euv" : "folio_euv-31ec-3455-a838-f34fb5c53dca",
        "hecho_id" : 1382
    }',
    CURLOPT_HTTPHEADER => array(
        'Content-Type: application/json',
        'Authorization: Bearer TOKEN'),
    ));

    $response = curl_exec($curl);

    curl_close($curl);
    echo $response;
```
con JavaScrip - fetch
```javascript
    const myHeaders = new Headers();
    myHeaders.append("Content-Type", "application/json");
    myHeaders.append("Authorization", "Bearer TOKEN");

    const raw = JSON.stringify({
    "folio_euv": "folio_euv-31ec-3455-a838-f34fb5c53dca",
    "hecho_id": 1382
    });

    const requestOptions = {
    method: "POST",
    headers: myHeaders,
    body: raw,
    redirect: "follow"
    };

    fetch("API_URL/api/seguimientos-por-hecho", requestOptions)
    .then((response) => response.text())
    .then((result) => console.log(result))
    .catch((error) => console.error(error));
```
#### La respuesta que se recibirá después de la petición.
con estatus 200  en formato json.
```json
[
    {
        "id": 2445,
        "users_id": 755,
        "hechos_id": 1382,
        "fecha_seguimiento": "2004-02-21",
        "hora_seguimiento": "16:44:21",
        "tipo_id": 7,
        "servicios_id": 26,
        "acciones": "Excepturi qciendis ut facilis illum dolorem.",
        "observaciones": "Doloremque non modi necessitatibus natus.",
        "created_at": "2023-10-31T19:53:20.000000Z",
        "updated_at": "2023-10-31T19:53:20.000000Z",
        "deleted_at": null,
        "servicio": {
            "id": 26,
            "tipo_servicio_id": 7,
            "descripcion": "PONER EN OBSERVACIONES",
            "esactivo": true,
            "created_at": "2023-12-01T17:04:35.000000Z",
            "updated_at": "2023-12-01T17:04:35.000000Z"
        },
        "tipo_servicio": {
            "id": 7,
            "descripcion": "ACTIVIDAD NO CATALOGADA",
            "esactivo": false,
            "created_at": "2023-12-01T17:04:35.000000Z",
            "updated_at": "2023-12-01T17:04:35.000000Z"
        }
    },
    {
        "id": 2446,
        "users_id": 755,
        "hechos_id": 1382,
        "fecha_seguimiento": "2003-09-11",
        "hora_seguimiento": "12:30:11",
        "tipo_id": 7,
        "servicios_id": 26,
        "acciones": "Est sequlitia quis et id. Illum veritatis ab harum voluptatum expedita.",
        "observaciones": "Voluptates tetur alias harum atque animi non.",
        "created_at": "2023-10-31T19:53:20.000000Z",
        "updated_at": "2023-10-31T19:53:20.000000Z",
        "deleted_at": null,
        "servicio": {
            "id": 26,
            "tipo_servicio_id": 7,
            "descripcion": "PONER EN OBSERVACIONES",
            "esactivo": true,
            "created_at": "2023-12-01T17:04:35.000000Z",
            "updated_at": "2023-12-01T17:04:35.000000Z"
        },
        "tipo_servicio": {
            "id": 7,
            "descripcion": "ACTIVIDAD NO CATALOGADA",
            "esactivo": false,
            "created_at": "2023-12-01T17:04:35.000000Z",
            "updated_at": "2023-12-01T17:04:35.000000Z"
        }
    }
]
```
--------------------------------------------------------------
### Búsqueda de seguimientos por folio EUV.
--------------------------------------------------------------
Esté método permite buscar seguimientos con el folio EUV.
#### POST
```
    API_URL/api/seguimientos-por-euv
```
#### Body raw (json)
```json
    {
        "folio_euv" : "folio_euv-31ec-3455-a838-f34fb5c53dca"
    }
```
#### Campos Obligatorios

|   Campo  | Obligatorio | Tipo de dato |
|:--------:|:-----------:|:------------:|    
|folio_euv |      SI     |    string    | 

#### Ejemplo de solicitud.
con curl.
```bash
    curl --location 'API_URL/api/seguimientos-por-euv' \
    --header 'Content-Type: application/json' \
    --header 'Authorization: TOKEN' \
    --data '{
        "folio_euv" : "folio_euv-31ec-3455-a838-f34fb5c53dca"
    }'
```
con PHP - cURL.
```php
    <?php

    $curl = curl_init();

    curl_setopt_array($curl, array(
    CURLOPT_URL => 'API_URL/api/seguimientos-por-euv',
    CURLOPT_RETURNTRANSFER => true,
    CURLOPT_ENCODING => '',
    CURLOPT_MAXREDIRS => 10,
    CURLOPT_TIMEOUT => 0,
    CURLOPT_FOLLOWLOCATION => true,
    CURLOPT_HTTP_VERSION => CURL_HTTP_VERSION_1_1,
    CURLOPT_CUSTOMREQUEST => 'POST',
    CURLOPT_POSTFIELDS =>'{
        "folio_euv" : "folio_euv-31ec-3455-a838-f34fb5c53dca"
    }',
    CURLOPT_HTTPHEADER => array(
        'Content-Type: application/json',
        'Authorization: Bearer TOKEN'
    ),
    ));

    $response = curl_exec($curl);

    curl_close($curl);
    echo $response;
```
con JavaScrip - fetch
```javascript
    const myHeaders = new Headers();
    myHeaders.append("Content-Type", "application/json");
    myHeaders.append("Authorization", "Bearer TOKEN");

    const raw = JSON.stringify({
    "folio_euv": "folio_euv-31ec-3455-a838-f34fb5c53dca"
    });

    const requestOptions = {
    method: "POST",
    headers: myHeaders,
    body: raw,
    redirect: "follow"
    };

    fetch("API_URL/api/seguimientos-por-euv", requestOptions)
    .then((response) => response.text())
    .then((result) => console.log(result))
    .catch((error) => console.error(error));
```
#### La respuesta que se recibirá después de la petición.
con estatus 200  en formato json.
```javascript
[
    {
        "id": 2445,
        "users_id": 755,
        "hechos_id": 1382,
        "fecha_seguimiento": "2004-02-21",
        "hora_seguimiento": "16:44:21",
        "tipo_id": 7,
        "servicios_id": 26,
        "acciones": "Excepturi quidis ut facilis illum dolorem.",
        "observaciones": "Doloremque ulpa non modi necessitatibus natus.",
        "created_at": "2023-10-31T19:53:20.000000Z",
        "updated_at": "2023-10-31T19:53:20.000000Z",
        "deleted_at": null,
        "servicio": {
            "id": 26,
            "tipo_servicio_id": 7,
            "descripcion": "PONER EN OBSERVACIONES",
            "esactivo": true,
            "created_at": "2023-12-01T17:04:35.000000Z",
            "updated_at": "2023-12-01T17:04:35.000000Z"
        },
        "tipo_servicio": {
            "id": 7,
            "descripcion": "ACTIVIDAD NO CATALOGADA",
            "esactivo": false,
            "created_at": "2023-12-01T17:04:35.000000Z",
            "updated_at": "2023-12-01T17:04:35.000000Z"
        }
    },
    {
        "id": 2446,
        "users_id": 755,
        "hechos_id": 1382,
        "fecha_seguimiento": "2003-09-11",
        "hora_seguimiento": "12:30:11",
        "tipo_id": 7,
        "servicios_id": 26,
        "acciones": "Est sequi itatis ab harum voluptatum expedita.",
        "observaciones": "Volnsectetur alias harum atque animi non.",
        "created_at": "2023-10-31T19:53:20.000000Z",
        "updated_at": "2023-10-31T19:53:20.000000Z",
        "deleted_at": null,
        "servicio": {
            "id": 26,
            "tipo_servicio_id": 7,
            "descripcion": "PONER EN OBSERVACIONES",
            "esactivo": true,
            "created_at": "2023-12-01T17:04:35.000000Z",
            "updated_at": "2023-12-01T17:04:35.000000Z"
        },
        "tipo_servicio": {
            "id": 7,
            "descripcion": "ACTIVIDAD NO CATALOGADA",
            "esactivo": false,
            "created_at": "2023-12-01T17:04:35.000000Z",
            "updated_at": "2023-12-01T17:04:35.000000Z"
        }
    }
]
```
--------------------------------------------------------------
### Búsqueda de folios por enlace estatal en un período de tiempo.
--------------------------------------------------------------
El método requiere que las fechas sean válidas, en orden con la fecha inicial y la final, utilizando el enlace_id correspondiente a su estado.
#### POST
```
    API_URL/api/folios-euv-por-enlace-estatal
```
#### Body raw (json)
```json
    {
        "fecha_inicial" : "2022-5-27",
        "fecha_final"   : "2024-7-27",
        "enlace_id"     : 1
    }
```
#### Campos Obligatorios

|      Campo     | Obligatorio | Tipo de dato |
|:--------------:|:-----------:|:------------:|
| fecha_inicial  |     SI      |     date     | 
| fecha_final    |     SI      |     date     |
| enlace_id      |     SI      |    integer   |

#### Ejemplo de solicitud.
con curl.
```bash
    curl --location 'API_URL/api/folios-euv-por-enlace-estatal' \
    --header 'Accept: application/json' \
    --header 'Content-Type: application/json' \
    --header 'Authorization: Bearer TOKEN' \
    --data '{
        "fecha_inicial" : "2022-5-27",
        "fecha_final"   : "2024-7-27",
        "enlace_id"     : 1
    }'
```
con PHP - cURL.
```php
    <?php

    $curl = curl_init();

    curl_setopt_array($curl, array(
    CURLOPT_URL => 'API_URL/api/folios-euv-por-enlace-estatal',
    CURLOPT_RETURNTRANSFER => true,
    CURLOPT_ENCODING => '',
    CURLOPT_MAXREDIRS => 10,
    CURLOPT_TIMEOUT => 0,
    CURLOPT_FOLLOWLOCATION => true,
    CURLOPT_HTTP_VERSION => CURL_HTTP_VERSION_1_1,
    CURLOPT_CUSTOMREQUEST => 'POST',
    CURLOPT_POSTFIELDS =>'{
        "fecha_inicial" : "2022-5-27",
        "fecha_final"   : "2024-7-27",
        "enlace_id"     : 1
    }',
    CURLOPT_HTTPHEADER => array(
        'Accept: application/json',
        'Content-Type: application/json',
        'Authorization: Bearer TOKEN'
    ));

    $response = curl_exec($curl);

    curl_close($curl);
    echo $response;
```
con JavaScrip - fetch.
```javascript
    const myHeaders = new Headers();
    myHeaders.append("Accept", "application/json");
    myHeaders.append("Content-Type", "application/json");
    myHeaders.append("Authorization", "Bearer TOKEN");

    const raw = JSON.stringify({
    "fecha_inicial": "2022-5-27",
    "fecha_final": "2024-7-27",
    "enlace_id": 1
    });

    const requestOptions = {
    method: "POST",
    headers: myHeaders,
    body: raw,
    redirect: "follow"
    };

    fetch("API_URL/api/folios-euv-por-enlace-estatal", requestOptions)
    .then((response) => response.text())
    .then((result) => console.log(result))
    .catch((error) => console.error(error));
```
#### La respuesta que se recibirá después de la petición.
con estatus 200  en formato json.
```json
[
    "EUV000000000005234072024",
    "EUV000000000005235072024",
    "EUV000000000005236072024",
    "EUV000000000005237072024",
    "EUV000000000005238072024",
    "EUV000000000005239072024",
    "EUV000000000005240072024",
    "EUV000000000005241072024",
    "EUV000000000005242072024",
    "EUV000000000005243072024",
    "EUV000000000005248072024"
]
```
con estatus 422. Usuario no tiene permisos para realizar esta acción.
```json
    {
        "message": "El usuario no tiene permisos para realizar esta acción",
        "errors": {
            "user": [
                "El usuario no tiene permisos para realizar esta acción"
            ]
        }
    }
```

--------------------------------------------------------------
### Búsqueda de folios euv por estado en un período de tiempo.
--------------------------------------------------------------
El método requiere que las fechas sean válidas, en orden con la fecha inicial y la final, utilizando la clave de la entidad y la página.
#### POST
```
    API_URL/api/folios-euv-por-estado
```
catálogos: [cve_ent](https://drive.google.com/file/d/1Y163QX4ddN4J6w8ZGNUg_11-l4EM9V5w/view?usp=sharing)
#### Body raw (json)
```json
    {
        "fecha_inicial" : "2022-5-27",
        "fecha_final"   : "2024-5-27",
        "page" : 1,
        "cve_ent" : 21
    }
```
#### Campos Obligatorios

|     Campo    | Obligatorio | Tipo de dato |
|:------------:|:-----------:|:------------:|
|fecha_inicial |      SI     |     date     |
|fecha_final   |      SI     |     date     |
|page          |      SI     |    integer   |
|cve_ent       |      SI     |    integer   |

#### Ejemplo de solicitud.
con curl.
```bash
    curl --location 'API_URL/api/folios-euv-por-estado' \
    --header 'Accept: application/json' \
    --header 'Content-Type: application/json' \
    --header 'Authorization: Bearer TOKEN' \
    --data '{
        "fecha_inicial" : "2022-5-27",
        "fecha_final"   : "2024-5-27",
        "page" : 1,
        "cve_ent" : 21
    }'
```
con PHP - cURL.
```php
    <?php

    $curl = curl_init();

    curl_setopt_array($curl, array(
    CURLOPT_URL => 'API_URL/api/folios-euv-por-estado',
    CURLOPT_RETURNTRANSFER => true,
    CURLOPT_ENCODING => '',
    CURLOPT_MAXREDIRS => 10,
    CURLOPT_TIMEOUT => 0,
    CURLOPT_FOLLOWLOCATION => true,
    CURLOPT_HTTP_VERSION => CURL_HTTP_VERSION_1_1,
    CURLOPT_CUSTOMREQUEST => 'POST',
    CURLOPT_POSTFIELDS =>'{
        "fecha_inicial" : "2022-5-27",
        "fecha_final"   : "2024-5-27",
        "page" : 1,
        "cve_ent" : 21
    }',
    CURLOPT_HTTPHEADER => array(
        'Accept: application/json',
        'Content-Type: application/json',
        'Authorization: Bearer TOKEN'
    ),
    ));

    $response = curl_exec($curl);

    curl_close($curl);
    echo $response;
```
con JavaScrip - fetch.
```javascript
    const myHeaders = new Headers();
    myHeaders.append("Accept", "application/json");
    myHeaders.append("Content-Type", "application/json");
    myHeaders.append("Authorization", "Bearer TOKEN");

    const raw = JSON.stringify({
    "fecha_inicial": "2022-5-27",
    "fecha_final": "2024-5-27",
    "page": 1,
    "cve_ent": 21
    });

    const requestOptions = {
    method: "POST",
    headers: myHeaders,
    body: raw,
    redirect: "follow"
    };

    fetch("API_URL/api/folios-euv-por-estado", requestOptions)
    .then((response) => response.text())
    .then((result) => console.log(result))
    .catch((error) => console.error(error));
```
#### La respuesta que se recibirá después de la petición.
con estatus 200  en formato json.
```json
{
    "total": 185,
    "page": 1,
    "folios": [
        "folios-9ffd-30f0-b946-77216a37b78a",
        "folios-d24b-3337-b85d-df7cd57dbe0a",
        "folios-43d7-36d3-acc0-7b9ec22c36a5",
        "folios-ac12-3924-ac7b-45aeef3cf8ba",
        "folios-ba35-384e-a4a9-edc32d09b136",
        "folios-da44-3de6-a911-b470d0a9cb31",
        "folios-62ee-387f-8175-d96355f8e529",
        "folios-22be-3b44-8d2d-789baee3f8c0",
        "folios-42d2-33d3-bcff-42feab544d89"
    ]
}
```
con estatus 422. Error en la paginación.
```json
{
    "message": "El campo page debe tener al menos 1.",
    "errors": {
        "page": [
            "El campo page debe tener al menos 1."
        ]
    }
}
```
con estatus 422. Faltan parámetros.
```json
{
    "message": "El campo fecha inicial es requerido. (and 2 more errors)",
    "errors": {
        "fecha_inicial": [
            "El campo fecha inicial es requerido."
        ],
        "fecha_final": [
            "El campo fecha final es requerido."
        ],
        "cve_ent": [
            "El campo cve ent es requerido."
        ]
    }
}
```

--------------------------------------------------------------
### Búsqueda de Hechos por id.
--------------------------------------------------------------
#### POST
```
    API_URL/api/hecho
```
#### Body raw (json)
```json
    {
        "id" : 2
    }
```
#### Campos Obligatorios

|   Campo  | Obligatorio | Tipo de dato |
|:--------:|:-----------:|:------------:|
|   id     |     SI      |    integer   |

#### Ejemplo de solicitud.
con curl.
```bash
    curl --location 'API_URL/api/hecho' \
    --header 'Content-Type: application/json' \
    --header 'Authorization: Bearer TOKEN' \
    --data '{
        "id" : 2
    }'
```
con PHP - cURL.
```php
    <?php

    $curl = curl_init();

    curl_setopt_array($curl, array(
    CURLOPT_URL => 'API_URL/api/hecho',
    CURLOPT_RETURNTRANSFER => true,
    CURLOPT_ENCODING => '',
    CURLOPT_MAXREDIRS => 10,
    CURLOPT_TIMEOUT => 0,
    CURLOPT_FOLLOWLOCATION => true,
    CURLOPT_HTTP_VERSION => CURL_HTTP_VERSION_1_1,
    CURLOPT_CUSTOMREQUEST => 'POST',
    CURLOPT_POSTFIELDS =>'{
        "id" : 2
    }',
    CURLOPT_HTTPHEADER => array(
        'Content-Type: application/json',
        'Authorization: Bearer TOKEN'
    ),
    ));

    $response = curl_exec($curl);

    curl_close($curl);
    echo $response;
```
con JavaScrip - fetch.
```javascript
    const myHeaders = new Headers();
    myHeaders.append("Content-Type", "application/json");
    myHeaders.append("Authorization", "Bearer TOKEN");

    const raw = JSON.stringify({
    "id": 2
    });

    const requestOptions = {
    method: "POST",
    headers: myHeaders,
    body: raw,
    redirect: "follow"
    };

    fetch("API_URL/api/hecho", requestOptions)
    .then((response) => response.text())
    .then((result) => console.log(result))
    .catch((error) => console.error(error));
```
#### La respuesta que se recibirá después de la petición.
con estatus 200  en formato json.
```json
{
    "id": 2,
    "victimas_id": 5258,
    "users_id": 1095,
    "fecha_hechos": "2024-08-09",
    "hora_hechos": "01:19:00",
    "descripcion_hechos": "fsdfsfsf",
    "lugar_id": 3,
    "lugar_detalle_id": 36,
    "en_domicilio_victima": true,
    "pais_id": 238,
    "cve_ent": 9,
    "cve_mun": 14,
    "calle": "dgdg",
    "num_exterior_km": "100",
    "num_interior": "101",
    "cve_loc": 1,
    "cp": "06700",
    "lat": null,
    "lng": null,
    "es_festivo": false,
    "conoce_la_autoridad": false,
    "conoce_la_autoridad_detalle": "",
    "tipo_violencia": [
        "8"
    ],
    "modalidad_violencia": 1,
    "es_victima_de_delincuencia_organizada": false,
    "hay_denuncia": false,
    "efectos_fisicos": "{\"6\":\"6\",\"3\":\"3\"}",
    "consecuencias_sexuales": "{\"4\":\"4\"}",
    "efectos_psicologicos": "{\"19\":\"19\"}",
    "efectos_economicos_y_patrimoniales": "{\"3\":\"3\"}",
    "agente_de_lesion": "{\"2\":\"2\"}",
    "area_anatomica_lesionada": "{\"4\":\"4\"}",
    "es_relacionada_con_orientacion_o_identidad": false,
    "created_at": "2024-08-12T17:20:11.000000Z",
    "updated_at": "2024-08-12T17:20:11.000000Z",
    "deleted_at": null,
    "folio": "EUV1500A1R00005258082024-1",
    "colonias_id": 114929,
    "culmino_en_muerte": null,
    "tipo_muerte_id": null,
    "tipo_muerte_otro": null,
    "tipo_conducta_violencia_sexual_id": null,
    "tipo_violencia_sexual": null,
    "tipo_agresor_sexual_id": null,
    "tipo_agresor_sexual_otro": null,
    "victima": {
        "id": 5258,
        "nombre": "ELISA",
        "primer_apellido": "LOPEZ",
        "segundo_apellido": "x",
        "fecha_nacimiento": "2005-01-12",
        "cve_ent": 9,
        "created_at": "2024-08-12T17:18:32.000000Z",
        "updated_at": "2024-08-12T17:18:32.000000Z",
        "curp": "false",
        "folio_euv": "EUV1500A1R00005258082024",
        "sexo": null,
        "extranjera": false,
        "users_id": 1095,
        "instancias_id": null,
        "areas_id": null,
        "sedes_id": null,
        "nacionalidad_id": 1,
        "deleted_at": null,
        "sexo_id": 2,
        "from_api": false
    },
    "lugar": {
        "id": 3,
        "pk_catalogo": 1,
        "descripcion": "Espacio particular",
        "orden": 1,
        "es_activo": true,
        "created_at": "2023-12-01T17:04:30.000000Z",
        "updated_at": "2023-12-01T17:04:30.000000Z",
        "detalles_localidad": [
            {
                "id": 36,
                "pk_catalogo": 1,
                "descripcion": "Casa habitación",
                "fk_lugar_ocurrencia": 1,
                "orden": 1,
                "es_activo": true,
                "created_at": "2023-12-01T17:04:30.000000Z",
                "updated_at": "2023-12-01T17:04:30.000000Z"
            },
            {
                "id": 35,
                "pk_catalogo": 2,
                "descripcion": "Empresa",
                "fk_lugar_ocurrencia": 1,
                "orden": 2,
                "es_activo": true,
                "created_at": "2023-12-01T17:04:30.000000Z",
                "updated_at": "2023-12-01T17:04:30.000000Z"
            },
            {
                "id": 34,
                "pk_catalogo": 3,
                "descripcion": "Negocio",
                "fk_lugar_ocurrencia": 1,
                "orden": 3,
                "es_activo": true,
                "created_at": "2023-12-01T17:04:30.000000Z",
                "updated_at": "2023-12-01T17:04:30.000000Z"
            }
        ]
    },
    "lugar_detalle": {
        "id": 36,
        "pk_catalogo": 1,
        "descripcion": "Casa habitación",
        "fk_lugar_ocurrencia": 1,
        "orden": 1,
        "es_activo": true,
        "created_at": "2023-12-01T17:04:30.000000Z",
        "updated_at": "2023-12-01T17:04:30.000000Z"
    },
    "seguimientos": [],
    "canalizaciones": [],
    "ordenes": [],
    "victimas_trata": [],
    "mujeres_prision": [],
    "colonia": {
        "id": 114929,
        "cve_ent": 9,
        "cve_mun": 15,
        "cp": 6700,
        "descripcion": "ROMA NORTE",
        "created_at": "2024-08-08T23:00:59.000000Z",
        "updated_at": "2024-08-08T23:00:59.000000Z"
    }
}
```

--------------------------------------------------------------
### Búsqueda de hechos por entidad y rango de fecha.
--------------------------------------------------------------
El consumo de este método requiere cuatro parámetros que permitirá obtener la información de la entidad y rango de fecha.
#### POST
```
    API_URL/api/hechos
```
catálogos: [cve_ent](https://drive.google.com/file/d/1Y163QX4ddN4J6w8ZGNUg_11-l4EM9V5w/view?usp=sharing)
#### Body raw (json)
```json
    {
        "fecha_inicial" : "2022-5-27",
        "fecha_final": "2024-10-27",
        "page": 2,
        "cve_ent": 2
    }
```
#### Campos Obligatorios

|     Campo    | Obligatorio | Tipo de dato |
|:------------:|:-----------:|:------------:|
|fecha_inicial |      No     |     date     |
|fecha_final   |      No     |     date     |
|page          |      No     |    integer   |
|cve_ent       |      No     |    integer   |

#### Ejemplo de solicitud.
con curl.
```bash
curl --location 'API_URL/api/hechos' \
--header 'Accept: application/json' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer TOKEN' \
--data '{
    "fecha_inicial" : "2022-5-27",
    "fecha_final": "2024-10-25",
    "page": 2,
    "cve_ent": 2
}'
```
con PHP - cURL.
```php
    <?php

    $curl = curl_init();

    curl_setopt_array($curl, array(
    CURLOPT_URL => 'API_URL/api/hechos',
    CURLOPT_RETURNTRANSFER => true,
    CURLOPT_ENCODING => '',
    CURLOPT_MAXREDIRS => 10,
    CURLOPT_TIMEOUT => 0,
    CURLOPT_FOLLOWLOCATION => true,
    CURLOPT_HTTP_VERSION => CURL_HTTP_VERSION_1_1,
    CURLOPT_CUSTOMREQUEST => 'POST',
    CURLOPT_POSTFIELDS =>'{
        "fecha_inicial" : "2022-5-27",
        "fecha_final": "2024-10-25",
        "page": 2,
        "cve_ent": 2
    }',
    CURLOPT_HTTPHEADER => array(
        'Accept: application/json',
        'Content-Type: application/json',
        'Authorization: Bearer TOKEN'
    ),
    ));

    $response = curl_exec($curl);

    curl_close($curl);
    echo $response;
```
con JavaScrip - fetch.
```javascript
const myHeaders = new Headers();
myHeaders.append("Accept", "application/json");
myHeaders.append("Content-Type", "application/json");
myHeaders.append("Authorization", "Bearer TOKEN");

const raw = JSON.stringify({
  "fecha_inicial": "2022-5-27",
  "fecha_final": "2024-10-25",
  "page": 2,
  "cve_ent": 2
});

const requestOptions = {
  method: "POST",
  headers: myHeaders,
  body: raw,
  redirect: "follow"
};

fetch("API_URL/api/hechos", requestOptions)
  .then((response) => response.text())
  .then((result) => console.log(result))
  .catch((error) => console.error(error));
```
#### La respuesta que se recibirá después de la petición.
con estatus 200  en formato json.
```json
{
    "total": 559,
    "page": 1,
    "pages": 56,
    "hechos_id": [
        3,
        6,
        7,
        8,
        9,
        11,
        12,
        13,
        14,
        15
    ]
}
```
con estatus 422. El campo fecha final debe ser una fecha valida.
```json
    {
        "message": "El campo fecha final debe ser una fecha antes de tomorrow.",
        "errors": {
            "fecha_final": [
                "El campo fecha final debe ser una fecha antes de tomorrow."
            ]
        }
    }
```
--------------------------------------------------------------
### Registro de víctima con CURP
--------------------------------------------------------------
El consumo de este método requiere cuatro parámetros de la víctima debido a que realiza una consulta a la renapo permitiendo traer el registro de sus datos.

#### POST
```
    API_URL/api/registrar-victima-con-curp
```
catálogos: [cve_ent](https://drive.google.com/file/d/1Y163QX4ddN4J6w8ZGNUg_11-l4EM9V5w/view?usp=sharing), [nacionalidad_id](https://drive.google.com/file/d/1Q0gUDPgv9_3xfmPYPJ6WLfrUrFdqvEGB/view)
#### Body raw (json)
```json
{
    "curp"              : "FOGW030824MTSLRNA5",
    "cve_ent"           : 22,
    "nacionalidad_id"   : 1,
    "extranjera"        : false,
    "identifica_mujer"  : true
}
```
#### Campos Obligatorios

|         Campo        | Obligatorio | Tipo de dato |
|:--------------------:|:-----------:|:------------:|
| curp                 |      SI     |    string    |
| cve_ent              |      SI     |    integer   | 
| nacionalidad_id      |      NO     |    integer   |
| extranjera           |      NO     |    boolean   |
| identifica_mujer     |      NO     |    boolean   |

#### Ejemplo de solicitud.
con curl.
```bash
    curl --location 'API_URL/api/registrar-victima-con-curp' \
    --header 'Accept: application/json' \
    --header 'Content-Type: application/json' \
    --header 'Authorization: Bearer TOKEN' \
    --data '{
    "curp"              : "CURP000000XXXXXXX0",
    "cve_ent"           : 22,
    "nacionalidad_id"   : 1,
    "extranjera"        : false,
    "identifica_mujer"  : true
    }'
```
con PHP - cURL.
```php
    <?php

    $curl = curl_init();

    curl_setopt_array($curl, array(
    CURLOPT_URL => 'API_URL/api/registrar-victima-con-curp',
    CURLOPT_RETURNTRANSFER => true,
    CURLOPT_ENCODING => '',
    CURLOPT_MAXREDIRS => 10,
    CURLOPT_TIMEOUT => 0,
    CURLOPT_FOLLOWLOCATION => true,
    CURLOPT_HTTP_VERSION => CURL_HTTP_VERSION_1_1,
    CURLOPT_CUSTOMREQUEST => 'POST',
    CURLOPT_POSTFIELDS =>'{
    "curp"              : "CURP000000XXXXXXX0",
    "cve_ent"           : 22,
    "nacionalidad_id"   : 1,
    "extranjera"        : false,
    "identifica_mujer"  : true
    }',
    CURLOPT_HTTPHEADER => array(
        'Accept: application/json',
        'Content-Type: application/json',
        'Authorization: Bearer TOKEN'
    ),
    ));

    $response = curl_exec($curl);

    curl_close($curl);
    echo $response;
```
con JavaScrip - fetch.
```javascript
    const myHeaders = new Headers();
    myHeaders.append("Accept", "application/json");
    myHeaders.append("Content-Type", "application/json");
    myHeaders.append("Authorization", "Bearer TOKEN");

    const raw = JSON.stringify({
    "curp": "CURP000000XXXXXXX0",
    "cve_ent": 22,
    "nacionalidad_id": 1,
    "extranjera": false,
    "identifica_mujer"  : true
    });

    const requestOptions = {
    method: "POST",
    headers: myHeaders,
    body: raw,
    redirect: "follow"
    };

    fetch("API_URL/api/registrar-victima-con-curp", requestOptions)
    .then((response) => response.text())
    .then((result) => console.log(result))
    .catch((error) => console.error(error));
```
#### La respuesta que se recibirá después de la petición.
con estatus 200  en formato json.
```json
    {
        "curp": "CURP000000XXXXXXX0",
        "nombre": "nombre",
        "primer_apellido": "primer_apellido",
        "segundo_apellido": "segundo_apellido",
        "fecha_nacimiento": "1999-08-11",
        "sexo_id": 2,
        "cve_ent": 9,
        "nacionalidad_id": 1,
        "extranjera": false,
        "users_id": 1,
        "instancias_id": null,
        "areas_id": null,
        "sedes_id": null,
        "updated_at": "2024-07-26T01:52:57.000000Z",
        "created_at": "2024-07-26T01:52:57.000000Z",
        "id": 5237,
        "folio_euv": "EUV000000000005238072024"
    }
```
con estatus 422. Error con número de caracteres el curp.
```json
{
    "message": "El campo curp debe tener 18 caracteres.",
    "errors": {
        "curp": [
            "El campo curp debe tener 18 caracteres."
        ]
    }
}
```
con estatus 422. No hay registro del curp en la renapo.
```json
{
    "message": "La CURP no se encuentra en la base de datos",
    "errors": {
        "curp": [
            "La CURP no se encuentra en la base de datos"
        ]
    }
}
```
--------------------------------------------------------------
### Registro de víctima con Datos
--------------------------------------------------------------
El consumo de este método requiere el curp y datos, es necesario enviarlos como parámetros.
#### POST
```
    API_URL/api/registrar-victima-con-datos
```
catálogos: [cve_ent](https://drive.google.com/file/d/1Y163QX4ddN4J6w8ZGNUg_11-l4EM9V5w/view?usp=sharing), [nacionalidad_id](https://drive.google.com/file/d/1Q0gUDPgv9_3xfmPYPJ6WLfrUrFdqvEGB/view)
#### Body raw (json)
```json
    {
        "nombre"            : "nombre",
        "primer_apellido"   : "primer_apellido",
        "segundo_apellido"  : "segundo_apellido",
        "fecha_nacimiento"  : "1965-11-20",
        "sexo"              : "H",
        "cve_ent"           : 24,
        "extranjera"        : false,
        "curp"              : "CURP000000XXXXXXX0",
        "nacionalidad_id"   : 1,
        "identifica_mujer"  : true
    }
```
#### Campos Obligatorios

|       Campo      |  Obligatorio  |  Tipo de dato |
|:----------------:|:-------------:|:-------------:|
| nombre           |      SI       |     string    | 
| primer_apellido  |      SI       |     string    |
| segundo_apellido |      SI       |     string    |
| fecha_nacimiento |      SI       |      date     |  
| sexo             |      SI       |     string    |
| cve_ent          |      SI       |     integer   |
| extranjera       |      NO       |     boolean   |
| curp             |      SI       |     string    |
| nacionalidad_id  |      NO       |     integer   |
| identifica_mujer |      NO       |     boolean   |

#### Ejemplo de solicitud.
con curl.
```bash
    curl --location 'API_URL/api/registrar-victima-con-datos' \
    --header 'Accept: application/json' \
    --header 'Content-Type: application/json' \
    --header 'Authorization: Bearer TOKEN' \
    --data '{
    "nombre"            : "nombre",
    "primer_apellido"   : "primer_apellido",
    "segundo_apellido"  : "segundo_apellido",
    "fecha_nacimiento"  : "1965-11-20",
    "sexo"              : "H",
    "cve_ent"           : 24,
    "extranjera"        : false,
    "curp"              : "CURP000000XXXXXXX0",
    "nacionalidad_id"   : 1,
    "identifica_mujer"  : true
    }'
```
con PHP - cURL.
```php
    <?php

    $curl = curl_init();

    curl_setopt_array($curl, array(
    CURLOPT_URL => 'API_URL/api/registrar-victima-con-datos',
    CURLOPT_RETURNTRANSFER => true,
    CURLOPT_ENCODING => '',
    CURLOPT_MAXREDIRS => 10,
    CURLOPT_TIMEOUT => 0,
    CURLOPT_FOLLOWLOCATION => true,
    CURLOPT_HTTP_VERSION => CURL_HTTP_VERSION_1_1,
    CURLOPT_CUSTOMREQUEST => 'POST',
    CURLOPT_POSTFIELDS =>'{
    "nombre"            : "nombre",
    "primer_apellido"   : "primer_apellido",
    "segundo_apellido"  : "segundo_apellido",
    "fecha_nacimiento"  : "1965-11-20",
    "sexo"              : "H",
    "cve_ent"           : 24,
    "extranjera"        : false,
    "curp"              : "CURP000000XXXXXXX0",
    "nacionalidad_id"   : 1,
    "identifica_mujer"  : true  
    }',
    CURLOPT_HTTPHEADER => array(
        'Accept: application/json',
        'Content-Type: application/json',
        'Authorization: Bearer TOKEN'
    ),
    ));

    $response = curl_exec($curl);

    curl_close($curl);
    echo $response;
```
con JavaScrip - fetch.
```javascript
    const myHeaders = new Headers();
    myHeaders.append("Accept", "application/json");
    myHeaders.append("Content-Type", "application/json");
    myHeaders.append("Authorization", "Bearer TOKEN");

    const raw = JSON.stringify({
    "nombre": "nombre",
    "primer_apellido": "primer_apellido",
    "segundo_apellido": "segundo_apellido",
    "fecha_nacimiento": "1965-11-20",
    "sexo": "H",
    "cve_ent": 24,
    "extranjera": false,
    "curp": "CURP000000XXXXXXX0",
    "nacionalidad_id"   : 1,
    "identifica_mujer"  : true
    });

    const requestOptions = {
    method: "POST",
    headers: myHeaders,
    body: raw,
    redirect: "follow"
    };

    fetch("API_URL/api/registrar-victima-con-datos", requestOptions)
    .then((response) => response.text())
    .then((result) => console.log(result))
    .catch((error) => console.error(error));
```
#### La respuesta que se recibirá después de la petición.
con estatus 200  en formato json.
```json
{
    "curp": "CURP000000XXXXXXX0",
    "nombre": "nombre",
    "primer_apellido": "primer_apellido",
    "segundo_apellido": "segundo_apellido",
    "fecha_nacimiento": "1999-08-11",
    "sexo_id": 2,
    "cve_ent": 9,
    "nacionalidad_id": 1,
    "extranjera": false,
    "users_id": 1,
    "instancias_id": null,
    "areas_id": null,
    "sedes_id": null,
    "updated_at": "2024-07-26T01:52:57.000000Z",
    "created_at": "2024-07-26T01:52:57.000000Z",
    "id": 5237,
    "folio_euv": "EUV000000000005238072024"
}
```
con estatus 422. La CURP ya existe en el sistema.
```json
{
    "message": "La CURP ya existe en el sistema, folio_euv: EUV000000000005241072024",
    "errors": {
        "curp": [
            "La CURP ya existe en el sistema, folio_euv: EUV000000000005241072024"
        ]
    }
}
```
con estatus 422. Faltan parámetros.
```json
{
    "message": "El campo nombre es requerido. (and 5 more errors)",
    "errors": {
        "nombre": [
            "El campo nombre es requerido."
        ],
        "primer_apellido": [
            "El campo primer apellido es requerido."
        ],
        "segundo_apellido": [
            "El campo segundo apellido es requerido."
        ],
        "fecha_nacimiento": [
            "El campo fecha nacimiento es requerido."
        ],
        "sexo": [
            "El campo sexo es requerido."
        ],
        "cve_ent": [
            "El campo cve ent es requerido."
        ]
    }
}
```
con estatus 500. No se envió el curp.
```json
 "message": "buscar_curp(): Argument #1 ($curp) must be of type string, null given, called in /var/www/html/app/Http/Controllers/ApiVictimaMethodsController.php on line 129",
```
--------------------------------------------------------------
### Registro de víctima sin CURP
--------------------------------------------------------------
El consumo de este método no requiere el curp de la víctima, sin embargo es necesario envíar los datos como parámetros.
#### POST
```
    API_URL/api/registrar-victima-sin-curp
```
catálogos: [cve_ent](https://drive.google.com/file/d/1Y163QX4ddN4J6w8ZGNUg_11-l4EM9V5w/view?usp=sharing), [nacionalidad_id](https://drive.google.com/file/d/1Q0gUDPgv9_3xfmPYPJ6WLfrUrFdqvEGB/view)
#### Body raw (json)
```json
{
    "nombre"            : "nombre",
    "primer_apellido"   : "primer_apellido",
    "segundo_apellido"  : "segundo_apellido",
    "fecha_nacimiento"  : "1965-11-20",
    "sexo"              : "H",
    "cve_ent"           : 24,
    "extranjera"        : false,
    "nacionalidad_id"   : 1,
    "identifica_mujer"  : true
}
```
#### Campos Obligatorios
 
|       Campo       |  Obligatorio  |  Tipo de dato  |
|:-----------------:|:-------------:|:--------------:|
| nombre            |      SI       |     string     |  
| primer_apellido   |      SI       |     string     | 
| segundo_apellido  |      SI       |     string     |
| fecha_nacimiento  |      SI       |      date      |
| sexo              |      SI       |     string     |
| cve_ent           |      SI       |     integer    |
| extranjera        |      NO       |     boolean    |
| nacionalidad_id   |      NO       |     integer    |
| identifica_mujer  |      NO       |     boolean    |
|||

#### Ejemplo de solicitud.
con curl.
```bash
    curl --location 'API_URL/api/registrar-victima-sin-curp' \
    --header 'Accept: application/json' \
    --header 'Content-Type: application/json' \
    --header 'Authorization: Bearer TOKEN' \
    --data '{
    "nombre"            : "nombre",
    "primer_apellido"   : "primer_apellido",
    "segundo_apellido"  : "segundo_apellido",
    "fecha_nacimiento"  : "1965-11-20",
    "sexo"              : "H",
    "cve_ent"           : 24,
    "extranjera"        : false,
    "nacionalidad_id"   : 1,
    "identifica_mujer"  : true
    }'
```
con PHP - cURL.
```php
    <?php

    $curl = curl_init();

    curl_setopt_array($curl, array(
    CURLOPT_URL => 'API_URL/api/registrar-victima-sin-curp',
    CURLOPT_RETURNTRANSFER => true,
    CURLOPT_ENCODING => '',
    CURLOPT_MAXREDIRS => 10,
    CURLOPT_TIMEOUT => 0,
    CURLOPT_FOLLOWLOCATION => true,
    CURLOPT_HTTP_VERSION => CURL_HTTP_VERSION_1_1,
    CURLOPT_CUSTOMREQUEST => 'POST',
    CURLOPT_POSTFIELDS =>'{
    "nombre"            : "nombre",
    "primer_apellido"   : "primer_apellido",
    "segundo_apellido"  : "segundo_apellido",
    "fecha_nacimiento"  : "1965-11-20",
    "sexo"              : "H",
    "cve_ent"           : 24,
    "extranjera"        : false,
    "nacionalidad_id"   : 1,
    "identifica_mujer"  : true
    }',
    CURLOPT_HTTPHEADER => array(
        'Accept: application/json',
        'Content-Type: application/json',
        'Authorization: Bearer TOKEN'
    ),
    ));

    $response = curl_exec($curl);

    curl_close($curl);
    echo $response;
```
con JavaScrip - fetch.
```javascript
    const myHeaders = new Headers();
    myHeaders.append("Accept", "application/json");
    myHeaders.append("Content-Type", "application/json");
    myHeaders.append("Authorization", "Bearer TOKEN");

    const raw = JSON.stringify({
    "nombre": "nombre",
    "primer_apellido": "primer_apellido",
    "segundo_apellido": "segundo_apellido",
    "fecha_nacimiento": "1965-11-20",
    "sexo": "H",
    "cve_ent": 24,
    "extranjera": false,
    "nacionalidad_id"   : 1,
    "identifica_mujer"  : true
    });

    const requestOptions = {
    method: "POST",
    headers: myHeaders,
    body: raw,
    redirect: "follow"
    };

    fetch("API_URL/api/registrar-victima-sin-curp", requestOptions)
    .then((response) => response.text())
    .then((result) => console.log(result))
    .catch((error) => console.error(error));
```
#### La respuesta que se recibirá después de la petición.
con estatus 200  en formato json.
```json
{
    "curp": "PISN651120XSPRGMXX",
    "nombre": "nombre",
    "primer_apellido": "primer_apellido",
    "segundo_apellido": "segundo_apellido",
    "fecha_nacimiento": "1965-11-20",
    "sexo_id": 1,
    "cve_ent": 24,
    "nacionalidad_id": null,
    "extranjera": false,
    "users_id": 1,
    "instancias_id": null,
    "areas_id": null,
    "sedes_id": null,
    "updated_at": "2024-07-26T04:46:12.000000Z",
    "created_at": "2024-07-26T04:46:12.000000Z",
    "id": 5242,
    "folio_euv": "EUV000000000005243072024"
}
```
con estatus 422. La CURP ya existe en el sistema.
```json
{
    "message": "La CURP ya existe en el sistema, folio_euv: EUV000000000005243072024",
    "errors": {
        "curp": [
            "La CURP ya existe en el sistema, folio_euv: EUV000000000005243072024"
        ]
    }
}
```
con estatus 422. Faltan parámetros.
```json
{
    "message": "El campo nombre es requerido. (and 5 more errors)",
    "errors": {
        "nombre": [
            "El campo nombre es requerido."
        ],
        "primer_apellido": [
            "El campo primer apellido es requerido."
        ],
        "segundo_apellido": [
            "El campo segundo apellido es requerido."
        ],
        "fecha_nacimiento": [
            "El campo fecha nacimiento es requerido."
        ],
        "sexo": [
            "El campo sexo es requerido."
        ],
        "cve_ent": [
            "El campo cve ent es requerido."
        ]
    }
}
```
--------------------------------------------------------------
### Registro de un hecho de violencia.
--------------------------------------------------------------
El hecho de violencia tiene una dependencia funcional, que requiere que la víctima esté registrada.
#### POST
```
    API_URL/api/hecho-de-violencia
```
catálogos: [cve_ent](https://drive.google.com/file/d/1Y163QX4ddN4J6w8ZGNUg_11-l4EM9V5w/view?usp=sharing), [lugar_id](https://drive.google.com/file/d/1_rs52tWT5M-6U0AVn3texkHHpeSwtmRZ/view), [lugar_detalle_id](https://drive.google.com/file/d/1HvwJJpAHiQ7h5XnzVLAMT6tDnUCZZnZk/view), [pais_id](https://drive.google.com/file/d/1-5gPflCkSALusWLp_pUN20NJbqEWE6ir/view), [cve_mun](https://drive.google.com/file/d/19q9v31lH0Dgq7bsCBpO3hGfr_mT5VfNk/view), [cve_loc](https://drive.google.com/file/d/1VDHjmDqURqkLc5MQb84MKhI9NlVrg3m3/view), [tipo_violencia](https://drive.google.com/file/d/1Vf4-VABIMH5LIK308ubKLfjfFKwfnhOk/view), [modalidad_violencia](https://drive.google.com/file/d/1_Oq77ueBKXeV6e9848S7Y4i-Xa2WGUG-/view), [efectos_fisicos](https://drive.google.com/file/d/1TFijVmvWgGEAzjrAE1WVW-WQveIg-UXy/view), [consecuencias_sexuales](https://drive.google.com/file/d/1qG8ny2idjFkaGjYP_piFKFb30Dgg4jhR/view), [efectos_psicologicos](https://drive.google.com/file/d/1F-R-n6QdIxWPczrw_ZFcSvzQzu_FPXY0/view), [efectos_economicos_y_patrimoniales](https://drive.google.com/file/d/1o9U4ySp0xzFkItncnuLDeqliUOhHIB1L/view), [agente_de_lesion](https://drive.google.com/file/d/1RjXu3Ea0osZeesVqZPWAQXP3iVVIY2Kh/view) y [area_anatomica_lesionada](https://drive.google.com/file/d/1R0fblWPIU9-bc74GapgzEnBwFjP4T11p/view).
#### Body raw (json)
```json
{
    "folio_euv"                                     : "EUV000000000005248072024",
    "fecha_hechos"                                  : "2022-10-26",
    "hora_hechos"                                   : "08:12",
    "descripcion_hechos"                            : "Este es un hecho",
    "lugar_id"                                      : 3,
    "lugar_detalle_id"                              : 36,
    "en_domicilio_victima"                          : true,
    "pais_id"                                       : 238,
    "cve_ent"                                       : 21,
    "cve_mun"                                       : 133,
    "calle"                                         : "siempre viva 23",
    "num_exterior_km"                               : "45",
    "cve_loc"                                       : 1,
    "cp"                                            : "74069",
    "lat"                                           : 19.2843100,
    "lng"                                           : -98.4388500,
    "es_festivo"                                    : false,
    "conoce_la_autoridad"                           : false,
    "tipo_violencia"                                : [1,3,5,8],
    "modalidad_violencia"                           : 6,
    "es_victima_de_delincuencia_organizada"         : false,
    "hay_denuncia"                                  : false,
    "efectos_fisicos"                               : [1,2],
    "consecuencias_sexuales"                        : [],
    "efectos_psicologicos"                          : [],
    "efectos_economicos_y_patrimoniales"            : [],
    "agente_de_lesion"                              : [],
    "area_anatomica_lesionada"                      : [],
    "es_relacionada_con_orientacion_o_identidad"    : false,
    "lugar_detalle_otro"                            : "lugar_detalle_otro",
    "tipo_violencia_otro"                           : "tipo_violencia_otro",
    "efecto_fisico_otro"                            : "efecto_fisico_otro",
    "consecuencia_sexual_otro"                      : "consecuencia_sexual_otro" ,          
    "efecto_psicologico_otro"                       : "efecto_psicologico_otro",           
    "efecto_economico_patrimonial_otro"             : "efecto_economico_patrimonial_otro",           
    "agente_lesion_otro"                            : "agente_lesion_otro",
    "area_anatomica_lesionada_otro"                 : "area_anatomica_lesionada_otro" 
}
```
#### Campos Obligatorios

|                     Campo                     | Obligatorio | Tipo de dato |
|:---------------------------------------------:|:-----------:|:------------:|
| folio_euv                                     |     SI      |    string    |
| fecha_hechos                                  |     SI      |     date     |
| hora_hechos                                   |     SI      |     time     | 
| descripcion_hechos                            |     SI      |    string    |
| lugar_id                                      |     NO      |    integer   |
| lugar_detalle_id                              |     NO      |    integer   |
| en_domicilio_victima                          |     NO      |    boolean   |
| pais_id                                       |     NO      |    integer   |
| cve_ent                                       |     SI      |    integer   |
| cve_mun                                       |     SI      |    integer   |
| calle                                         |     NO      |    string    |   
| num_exterior_km                               |     NO      |    string    |
| cve_loc                                       |     NO      |    integer   |
| cp                                            |     SI      |    string    |
| lat                                           |     NO      |    double    |
| lng                                           |     NO      |    double    |
| es_festivo                                    |     NO      |    bolean    |
| conoce_la_autoridad                           |     NO      |    bolean    |
| tipo_violencia                                |     NO      |   integer[]  |
| modalidad_violencia                           |     NO      |    integer   |
| es_victima_de_delincuencia_organizada         |     NO      |    bolean    |
| hay_denuncia                                  |     NO      |    bolean    |
| efectos_fisicos                               |     NO      |   integer[]  |
| consecuencias_sexuales                        |     NO      |   integer[]  |
| efectos_psicologicos                          |     NO      |   integer[]  |
| efectos_economicos_y_patrimoniales            |     NO      |   integer[]  |
| agente_de_lesion                              |     NO      |   integer[]  |
| area_anatomica_lesionada                      |     NO      |   integer[]  |
| es_relacionada_con_orientacion_o_identidad    |     NO      |    bolean    |
| lugar_detalle_otro                            |     NO      |    string    |  
| tipo_violencia_otro                           |     NO      |    string    | 
| efecto_fisico_otro                            |     NO      |    string    | 
| consecuencia_sexual_otro                      |     NO      |    string    | 
| efecto_psicologico_otro                       |     NO      |    string    | 
| efecto_economico_patrimonial_otro             |     NO      |    string    | 
| agente_lesion_otro                            |     NO      |    string    | 
| area_anatomica_lesionada_otro                 |     NO      |    string    | 
|||

#### Ejemplo de solicitud.
con curl.
```bash
    curl --location 'API_URL/api/hecho-de-violencia' \
    --header 'Accept: application/json' \
    --header 'Content-Type: application/json' \
    --header 'Authorization: Bearer TOKEN' \
    --data '{
    "folio_euv"                                     : "EUV000000000005248072024",
    "fecha_hechos"                                  : "2022-10-26",
    "hora_hechos"                                   : "08:12",
    "descripcion_hechos"                            : "Este es un hecho",
    "lugar_id"                                      : 3,
    "lugar_detalle_id"                              : 36,
    "en_domicilio_victima"                          : true,
    "pais_id"                                       : 238,
    "cve_ent"                                       : 21,
    "cve_mun"                                       : 133,
    "calle"                                         : "siempre viva 23",
    "num_exterior_km"                               : "45",
    "cve_loc"                                       : 1,
    "cp"                                            : "74069",
    "lat"                                           : 19.2843100,
    "lng"                                           : -98.4388500,
    "es_festivo"                                    : false,
    "conoce_la_autoridad"                           : false,
    "tipo_violencia"                                : [1,3,5,8],
    "modalidad_violencia"                           : 6,
    "es_victima_de_delincuencia_organizada"         : false,
    "hay_denuncia"                                  : false,
    "efectos_fisicos"                               : [1,2],
    "consecuencias_sexuales"                        : [],
    "efectos_psicologicos"                          : [],
    "efectos_economicos_y_patrimoniales"            : [],
    "agente_de_lesion"                              : [],
    "area_anatomica_lesionada"                      : [],
    "es_relacionada_con_orientacion_o_identidad"    : false,
    "lugar_detalle_otro"                            : "lugar_detalle_otro",
    "tipo_violencia_otro"                           : "tipo_violencia_otro",
    "efecto_fisico_otro"                            : "efecto_fisico_otro",
    "consecuencia_sexual_otro"                      : "consecuencia_sexual_otro" ,          
    "efecto_psicologico_otro"                       : "efecto_psicologico_otro",           
    "efecto_economico_patrimonial_otro"             : "efecto_economico_patrimonial_otro",           
    "agente_lesion_otro"                            : "agente_lesion_otro",
    "area_anatomica_lesionada_otro"                 : "area_anatomica_lesionada_otro" 
    }'
```
con PHP - cURL.
```php
    <?php

    $curl = curl_init();

    curl_setopt_array($curl, array(
    CURLOPT_URL => 'API_URL/api/hecho-de-violencia',
    CURLOPT_RETURNTRANSFER => true,
    CURLOPT_ENCODING => '',
    CURLOPT_MAXREDIRS => 10,
    CURLOPT_TIMEOUT => 0,
    CURLOPT_FOLLOWLOCATION => true,
    CURLOPT_HTTP_VERSION => CURL_HTTP_VERSION_1_1,
    CURLOPT_CUSTOMREQUEST => 'POST',
    CURLOPT_POSTFIELDS =>'{
    "folio_euv"                                     : "EUV000000000005248072024",
    "fecha_hechos"                                  : "2022-10-26",
    "hora_hechos"                                   : "08:12",
    "descripcion_hechos"                            : "Este es un hecho",
    "lugar_id"                                      : 3,
    "lugar_detalle_id"                              : 36,
    "en_domicilio_victima"                          : true,
    "pais_id"                                       : 238,
    "cve_ent"                                       : 21,
    "cve_mun"                                       : 133,
    "calle"                                         : "siempre viva 23",
    "num_exterior_km"                               : "45",
    "cve_loc"                                       : 1,
    "cp"                                            : "74069",
    "lat"                                           : 19.2843100,
    "lng"                                           : -98.4388500,
    "es_festivo"                                    : false,
    "conoce_la_autoridad"                           : false,
    "tipo_violencia"                                : [1,3,5,8],
    "modalidad_violencia"                           : 6,
    "es_victima_de_delincuencia_organizada"         : false,
    "hay_denuncia"                                  : false,
    "efectos_fisicos"                               : [1,2],
    "consecuencias_sexuales"                        : [],
    "efectos_psicologicos"                          : [],
    "efectos_economicos_y_patrimoniales"            : [],
    "agente_de_lesion"                              : [],
    "area_anatomica_lesionada"                      : [],
    "es_relacionada_con_orientacion_o_identidad"    : false,
    "lugar_detalle_otro"                            : "lugar_detalle_otro",
    "tipo_violencia_otro"                           : "tipo_violencia_otro",
    "efecto_fisico_otro"                            : "efecto_fisico_otro",
    "consecuencia_sexual_otro"                      : "consecuencia_sexual_otro" ,          
    "efecto_psicologico_otro"                       : "efecto_psicologico_otro",           
    "efecto_economico_patrimonial_otro"             : "efecto_economico_patrimonial_otro",           
    "agente_lesion_otro"                            : "agente_lesion_otro",
    "area_anatomica_lesionada_otro"                 : "area_anatomica_lesionada_otro" 
    }',
    CURLOPT_HTTPHEADER => array(
        'Accept: application/json',
        'Content-Type: application/json',
        'Authorization: Bearer TOKEN'
    ),
    ));

    $response = curl_exec($curl);

    curl_close($curl);
    echo $response;
```
con JavaScrip - fetch.
```javascript
    const myHeaders = new Headers();
    myHeaders.append("Accept", "application/json");
    myHeaders.append("Content-Type", "application/json");
    myHeaders.append("Authorization", "Bearer TOKEN");

    const raw = JSON.stringify({
    "folio_euv": "EUV000000000005248072024",
    "fecha_hechos": "2022-10-26",
    "hora_hechos": "08:12",
    "descripcion_hechos": "Este es un hecho",
    "lugar_id": 3,
    "lugar_detalle_id": 36,
    "en_domicilio_victima": true,
    "pais_id": 238,
    "cve_ent": 21,
    "cve_mun": 133,
    "calle": "siempre viva 23",
    "num_exterior_km": "45",
    "cve_loc": 1,
    "cp": "74069",
    "lat": 19.28431,
    "lng": -98.43885,
    "es_festivo": false,
    "conoce_la_autoridad": false,
    "tipo_violencia": [1,3,5,8],
    "modalidad_violencia": 6,
    "es_victima_de_delincuencia_organizada": false,
    "hay_denuncia": false,
    "efectos_fisicos": [1,2],
    "consecuencias_sexuales": [],
    "efectos_psicologicos": [],
    "efectos_economicos_y_patrimoniales": [],
    "agente_de_lesion": [],
    "area_anatomica_lesionada": [],
    "es_relacionada_con_orientacion_o_identidad": false,
    "lugar_detalle_otro"                        : "lugar_detalle_otro",
    "tipo_violencia_otro"                       : "tipo_violencia_otro",
    "efecto_fisico_otro"                        : "efecto_fisico_otro",
    "consecuencia_sexual_otro"                  : "consecuencia_sexual_otro" ,          
    "efecto_psicologico_otro"                   : "efecto_psicologico_otro",           
    "efecto_economico_patrimonial_otro"         : "efecto_economico_patrimonial_otro",           
    "agente_lesion_otro"                        : "agente_lesion_otro",
    "area_anatomica_lesionada_otro"             : "area_anatomica_lesionada_otro" 
    });

    const requestOptions = {
    method: "POST",
    headers: myHeaders,
    body: raw,
    redirect: "follow"
    };

    fetch("API_URL/api/hecho-de-violencia", requestOptions)
    .then((response) => response.text())
    .then((result) => console.log(result))
    .catch((error) => console.error(error));
```
#### La respuesta que se recibirá después de la petición.
con estatus 200  en formato json.
```json
{
    "hecho": {
        "users_id": 1,
        "victimas_id": 5247,
        "folio": "EUV000000000005248072024-1",
        "fecha_hechos": "2022-10-26",
        "hora_hechos": "08:12",
        "descripcion_hechos": "Este es un hecho",
        "lugar_id": 3,
        "lugar_detalle_id": 36,
        "en_domicilio_victima": true,
        "pais_id": 238,
        "cve_ent": 21,
        "cve_mun": 133,
        "calle": "siempre viva 23",
        "num_exterior_km": "45",
        "num_interior": null,
        "cve_loc": 1,
        "cp": "74069",
        "lat": 19.28431,
        "lng": -98.43885,
        "es_festivo": false,
        "conoce_la_autoridad": false,
        "conoce_la_autoridad_detalle": "",
        "tipo_violencia": "[1,3,5,8]",
        "modalidad_violencia": 6,
        "es_victima_de_delincuencia_organizada": false,
        "hay_denuncia": false,
        "efectos_fisicos": "[1,2]",
        "consecuencias_sexuales": "[]",
        "efectos_psicologicos": "[]",
        "efectos_economicos_y_patrimoniales": "[]",
        "agente_de_lesion": "[]",
        "area_anatomica_lesionada": "[]",
        "es_relacionada_con_orientacion_o_identidad": false,
        "updated_at": "2024-07-26T19:11:04.000000Z",
        "created_at": "2024-07-26T19:11:04.000000Z",
        "id": 5300
    }
}
```
con estatus 422. El folio_euv no existe en el sistema.
```json
{
    "message": "El folio EUV no está registrado en el sistema",
    "errors": {
        "folio_euv": [
            "El folio EUV no está registrado en el sistema"
        ]
    }
}
```
con estatus 422. Faltan parámetros.
```json
{
    "message": "El campo fecha hechos es requerido. (and 3 more errors)",
    "errors": {
        "fecha_hechos": [
            "El campo fecha hechos es requerido."
        ],
        "hora_hechos": [
            "El campo hora hechos es requerido."
        ],
        "descripcion_hechos": [
            "El campo descripcion hechos es requerido."
        ],
        "cve_ent": [
            "El campo cve ent es requerido."
        ]
    }
}
```
--------------------------------------------------------------
### Registro o edición de los datos de una víctima
--------------------------------------------------------------
Este endpoint puede ser utilizado tanto para crear un nuevo registro como para editar uno existente y tiene una dependencia funcional que requiere que la víctima tenga un hecho de víolencia registrado.

1.- **Para un nuevo registro**: Si el identificador hechos_id de la solicitud, no existe registrado en los datos de la víctima del sistema, se interpreta que debe crear un nuevo registro con los datos proporcionados.

2.- **Para editar un registro existente**: Si el identificador hechos_id de la solicitud, tiene registro en los datos de la víctima del sistema, se interpreta que debe editar el registro con los datos proporcionados.

Esto permite aprovechar el mismo endpoint para múltiples acciones, facilitando tanto la creación como la modificación de los registros.


#### POST
```json
    API_URL/api/registrar-datos-victima-por-hecho
```
catálogos: [cve_ent](https://drive.google.com/file/d/1Y163QX4ddN4J6w8ZGNUg_11-l4EM9V5w/view?usp=sharing), [cve_mun](https://drive.google.com/file/d/19q9v31lH0Dgq7bsCBpO3hGfr_mT5VfNk/view), [cve_loc](https://drive.google.com/file/d/1VDHjmDqURqkLc5MQb84MKhI9NlVrg3m3/view), [colonias_id](https://drive.google.com/file/d/1wWVhPLa0zFgdoeQYEtCRNfODEcpQx6BH/view), [estado_conyugal](https://drive.google.com/file/d/1ealfYFyyPpz_C2VfJ3tTrpXzGMVAzfwi/view?usp=sharing), [pais_id](https://drive.google.com/file/d/1-5gPflCkSALusWLp_pUN20NJbqEWE6ir/view), [escolaridad_id](https://drive.google.com/file/d/1Pf_eJpt_S34Ipo908Ih7RS-kQHfwJQGr/view), [ingreso_economico_id](https://docs.google.com/spreadsheets/d/15yA5gPDJXZFZkW1fiFKhrCXeQT1uVGJB/edit?gid=717444154#gid=717444154), [ocupacion_id](https://drive.google.com/file/d/1tY37QRvcZa0c-vzlSsgEpIRJnN2BqCw_/view), [discapacidad](https://drive.google.com/file/d/1mNHUBUsaUKiOPfnoRkGyJjG9fWvcKZFU/view?usp=sharing), [pueblo_indigena_id](https://drive.google.com/file/d/1HUX7WHG1yI-QA-Jo7DG483HCf2kJBBDA/view?usp=drive_link), [identidad_genero_id](https://drive.google.com/file/d/1K7jBCF4E6aiBfnrg4ioD5cqh6gbFOXcL/view), [orientacion_sexual_id](https://drive.google.com/file/d/1hc5Yhl2gr6_pWBFED7G0QMGyooVzIS5x/view), [cual_adiccion](https://drive.google.com/file/d/165EOC-eMmor3JFDXKJT4-Scb8zgjZEAF/view?usp=sharing), [gestacion_id](https://drive.google.com/file/d/1pJCpimw2w7NzZwj0_0zRW3dalcDBgzdu/view?usp=sharing), [servicio_medico_id](https://drive.google.com/file/d/148DpyySvBaHaBcimb0VhGkyusW3lqrc9/view?usp=sharing), [tipos_adicciones](https://drive.google.com/file/d/1Rj1oaktbwIiDQ04Jq6-n8bSXv09FCTaa/view?usp=sharing).

#### Body raw (json)
```
{
    "hechos_id"                    : 2,         
    "edad"                         : 33,                      
    "telefono"                     : "26460978",                  
    "correo_electronico"           : "prueba@fake.com",
    "calle"                        : "calle",          
    "num_exterior"                 : "e1",        
    "num_interior"                 : "i1",         
    "cve_ent"                      : 21,                       
    "cve_mun"                      : 132,                      
    "cve_loc"                      : 1,                    
    "colonias_id"                  : 1,        
    "entre_calle_uno"              : "uno",
    "entre_calle_dos"              : "dos",
    "referencia"                   : "referencia", 
    "estado_conyugal"              : 1, 
    "pais"                         : 153,
    "codigo_postal"                : 6400,
    "escolaridad_id"               : 1, 
    "ingreso_economico_id"         : 1, 
    "ocupacion_id"                 : 1,
    "presenta_discapacidad"        : true, 
    "discapacidad"                 : [1],           
    "otra_discapacidad"            : "sin definir", 
    "enfermedad"                   : true, 
    "cual_enfermedad"              : "descripción de la enfermedad",
    "enfermedad_psiquiatrica"      : true,
    "cual_enfermedad_psiquiatrica" : "psiquiatrica",
    "tratamiento"                  : true,
    "cual_tratamiento"             : "tratamiento",
    "situacion_calle"              : true, 
    "migrante"                     : true,
    "pueblo_indigena"              : true,
    "pueblo_indigena_id"           : 1, 
    "lgbtti"                       : true, 
    "identidad_genero_id"          : 1, 
    "orientacion_sexual_id"        : 1, 
    "embarazada"                   : true, 
    "numero_semanas_emb"           : 20, 
    "dependientes"                 : true, 
    "red_apoyo"                    : true,
    "adiccion"                     : true, 
    "cual_adiccion"                : [1], 
    "persona_afrodescendiente"     : "afro",  
    "caso_name"                    : "no name", 
    "realiza_mas_actividades"      : true, 
    "discapacidad_por_violencia"   : true,
    "gestacion_id"                 : 1, 
    "clave_centro_escolar"         : "Clave centro", 
    "numero_seguro_social"         : "AV0569806410",
    "servicio_medico_id"           : 1,
    "otro_servicio_medico"         : "otro servicio medico", 
    "tipos_adicciones"             : [1] 
}
```
#### Campos Obligatorios

|   Campo                      | Obligatorio |Tipo de dato |
|:----------------------------:|:-----------:|:-----------:|
| hechos_id                    |     SI      |   integer   | 
| edad                         |     NO      |   integer   | 
| telefono                     |     NO      |   string    | 
| correo_electronico           |     NO      |   string    | 
| calle                        |     NO      |   string    | 
| num_exterior                 |     NO      |   string    | 
| num_interior                 |     NO      |   string    | 
| cve_ent                      |     NO      |   integer   | 
| cve_mun                      |     NO      |   integer   | 
| cve_loc                      |     NO      |   integer   | 
| colonias_id                  |     NO      |   integer   | 
| entre_calle_uno              |     NO      |   string    | 
| entre_calle_dos              |     NO      |   string    | 
| referencia                   |     NO      |   string    | 
| estado_conyugal              |     NO      |   integer   | 
| pais                         |     NO      |   integer   | 
| codigo_postal                |     NO      |   integer   | 
| escolaridad_id               |     NO      |   integer   | 
| ingreso_economico_id         |     NO      |   integer   | 
| ocupacion_id                 |     NO      |   integer   | 
| presenta_discapacidad        |     NO      |   boolean   | 
| discapacidad                 |     NO      |   integer[] | 
| otra_discapacidad            |     NO      |   string    | 
| enfermedad                   |     NO      |   boolean   | 
| cual_enfermedad              |     NO      |   string    | 
| enfermedad_psiquiatrica      |     NO      |   boolean   | 
| cual_enfermedad_psiquiatrica |     NO      |   string    | 
| tratamiento                  |     NO      |   boolean   | 
| cual_tratamiento             |     NO      |   string    | 
| situacion_calle              |     NO      |   boolean   | 
| migrante                     |     NO      |   boolean   | 
| pueblo_indigena              |     NO      |   boolean   | 
| pueblo_indigena_id           |     NO      |   integer   | 
| lgbtti                       |     NO      |   boolean   | 
| identidad_genero_id          |     NO      |   integer   | 
| orientacion_sexual_id        |     NO      |   integer   | 
| embarazada                   |     NO      |   boolean   | 
| numero_semanas_emb           |     NO      |   integer   | 
| dependientes                 |     NO      |   boolean   | 
| red_apoyo                    |     NO      |   boolean   | 
| adiccion                     |     NO      |   boolean   | 
| cual_adiccion                |     NO      |   integer[] | 
| persona_afrodescendiente     |     NO      |   string    | 
| caso_name                    |     NO      |   string    | 
| realiza_mas_actividades      |     NO      |   boolean   | 
| discapacidad_por_violencia   |     NO      |   boolean   | 
| gestacion_id                 |     NO      |   integer   | 
| clave_centro_escolar         |     NO      |   string    | 
| numero_seguro_social         |     NO      |   string    | 
| servicio_medico_id           |     NO      |   integer   | 
| otro_servicio_medico         |     NO      |   string    | 
| tipos_adicciones             |     NO      |   integer[] | 

#### Ejemplo de solicitud.
con curl.
```bash
curl --location 'API_URL/api/registrar-datos-victima-por-hecho' \
--header 'Accept: application/json' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer TOKEN' \
--data-raw '{
    "hechos_id"                    : 2,         
    "edad"                         : 33,                      
    "telefono"                     : "26460978",                  
    "correo_electronico"           : "prueba@fake.com",
    "calle"                        : "calle",          
    "num_exterior"                 : "e1",        
    "num_interior"                 : "i1",         
    "cve_ent"                      : 21,                       
    "cve_mun"                      : 132,                      
    "cve_loc"                      : 1,                    
    "colonias_id"                  : 1,        
    "entre_calle_uno"              : "uno",
    "entre_calle_dos"              : "dos",
    "referencia"                   : "referencia", 
    "estado_conyugal"              : 1, 
    "pais"                         : 153,
    "codigo_postal"                : 6400,
    "escolaridad_id"               : 1, 
    "ingreso_economico_id"         : 1, 
    "ocupacion_id"                 : 1,
    "presenta_discapacidad"        : true, 
    "discapacidad"                 : [1],           
    "otra_discapacidad"            : "sin definir", 
    "enfermedad"                   : true, 
    "cual_enfermedad"              : "descripción de la enfermedad",
    "enfermedad_psiquiatrica"      : true,
    "cual_enfermedad_psiquiatrica" : "psiquiatrica",
    "tratamiento"                  : true,
    "cual_tratamiento"             : "tratamiento",
    "situacion_calle"              : true, 
    "migrante"                     : true,
    "pueblo_indigena"              : true,
    "pueblo_indigena_id"           : 1, 
    "lgbtti"                       : true, 
    "identidad_genero_id"          : 1, 
    "orientacion_sexual_id"        : 1, 
    "embarazada"                   : true, 
    "numero_semanas_emb"           : 20, 
    "dependientes"                 : true, 
    "red_apoyo"                    : true,
    "adiccion"                     : true, 
    "cual_adiccion"                : [1], 
    "persona_afrodescendiente"     : "afro",  
    "caso_name"                    : "no name", 
    "realiza_mas_actividades"      : true, 
    "discapacidad_por_violencia"   : true,
    "gestacion_id"                 : 1, 
    "clave_centro_escolar"         : "Clave centro", 
    "numero_seguro_social"         : "AV0569806410",
    "servicio_medico_id"           : 1,
    "otro_servicio_medico"         : "otro servicio medico", 
    "tipos_adicciones"             : [1] 
}'
```
con PHP - cURL.
```php
<?php

$curl = curl_init();

curl_setopt_array($curl, array(
  CURLOPT_URL => 'API_URL/api/registrar-datos-victima-por-hecho',
  CURLOPT_RETURNTRANSFER => true,
  CURLOPT_ENCODING => '',
  CURLOPT_MAXREDIRS => 10,
  CURLOPT_TIMEOUT => 0,
  CURLOPT_FOLLOWLOCATION => true,
  CURLOPT_HTTP_VERSION => CURL_HTTP_VERSION_1_1,
  CURLOPT_CUSTOMREQUEST => 'POST',
  CURLOPT_POSTFIELDS =>'{
    "hechos_id"                    : 2,         
    "edad"                         : 33,                      
    "telefono"                     : "26460978",                  
    "correo_electronico"           : "prueba@fake.com",
    "calle"                        : "calle",          
    "num_exterior"                 : "e1",        
    "num_interior"                 : "i1",         
    "cve_ent"                      : 21,                       
    "cve_mun"                      : 132,                      
    "cve_loc"                      : 1,                    
    "colonias_id"                  : 1,        
    "entre_calle_uno"              : "uno",
    "entre_calle_dos"              : "dos",
    "referencia"                   : "referencia", 
    "estado_conyugal"              : 1, 
    "pais"                         : 153,
    "codigo_postal"                : 6400,
    "escolaridad_id"               : 1, 
    "ingreso_economico_id"         : 1, 
    "ocupacion_id"                 : 1,
    "presenta_discapacidad"        : true, 
    "discapacidad"                 : [1],           
    "otra_discapacidad"            : "sin definir", 
    "enfermedad"                   : true, 
    "cual_enfermedad"              : "descripción de la enfermedad",
    "enfermedad_psiquiatrica"      : true,
    "cual_enfermedad_psiquiatrica" : "psiquiatrica",
    "tratamiento"                  : true,
    "cual_tratamiento"             : "tratamiento",
    "situacion_calle"              : true, 
    "migrante"                     : true,
    "pueblo_indigena"              : true,
    "pueblo_indigena_id"           : 1, 
    "lgbtti"                       : true, 
    "identidad_genero_id"          : 1, 
    "orientacion_sexual_id"        : 1, 
    "embarazada"                   : true, 
    "numero_semanas_emb"           : 20, 
    "dependientes"                 : true, 
    "red_apoyo"                    : true,
    "adiccion"                     : true, 
    "cual_adiccion"                : [1], 
    "persona_afrodescendiente"     : "afro",  
    "caso_name"                    : "no name", 
    "realiza_mas_actividades"      : true, 
    "discapacidad_por_violencia"   : true,
    "gestacion_id"                 : 1, 
    "clave_centro_escolar"         : "Clave centro", 
    "numero_seguro_social"         : "AV0569806410",
    "servicio_medico_id"           : 1,
    "otro_servicio_medico"         : "otro servicio medico", 
    "tipos_adicciones"             : [1] 
}',
  CURLOPT_HTTPHEADER => array(
    'Accept: application/json',
    'Content-Type: application/json',
    'Authorization: Bearer TOKEN',
  ),
));

$response = curl_exec($curl);

curl_close($curl);
echo $response;
```
con JavaScrip - fetch.
```javascript
const myHeaders = new Headers();
myHeaders.append("Accept", "application/json");
myHeaders.append("Content-Type", "application/json");
myHeaders.append("Authorization", "Bearer TOKEN");

const raw = JSON.stringify({
  "hechos_id": 2,
  "edad": 33,
  "telefono": "26460978",
  "correo_electronico": "prueba@fake.com",
  "calle": "calle",
  "num_exterior": "e1",
  "num_interior": "i1",
  "cve_ent": 21,
  "cve_mun": 132,
  "cve_loc": 1,
  "colonias_id": 1,
  "entre_calle_uno": "uno",
  "entre_calle_dos": "dos",
  "referencia": "referencia",
  "estado_conyugal": 1,
  "pais": 153,
  "codigo_postal": 6400,
  "escolaridad_id": 1,
  "ingreso_economico_id": 1,
  "ocupacion_id": 1,
  "presenta_discapacidad": true,
  "discapacidad": [
    1
  ],
  "otra_discapacidad": "sin definir",
  "enfermedad": true,
  "cual_enfermedad": "descripción de la enfermedad",
  "enfermedad_psiquiatrica": true,
  "cual_enfermedad_psiquiatrica": "psiquiatrica",
  "tratamiento": true,
  "cual_tratamiento": "tratamiento",
  "situacion_calle": true,
  "migrante": true,
  "pueblo_indigena": true,
  "pueblo_indigena_id": 1,
  "lgbtti": true,
  "identidad_genero_id": 1,
  "orientacion_sexual_id": 1,
  "embarazada": true,
  "numero_semanas_emb": 20,
  "dependientes": true,
  "red_apoyo": true,
  "adiccion": true,
  "cual_adiccion": [
    1
  ],
  "persona_afrodescendiente": "afro",
  "caso_name": "no name",
  "realiza_mas_actividades": true,
  "discapacidad_por_violencia": true,
  "gestacion_id": 1,
  "clave_centro_escolar": "Clave centro",
  "numero_seguro_social": "AV0569806410",
  "servicio_medico_id": 1,
  "otro_servicio_medico": "otro servicio medico",
  "tipos_adicciones": [
    1
  ]
});

const requestOptions = {
  method: "POST",
  headers: myHeaders,
  body: raw,
  redirect: "follow"
};

fetch("API_URL/api/registrar-datos-victima-por-hecho", requestOptions)
  .then((response) => response.text())
  .then((result) => console.log(result))
  .catch((error) => console.error(error));
```
#### La respuesta que se recibirá después de la petición.
con estatus 200  en formato json.
```json
{
    "id": 1,
    "hechos_id": 2,
    "users_id": 1,
    "edad": 33,
    "telefono": "26460978",
    "correo_electronico": "prueba@fake.com",
    "calle": "calle",
    "num_exterior": "e1",
    "num_interior": "i1",
    "cve_ent": 21,
    "cve_mun": 132,
    "cve_loc": 1,
    "colonias_id": 1,
    "entre_calle_uno": "uno",
    "entre_calle_dos": "dos",
    "referencia": "referencia",
    "estado_conyugal": null,
    "pais": 153,
    "codigo_postal": 6400,
    "escolaridad_id": 1,
    "ingreso_economico_id": 1,
    "ocupacion_id": 1,
    "presenta_discapacidad": true,
    "discapacidad": [
        1
    ],
    "otra_discapacidad": "sin definir",
    "enfermedad": true,
    "cual_enfermedad": "descripción de la enfermedad",
    "enfermedad_psiquiatrica": true,
    "cual_enfermedad_psiquiatrica": "psiquiatrica",
    "tratamiento": true,
    "cual_tratamiento": "tratamiento",
    "situacion_calle": true,
    "migrante": true,
    "pueblo_indigena": true,
    "pueblo_indigena_id": 1,
    "lgbtti": true,
    "identidad_genero_id": 1,
    "orientacion_sexual_id": 1,
    "embarazada": true,
    "numero_semanas_emb": 20,
    "dependientes": true,
    "red_apoyo": true,
    "adiccion": true,
    "created_at": "2024-08-12T17:33:20.000000Z",
    "updated_at": "2024-11-13T01:49:51.000000Z",
    "cual_adiccion": [
        1
    ],
    "deleted_at": null,
    "persona_afrodescendiente": "afro",
    "caso_name": "no name",
    "realiza_mas_actividades": true,
    "discapacidad_por_violencia": true,
    "gestacion_id": 1,
    "clave_centro_escolar": "Clave centro",
    "numero_seguro_social": "AV0569806410",
    "servicio_medico_id": 1,
    "otro_servicio_medico": "otro servicio medico",
    "tipos_adicciones": [
        1
    ],
    "ocupacion": null,
    "escolaridad": null,
    "ingreso_economico": null,
    "colonia": null,
    "identidad_genero": null,
    "orientacion_sexual": null,
    "semanas_gestacion": {
        "id": null
    }
}
```
con estatus 422. Error de tipo de dato.
```json
{
    "message": "El campo NOMBRE_CAMPO debe ser una TIPO_DATO.",
    "errors": {
        "otra_discapacidad": [
            "El campo NOMBRE_CAMPO debe ser una TIPO_DATO."
        ]
    }
}
```
--------------------------------------------------------------
### Registro de un dependiente de una víctima por hecho de violencia
--------------------------------------------------------------
El registro de un dependiente de la víctima, tiene una dependencia funcional que requiere que la víctima tenga un hecho de víolencia registrado.
#### POST
```json
    API_URL/api/registrar-dependiente-victima-por-hecho
```
catálogo: [vinculo_victima_id](https://drive.google.com/file/d/1_F5CDwHh3Tq7JtOho6v1upeZYnIZf8C6/view)
#### Body raw (json)
```json
{
    "hechos_id"                    : 2,         
    "nombre"                       : "dependiente",
    "primer_apellido"              : "papellido",
    "segundo_apellido"             : "sapellido",
    "edad"                         : 3,
    "vinculo_victima_id"           : 1,
    "esta_en_riesgo"               : true,
    "presenta_discapacidad"        : true,
    "extranjera"                   : true,
    "enfermedad"                   : true,
    "cual_enfermedad"              : "enfermedad",
    "observaciones"                : "observación"
}
```
#### Campos Obligatorios

|             Campo       | Obligatorio |Tipo de dato |
|:-----------------------:|:-----------:|:-----------:|
| hechos_id               |     SI      |   integer   | 
| nombre                  |     NO      |   string    | 
| primer_apellido         |     NO      |   string    | 
| segundo_apellido        |     NO      |   string    | 
| edad                    |     NO      |   integer   | 
| vinculo_victima_id      |     NO      |   integer   | 
| esta_en_riesgo          |     NO      |   boolean   | 
| presenta_discapacidad   |     NO      |   boolean   | 
| extranjera              |     NO      |   boolean   | 
| enfermedad              |     NO      |   boolean   | 
| cual_enfermedad         |     NO      |   string    | 
| observaciones           |     NO      |   string    | 
| | |
#### Ejemplo de solicitud.
con curl.
```bash
    curl --location 'API_URL/api/registrar-dependiente-victima-por-hecho' \
    --header 'Accept: application/json' \
    --header 'Content-Type: application/json' \
    --header 'Authorization: Bearer TOKEN' \
    --data '{
        "hechos_id"                    : 2,         
        "nombre"                       : "dependiente",
        "primer_apellido"              : "papellido",
        "segundo_apellido"             : "sapellido",
        "edad"                         : 3,
        "vinculo_victima_id"           : 1,
        "esta_en_riesgo"               : true,
        "presenta_discapacidad"        : true,
        "extranjera"                   : true,
        "enfermedad"                   : true,
        "cual_enfermedad"              : "enfermedad",
        "observaciones"                : "observación"
    }'
```
con PHP - cURL.
```php
    <?php

    $curl = curl_init();

    curl_setopt_array($curl, array(
    CURLOPT_URL => 'API_URL/api/registrar-dependiente-victima-por-hecho',
    CURLOPT_RETURNTRANSFER => true,
    CURLOPT_ENCODING => '',
    CURLOPT_MAXREDIRS => 10,
    CURLOPT_TIMEOUT => 0,
    CURLOPT_FOLLOWLOCATION => true,
    CURLOPT_HTTP_VERSION => CURL_HTTP_VERSION_1_1,
    CURLOPT_CUSTOMREQUEST => 'POST',
    CURLOPT_POSTFIELDS =>'{
        "hechos_id"                    : 2,         
        "nombre"                       : "dependiente",
        "primer_apellido"              : "papellido",
        "segundo_apellido"             : "sapellido",
        "edad"                         : 3,
        "vinculo_victima_id"           : 1,
        "esta_en_riesgo"               : true,
        "presenta_discapacidad"        : true,
        "extranjera"                   : true,
        "enfermedad"                   : true,
        "cual_enfermedad"              : "enfermedad",
        "observaciones"                : "observación"
    }',
    CURLOPT_HTTPHEADER => array(
        'Accept: application/json',
        'Content-Type: application/json',
        'Authorization: Bearer TOKEN'
    ),
    ));

    $response = curl_exec($curl);

    curl_close($curl);
    echo $response;
```
con JavaScrip - fetch.
```javascript
    const myHeaders = new Headers();
    myHeaders.append("Accept", "application/json");
    myHeaders.append("Content-Type", "application/json");
    myHeaders.append("Authorization", "Bearer TOKEN");

    const raw = JSON.stringify({
    "hechos_id": 2,
    "nombre": "dependiente",
    "primer_apellido": "papellido",
    "segundo_apellido": "sapellido",
    "edad": 3,
    "vinculo_victima_id": 1,
    "esta_en_riesgo": true,
    "presenta_discapacidad": true,
    "extranjera": true,
    "enfermedad": true,
    "cual_enfermedad": "enfermedad",
    "observaciones": "onservación"
    });

    const requestOptions = {
    method: "POST",
    headers: myHeaders,
    body: raw,
    redirect: "follow"
    };

    fetch("API_URL/api/registrar-dependiente-victima-por-hecho", requestOptions)
    .then((response) => response.text())
    .then((result) => console.log(result))
    .catch((error) => console.error(error));
```
#### La respuesta que se recibirá después de la petición.
con estatus 200  en formato json.
```json
{
    "nombre": "dependiente",
    "primer_apellido": "papellido",
    "segundo_apellido": "sapellido",
    "edad": 3,
    "vinculo_victima_id": 1,
    "esta_en_riesgo": true,
    "presenta_discapacidad": true,
    "extranjera": true,
    "enfermedad": true,
    "cual_enfermedad": "enfermedad",
    "observaciones": "observación",
    "users_id": 1,
    "hechos_id": 2,
    "updated_at": "2024-11-14T00:18:38.000000Z",
    "created_at": "2024-11-14T00:18:38.000000Z",
    "id": 73
}
```
con estatus 422. El campo hechos id es requerido por la dependencia funcional.
```json
{
    "message": "El campo hechos id es requerido.",
    "errors": {
        "hechos_id": [
            "El campo hechos id es requerido."
        ]
    }
}
```
con estatus 422. Error de tipo de dato.
```json
{
    "message": "El campo NOMBRE_CAMPO debe ser una TIPO_DATO.",
    "errors": {
        "otra_discapacidad": [
            "El campo NOMBRE_CAMPO debe ser una TIPO_DATO."
        ]
    }
}
```
--------------------------------------------------------------
### Registro de la red de apoyo de una víctima por hecho de violencia
--------------------------------------------------------------
El registro de la red de apoyo requiere que la víctima tenga un hecho de víolencia registrado.
#### POST
```json
    API_URL/api/registrar-red-apoyo-victima-por-hecho
```
catálogo: [vinculo_victima_id](https://drive.google.com/file/d/1_F5CDwHh3Tq7JtOho6v1upeZYnIZf8C6/view)
#### Body raw (json)
```json
{
    "hechos_id"             : 1,
    "nombre"                : "Juanita",
    "primer_apellido"       : "papellido",
    "segundo_apellido"      : "sapellido",
    "vinculo_victima_id"    : 4,
    "telefono"              : "2258877731",
    "observaciones"         : "observaciones"
}
```
#### Campos Obligatorios

|        Campo       | Obligatorio | Tipo de dato |
|:------------------:|:-----------:|:------------:|
| hechos_id          |      SI     |    integer   | 
| nombre             |      NO     |    string    | 
| primer_apellido    |      NO     |    string    | 
| segundo_apellido   |      NO     |    string    | 
| vinculo_victima_id |      NO     |    integer   | 
| telefono           |      NO     |    string    | 
| observaciones      |      NO     |    string    | 
|||
#### Ejemplo de solicitud.
con curl.
```bash
    curl --location 'API_URL/api/registrar-red-apoyo-victima-por-hecho' \
    --header 'Accept: application/json' \
    --header 'Content-Type: application/json' \
    --header 'Authorization: Bearer TOKEN' \
    --data '{
        "hechos_id"             : 1,
        "nombre"                : "Juanita",
        "primer_apellido"       : "papellido",
        "segundo_apellido"      : "sapellido",
        "vinculo_victima_id"    : 4,
        "telefono"              : "2258877731",
        "observaciones"         : "observaciones"
    }'
```
con PHP - cURL.
```php
    <?php

    $curl = curl_init();

    curl_setopt_array($curl, array(
    CURLOPT_URL => 'API_URL/api/registrar-red-apoyo-victima-por-hecho',
    CURLOPT_RETURNTRANSFER => true,
    CURLOPT_ENCODING => '',
    CURLOPT_MAXREDIRS => 10,
    CURLOPT_TIMEOUT => 0,
    CURLOPT_FOLLOWLOCATION => true,
    CURLOPT_HTTP_VERSION => CURL_HTTP_VERSION_1_1,
    CURLOPT_CUSTOMREQUEST => 'POST',
    CURLOPT_POSTFIELDS =>'{
        "hechos_id"             : 1,
        "nombre"                : "Juanita",
        "primer_apellido"       : "papellido",
        "segundo_apellido"      : "sapellido",
        "vinculo_victima_id"    : 4,
        "telefono"              : "2258877731",
        "observaciones"         : "observaciones"
    }',
    CURLOPT_HTTPHEADER => array(
        'Accept: application/json',
        'Content-Type: application/json',
        'Authorization: Bearer TOKEN'
    ),
    ));

    $response = curl_exec($curl);

    curl_close($curl);
    echo $response;
```
con JavaScrip - fetch.
```javascript
    const myHeaders = new Headers();
    myHeaders.append("Accept", "application/json");
    myHeaders.append("Content-Type", "application/json");
    myHeaders.append("Authorization", "Bearer TOKEN");

    const raw = JSON.stringify({
    "hechos_id": 1,
    "nombre": "Juanita",
    "primer_apellido": "papellido",
    "segundo_apellido": "sapellido",
    "vinculo_victima_id": 4,
    "telefono": "2258877731",
    "observaciones": "observaciones"
    });

    const requestOptions = {
    method: "POST",
    headers: myHeaders,
    body: raw,
    redirect: "follow"
    };

    fetch("API_URL/api/registrar-red-apoyo-victima-por-hecho", requestOptions)
    .then((response) => response.text())
    .then((result) => console.log(result))
    .catch((error) => console.error(error));
```
#### La respuesta que se recibirá después de la petición.
con estatus 200  en formato json.
```json
{
    "nombre": "Juanita",
    "primer_apellido": "papellido",
    "segundo_apellido": "sapellido",
    "vinculo_victima_id": 4,
    "telefono": "2258877731",
    "observaciones": "observaciones",
    "users_id": 1,
    "hechos_id": 1,
    "updated_at": "2024-11-14T00:48:45.000000Z",
    "created_at": "2024-11-14T00:48:45.000000Z",
    "id": 51
}
```
con estatus 422. Error de tipo de dato.
```json
{
    "message": "El campo NOMBRE_CAMPO debe ser una TIPO_DATO.",
    "errors": {
        "otra_discapacidad": [
            "El campo NOMBRE_CAMPO debe ser una TIPO_DATO."
        ]
    }
}
con estatus 422. El hecho es un campo que debe envíar.
```json
{
    "message": "El campo hechos id es requerido.",
    "errors": {
        "hechos_id": [
            "El campo hechos id es requerido."
        ]
    }
}
```
--------------------------------------------------------------
Registro del seguimiento de un hecho de violencia.
--------------------------------------------------------------
El seguimiento requiere que exista un hecho de violencia para ser registrado, teniendo una dependencia funcional directa.
#### POST
```
    API_URL/api/seguimiento
```
catálogos: [cve_ent](https://drive.google.com/file/d/1Y163QX4ddN4J6w8ZGNUg_11-l4EM9V5w/view), [tipo_id](https://drive.google.com/file/d/1Tz1iLpC6t2DzMORBua1caDhzsWivB_WL/view) y [servicios_id](https://drive.google.com/file/d/1aK-fX_3OAdzQIF72efVlLA-20lJFOda1/view)
#### Body raw (json)
```json
    {
        "hechos_id"         : 1,
        "fecha_seguimiento" : "2023-10-27",
        "hora_seguimiento"    : "08:12",
        "tipo_id"             : 1,
        "servicios_id"        : 1,
        "acciones"            : "Se realizo la accion de prueba",
        "observaciones"       : "Se realizo la observacion de prueba"
    }
```
#### Campos Obligatorios

|   Campo           | Obligatorio |Tipo de dato |
|:-----------------:|:-----------:|:-----------:|
| hechos_id         |     SI      |   integer   |
| fecha_seguimiento |     SI      |    date     | 
| hora_seguimiento  |     SI      |    time     | 
| tipo_id           |     SI      |   integer   | 
| servicios_id      |     SI      |   integer   | 
| acciones          |     SI      |   string    | 
| observaciones     |     SI      |   string    | 
|||| 

#### Ejemplo de solicitud.
con curl.
```bash
    curl --location 'API_URL/api/seguimiento' \
    --header 'Accept: application/json' \
    --header 'Content-Type: application/json' \
    --header 'Authorization: Bearer TOKEN' \
    --data '{
        "hechos_id"         : 1,
        "fecha_seguimiento" : "2023-10-27",
        "hora_seguimiento"    : "08:12",
        "tipo_id"             : 1,
        "servicios_id"        : 1,
        "acciones"            : "Se realizo la accion de prueba",
        "observaciones"       : "Se realizo la observacion de prueba"
    }'
```
con PHP - cURL.
```php
    <?php

    $curl = curl_init();

    curl_setopt_array($curl, array(
    CURLOPT_URL => 'API_URL/api/seguimiento',
    CURLOPT_RETURNTRANSFER => true,
    CURLOPT_ENCODING => '',
    CURLOPT_MAXREDIRS => 10,
    CURLOPT_TIMEOUT => 0,
    CURLOPT_FOLLOWLOCATION => true,
    CURLOPT_HTTP_VERSION => CURL_HTTP_VERSION_1_1,
    CURLOPT_CUSTOMREQUEST => 'POST',
    CURLOPT_POSTFIELDS =>'{
        "hechos_id"         : 1,
        "fecha_seguimiento" : "2023-10-27",
        "hora_seguimiento"    : "08:12",
        "tipo_id"             : 1,
        "servicios_id"        : 1,
        "acciones"            : "Se realizo la accion de prueba",
        "observaciones"     : "Se realizo la observacion de prueba"
    }',
    CURLOPT_HTTPHEADER => array(
        'Accept: application/json',
        'Content-Type: application/json',
        'Authorization: Bearer TOKEN'
    ),
    ));

    $response = curl_exec($curl);

    curl_close($curl);
    echo $response;
```
con JavaScrip - fetch.
```javascript
    const myHeaders = new Headers();
    myHeaders.append("Accept", "application/json");
    myHeaders.append("Content-Type", "application/json");
    myHeaders.append("Authorization", "Bearer TOKEN");

    const raw = JSON.stringify({
    "hechos_id": 1,
    "fecha_seguimiento": "2023-10-27",
    "hora_seguimiento": "08:12",
    "tipo_id": 1,
    "servicios_id": 1,
    "acciones": "Se realizo la accion de prueba",
    "observaciones": "Se realizo la observacion de prueba"
    });

    const requestOptions = {
    method: "POST",
    headers: myHeaders,
    body: raw,
    redirect: "follow"
    };

    fetch("API_URL/api/seguimiento", requestOptions)
    .then((response) => response.text())
    .then((result) => console.log(result))
    .catch((error) => console.error(error));
```
#### La respuesta que se recibirá después de la petición.
con estatus 200  en formato json.
```json
{
    "seguimiento": {
        "users_id": 1,
        "hechos_id": 1,
        "fecha_seguimiento": "2023-10-27",
        "hora_seguimiento": "08:12",
        "tipo_id": 1,
        "servicios_id": 1,
        "acciones": "Se realizo la accion de prueba",
        "observaciones": "Se realizo la observacion de prueba",
        "updated_at": "2024-07-26T22:08:18.000000Z",
        "created_at": "2024-07-26T22:08:18.000000Z",
        "id": 9031
    }
}
```
con estatus 422. El hechos_id no existe en el sistema.
```json
    {
        "message": "El hecho de violencia no está registrado en el sistema",
        "errors": {
            "hechos_id": [
                "El hecho de violencia no está registrado en el sistema"
            ]
        }
    }
```
con estatus 422. .
```json
    {
        "message": "El campo fecha seguimiento es requerido. (and 5 more errors)",
        "errors": {
            "fecha_seguimiento": [
                "El campo fecha seguimiento es requerido."
            ],
            "hora_seguimiento": [
                "El campo hora seguimiento es requerido."
            ],
            "tipo_id": [
                "El campo tipo id es requerido."
            ],
            "servicios_id": [
                "El campo servicios id es requerido."
            ],
            "acciones": [
                "El campo acciones es requerido."
            ],
            "observaciones": [
                "El campo observaciones es requerido."
            ]
        }
    }
```

--------------------------------------------------------------
### Registro de mujeres en prisión para un hecho de violencia
--------------------------------------------------------------
El registro requiere que exista un hecho de violencia, teniendo una dependencia funcional directa.
#### POST
```
    API_URL/api/registrar-mujer-en-prision
```
catálogos: [calidad_legal_id](https://drive.google.com/file/d/1MpQvCjgHaLPso9DCxl6ugngvlyUwamRF/view), [tortura_tipo_id](https://drive.google.com/file/d/1ycr5AJKeDrzD8q3HfJAyCNuK_uZ26n_Z/view), [tortura_momento_id](https://drive.google.com/file/d/1taXbMYYtM7tfd4X4GaX1rZrRImRmIzYZ/view) y [autoridad_id](https://drive.google.com/file/d/1V5roC6VJQBAm_2D0Mw6VrapUoZSPlBP7/view).
#### Body raw (json)
```json
{
    "hechos_id"             : 1,
    "privada_de_libertad"   : true,
    "calidad_legal_id"      : 1,
    "delitos"               : "robo",
    "victima_de_tortura"    : true,
    "protocolo_de_estambul" : true,
    "victima_de_tortura_despues_del_protocolo_de_estambul" : true,
    "tortura_tipo_id"       : 1,
    "tortura_momento_id"    : 5,
    "autoridad_id"          : 1
}
```
#### Campos Obligatorios

|                            Campo                       | Obligatorio | Tipo de dato |
|:------------------------------------------------------:|:-----------:|:------------:|
| hechos_id                                              |     SI      |    integer   |
| privada_de_libertad                                    |     NO      |    boolean   |
| calidad_legal_id                                       |     NO      |    integer   |
| delitos                                                |     NO      |    string    |
| victima_de_tortura                                     |     NO      |    boolean   |
| protocolo_de_estambul                                  |     NO      |    boolean   |
| victima_de_tortura_despues_del_protocolo_de_estambul   |     NO      |    boolean   |
| tortura_tipo_id                                        |     NO      |    integer   |
| tortura_momento_id                                     |     NO      |    integer   |
| autoridad_id                                           |     NO      |    integer   |
|||
#### Ejemplo de solicitud.
con curl.
```bash
    curl --location 'API_URL/api/registrar-mujer-en-prision' \
    --header 'Accept: application/json' \
    --header 'Content-Type: application/json' \
    --header 'Authorization: Bearer TOKEN' \
    --data '{
        "hechos_id"             : 1,
        "privada_de_libertad"   : true,
        "calidad_legal_id"      : 1,
        "delitos"               : "robo",
        "victima_de_tortura"    : true,
        "protocolo_de_estambul" : true,
        "victima_de_tortura_despues_del_protocolo_de_estambul" : true,
        "tortura_tipo_id"       : 1,
        "tortura_momento_id"    : 5,
        "autoridad_id"          : 1
    }'
```
con PHP - cURL.
```php
    <?php

    $curl = curl_init();

    curl_setopt_array($curl, array(
    CURLOPT_URL => 'API_URL/api/registrar-mujer-en-prision',
    CURLOPT_RETURNTRANSFER => true,
    CURLOPT_ENCODING => '',
    CURLOPT_MAXREDIRS => 10,
    CURLOPT_TIMEOUT => 0,
    CURLOPT_FOLLOWLOCATION => true,
    CURLOPT_HTTP_VERSION => CURL_HTTP_VERSION_1_1,
    CURLOPT_CUSTOMREQUEST => 'POST',
    CURLOPT_POSTFIELDS =>'{
        "hechos_id"             : 1,
        "privada_de_libertad"   : true,
        "calidad_legal_id"      : 1,
        "delitos"               : "robo",
        "victima_de_tortura"    : true,
        "protocolo_de_estambul" : true,
        "victima_de_tortura_despues_del_protocolo_de_estambul" : true,
        "tortura_tipo_id"       : 1,
        "tortura_momento_id"    : 5,
        "autoridad_id"          : 1
    }',
    CURLOPT_HTTPHEADER => array(
        'Accept: application/json',
        'Content-Type: application/json',
        'Authorization: Bearer TOKEN'
    ),
    ));

    $response = curl_exec($curl);

    curl_close($curl);
    echo $response;
```
con JavaScrip - fetch.
```javascript
    const myHeaders = new Headers();
    myHeaders.append("Accept", "application/json");
    myHeaders.append("Content-Type", "application/json");
    myHeaders.append("Authorization", "Bearer TOKEN");

    const raw = JSON.stringify({
    "hechos_id": 1,
    "privada_de_libertad": true,
    "calidad_legal_id": 1,
    "delitos": "robo",
    "victima_de_tortura": true,
    "protocolo_de_estambul": true,
    "victima_de_tortura_despues_del_protocolo_de_estambul": true,
    "tortura_tipo_id": 1,
    "tortura_momento_id": 5,
    "autoridad_id": 1
    });

    const requestOptions = {
    method: "POST",
    headers: myHeaders,
    body: raw,
    redirect: "follow"
    };

    fetch("API_URL/api/registrar-mujer-en-prision", requestOptions)
    .then((response) => response.text())
    .then((result) => console.log(result))
    .catch((error) => console.error(error));
```
#### La respuesta que se recibirá después de la petición.
con estatus 200  en formato json.
```json
{
    "hechos_id": 1,
    "users_id": 1,
    "privada_de_libertad": true,
    "calidad_legal_id": 1,
    "delitos": "robo",
    "victima_de_tortura": true,
    "protocolo_de_estambul": true,
    "victima_de_tortura_despues_del_protocolo_de_estambul": true,
    "tortura_tipo_id": 1,
    "tortura_momento_id": 5,
    "autoridad_id": 1,
    "updated_at": "2024-08-05T21:25:09.000000Z",
    "created_at": "2024-08-05T21:25:09.000000Z",
    "id": 25
}
```
con estatus 422. Faltan parámetros.
```json
{
    "message": "El campo hechos id seleccionado no es válido.",
    "errors": {
        "hechos_id": [
            "El campo hechos id seleccionado no es válido."
        ]
    }
}
```
--------------------------------------------------------------
### Registro de mujeres víctimas de trata para un hecho de violencia
--------------------------------------------------------------
El registro requiere que exista un hecho de violencia, teniendo una dependencia funcional directa.
#### POST
```
    API_URL/api/registrar-mujer-victima-de-trata
```
catálogos: [accion_omision_dolosa_id](https://drive.google.com/file/d/13CdVeVrsyRmC9PmOj5kPkgw52Xrv2b2M/view), [fines_reclutamiento_id](https://drive.google.com/file/d/1jC7ZmP3p_CiuNqCcflfkDsilpbrLfx02/view), [accion_omision_dolosa_array](https://drive.google.com/file/d/13CdVeVrsyRmC9PmOj5kPkgw52Xrv2b2M/view) y [fines_reclutamiento_array](https://drive.google.com/file/d/1jC7ZmP3p_CiuNqCcflfkDsilpbrLfx02/view)
#### Body raw (json)
```json
    {
        "hechos_id"                     : 5,
        "victima_de_trata_de_personas"  : true,
        "accion_omision_dolosa_id"      : 8,
        "fines_reclutamiento_id"        : 4,
        "accion_omision_dolosa_array"   : [1,2,3],
        "fines_reclutamiento_array"     : [1,2,3]
    }
```
#### Campos Obligatorios

|              Campo            | Obligatorio  | Tipo de dato |
|:-----------------------------:|:------------:|:------------:|
| hechos_id                     |      SI      |    integer   |
| victima_de_trata_de_personas  |      NO      |    boolean   |
| accion_omision_dolosa_id      |      NO      |    integer   |
| fines_reclutamiento_id        |      NO      |    integer   |
| accion_omision_dolosa_array   |      NO      |   integer[]  |
| fines_reclutamiento_array     |      NO      |   integer[]  |
|||

#### Ejemplo de solicitud.
con curl.
```bash
    curl --location 'API_URL/api/registrar-mujer-victima-de-trata' \
    --header 'Accept: application/json' \
    --header 'Content-Type: application/json' \
    --header 'Authorization: Bearer TOKEN' \
    --data '{
        "hechos_id"                     : 5,
        "victima_de_trata_de_personas"  : true,
        "accion_omision_dolosa_id"      : 8,
        "fines_reclutamiento_id"        : 4,
        "accion_omision_dolosa_array"   : [1,2,3],
        "fines_reclutamiento_array"     : [1,2, 3]
    }'
```
con PHP - cURL.
```php
    <?php

    $curl = curl_init();

    curl_setopt_array($curl, array(
    CURLOPT_URL => 'API_URL/api/registrar-mujer-victima-de-trata',
    CURLOPT_RETURNTRANSFER => true,
    CURLOPT_ENCODING => '',
    CURLOPT_MAXREDIRS => 10,
    CURLOPT_TIMEOUT => 0,
    CURLOPT_FOLLOWLOCATION => true,
    CURLOPT_HTTP_VERSION => CURL_HTTP_VERSION_1_1,
    CURLOPT_CUSTOMREQUEST => 'POST',
    CURLOPT_POSTFIELDS =>'{
        "hechos_id"                     : 5,
        "victima_de_trata_de_personas"  : true,
        "accion_omision_dolosa_id"      : 8,
        "fines_reclutamiento_id"        : 4,
        "accion_omision_dolosa_array"   : [1,2,3],
        "fines_reclutamiento_array"     : [1,2, 3]
    }',
    CURLOPT_HTTPHEADER => array(
        'Accept: application/json',
        'Content-Type: application/json',
        'Authorization: Bearer TOKEN'
    ),
    ));

    $response = curl_exec($curl);

    curl_close($curl);
    echo $response;
```
con JavaScrip - fetch.
```javascript
    const myHeaders = new Headers();
    myHeaders.append("Accept", "application/json");
    myHeaders.append("Content-Type", "application/json");
    myHeaders.append("Authorization", "Bearer TOKEN");

    const raw = JSON.stringify({
    "hechos_id": 5,
    "victima_de_trata_de_personas": true,
    "accion_omision_dolosa_id": 8,
    "fines_reclutamiento_id": 4,
    "accion_omision_dolosa_array": [1,2,3],
    "fines_reclutamiento_array": [1,2,3]
    });

    const requestOptions = {
    method: "POST",
    headers: myHeaders,
    body: raw,
    redirect: "follow"
    };

    fetch("API_URL/api/registrar-mujer-victima-de-trata", requestOptions)
    .then((response) => response.text())
    .then((result) => console.log(result))
    .catch((error) => console.error(error));
```
#### La respuesta que se recibirá después de la petición.
con estatus 200  en formato json.
```json
{
    "hechos_id": 5,
    "users_id": 1,
    "victima_de_trata_de_personas": true,
    "accion_omision_dolosa_id": 8,
    "fines_reclutamiento_id": 4,
    "accion_omision_dolosa_array": "[1,2,3]",
    "fines_reclutamiento_array": "[1,2,3]",
    "updated_at": "2024-08-05T22:13:39.000000Z",
    "created_at": "2024-08-05T22:13:39.000000Z",
    "id": 27
}
```
con estatus 422. Faltan parámetros.
```json
{
    "message": "El campo hechos id es requerido.",
    "errors": {
        "hechos_id": [
            "El campo hechos id es requerido."
        ]
    }
}
```

--------------------------------------------------------------
### Registro de mujeres desaparecidas para un hecho de violencia
--------------------------------------------------------------
El registro requiere que exista un hecho de violencia, teniendo una dependencia funcional directa.
#### POST
```
    API_URL/api/registrar-mujer-desaparecida
```
catálogos: [tipo_de_desaparicion_id](https://drive.google.com/file/d/1iILF7y_7DU-U5l_AKquuuqarLu1AA-6y/view), [cve_ent](https://drive.google.com/file/d/1Y163QX4ddN4J6w8ZGNUg_11-l4EM9V5w/view), [cve_mun](https://drive.google.com/file/d/19q9v31lH0Dgq7bsCBpO3hGfr_mT5VfNk/view), [vinculo_victima_id](https://drive.google.com/file/d/1_F5CDwHh3Tq7JtOho6v1upeZYnIZf8C6/view) y [estatus_desaparicion_id](https://drive.google.com/file/d/16k8t3y2nOkq1xhZ-7aGx77V333Yq__eg/view)
#### Body raw (json)
```json
{
    "hechos_id"                 : 5,
    "persona_desaparecida"      : true,
    "tipo_de_desaparicion_id"   : 2,
    "fecha_de_desaparicion"     : "2022-11-1",
    "cve_ent"                   : 21,
    "cve_mun"                   : 132,
    "es_institucion"            : true,
    "nombre_institucion"        : "CCI",
    "nombre"                    : "JP",
    "apellido_paterno"          : "MRT",
    "apellido_materno"          : "MKT",
    "edad"                      : 23,
    "vinculo_victima_id"        : 4,
    "estatus_desaparicion_id"   : 1,
    "con_vida"                  : true
}
```
#### Campos Obligatorios

|          Campo           |  Obligatorio | Tipo de dato |
|:------------------------:|:------------:|:------------:|
| hechos_id                |      SI      |    integer   |
| persona_desaparecida     |      NO      |    boolean   |
| tipo_de_desaparicion_id  |      NO      |    integer   | 
| fecha_de_desaparicion    |      NO      |     date     |
| cve_ent                  |      NO      |    integer   |
| cve_mun                  |      NO      |    integar   |
| es_institucion           |      NO      |    boolean   |
| nombre_institucion       |      NO      |    string    |
| nombre                   |      NO      |    string    |
| apellido_paterno         |      NO      |    string    |
| apellido_materno         |      NO      |    string    |
| edad                     |      NO      |    integer   |
| vinculo_victima_id       |      NO      |    integer   |
| estatus_desaparicion_id  |      NO      |    integer   |
| con_vida                 |      NO      |    boolean   |
|||

#### Ejemplo de solicitud.
con curl.
```bash
    curl --location 'API_URL/api/registrar-mujer-victima-de-trata' \
    --header 'Accept: application/json' \
    --header 'Content-Type: application/json' \
    --header 'Authorization: Bearer TOKEN' \
    --data '{
        "hechos_id"                 : 5,
        "persona_desaparecida"      : true,
        "tipo_de_desaparicion_id"   : 2,
        "fecha_de_desaparicion"     : "2022-11-1",
        "cve_ent"                   : 21,
        "cve_mun"                   : 132,
        "es_institucion"            : true,
        "nombre_institucion"        : "CCI",
        "nombre"                    : "JP",
        "apellido_paterno"          : "MRT",
        "apellido_materno"          : "MKT",
        "edad"                      : 23,
        "vinculo_victima_id"        : 4,
        "estatus_desaparicion_id"   : 1,
        "con_vida"                  : true
    }'
```
con PHP - cURL.
```php
    <?php

    $curl = curl_init();

    curl_setopt_array($curl, array(
    CURLOPT_URL => 'API_URL/api/registrar-mujer-victima-de-trata',
    CURLOPT_RETURNTRANSFER => true,
    CURLOPT_ENCODING => '',
    CURLOPT_MAXREDIRS => 10,
    CURLOPT_TIMEOUT => 0,
    CURLOPT_FOLLOWLOCATION => true,
    CURLOPT_HTTP_VERSION => CURL_HTTP_VERSION_1_1,
    CURLOPT_CUSTOMREQUEST => 'POST',
    CURLOPT_POSTFIELDS =>'{
        "hechos_id"                 : 5,
        "persona_desaparecida"      : true,
        "tipo_de_desaparicion_id"   : 2,
        "fecha_de_desaparicion"     : "2022-11-1",
        "cve_ent"                   : 21,
        "cve_mun"                   : 132,
        "es_institucion"            : true,
        "nombre_institucion"        : "CCI",
        "nombre"                    : "JP",
        "apellido_paterno"          : "MRT",
        "apellido_materno"          : "MKT",
        "edad"                      : 23,
        "vinculo_victima_id"        : 4,
        "estatus_desaparicion_id"   : 1,
        "con_vida"                  : true
    }',
    CURLOPT_HTTPHEADER => array(
        'Accept: application/json',
        'Content-Type: application/json',
        'Authorization: TOKEN'
    ),
    ));

    $response = curl_exec($curl);

    curl_close($curl);
    echo $response;
```
con JavaScrip - fetch.
```javascript
    const myHeaders = new Headers();
    myHeaders.append("Accept", "application/json");
    myHeaders.append("Content-Type", "application/json");
    myHeaders.append("Authorization", "Bearer TOKEN");

    const raw = JSON.stringify({
    "hechos_id": 5,
    "persona_desaparecida": true,
    "tipo_de_desaparicion_id": 2,
    "fecha_de_desaparicion": "2022-11-1",
    "cve_ent": 21,
    "cve_mun": 132,
    "es_institucion": true,
    "nombre_institucion": "CCI",
    "nombre": "JP",
    "apellido_paterno": "MRT",
    "apellido_materno": "MKT",
    "edad": 23,
    "vinculo_victima_id": 4,
    "estatus_desaparicion_id": 1,
    "con_vida": true
    });

    const requestOptions = {
    method: "POST",
    headers: myHeaders,
    body: raw,
    redirect: "follow"
    };

    fetch("API_URL/api/registrar-mujer-victima-de-trata", requestOptions)
    .then((response) => response.text())
    .then((result) => console.log(result))
    .catch((error) => console.error(error));
```
#### La respuesta que se recibirá después de la petición.
con estatus 200  en formato json.
```json
{
    "hechos_id": 5,
    "users_id": 1,
    "persona_desaparecida": true,
    "tipo_de_desaparicion_id": 2,
    "fecha_de_desaparicion": "2022-11-1",
    "cve_ent": 21,
    "cve_mun": 132,
    "es_institucion": true,
    "nombre_institucion": "CCI",
    "nombre": "JP",
    "apellido_paterno": "MRT",
    "apellido_materno": "MKT",
    "edad": 23,
    "vinculo_victima_id": 4,
    "estatus_desaparicion_id": 1,
    "con_vida": true,
    "updated_at": "2024-08-05T23:09:57.000000Z",
    "created_at": "2024-08-05T23:09:57.000000Z",
    "id": 24
}
```
con estatus 422. Faltan parámetros.
```json
{
    "message": "El campo hechos id es requerido.",
    "errors": {
        "hechos_id": [
            "El campo hechos id es requerido."
        ]
    }
}
```
--------------------------------------------------------------
### Registro una canalización para un hecho de violencia
--------------------------------------------------------------
El registro requiere que exista un hecho de violencia, teniendo una dependencia funcional directa.
#### POST
```
    API_URL/api/registrar-canalizacion
```
catálogos: [instancias_id](https://drive.google.com/file/d/152JS404Y43GSvJnTWFx3K7DSuQJX9OUK/view), [instancias_envia_id](https://drive.google.com/file/d/152JS404Y43GSvJnTWFx3K7DSuQJX9OUK/view), [cve_ent](https://drive.google.com/file/d/1Y163QX4ddN4J6w8ZGNUg_11-l4EM9V5w/view), [cve_mun](https://drive.google.com/file/d/19q9v31lH0Dgq7bsCBpO3hGfr_mT5VfNk/view), tipo_id y [servicios_id](https://drive.google.com/file/d/1Tz1iLpC6t2DzMORBua1caDhzsWivB_WL/view)
#### Body raw (json)
```json
{
    "hechos_id"                 : 5,
    "instancias_id"             : 251,
    "instancias_envia_id"       : 405,
    "cve_ent"                   : 21,
    "cve_mun"                   : 132,
    "fecha_canalizacion"        : "2022-03-2",
    "observaciones_incompetencia" : "nope",
    "observaciones"             : "nein",
    "acompanamiento"            : false,
    "estatus"                   : "ahí va",
    "nombre"                    : "JJJJ",
    "primer_apellido"           : "PPPP",
    "segundo_apellido"          : "ZZZZZ",
    "tipo_id"                   : 1,
    "servicios_id"              : 1
}
```
#### Campos Obligatorios

|             Campo             | Obligatorio | Tipo de dato |
|:-----------------------------:|:-----------:|:------------:|
| hechos_id                     |      SI     |    integer   | 
| instancias_id                 |      SI     |    integer   |
| instancias_envia_id           |      SI     |    integer   |
| cve_ent                       |      NO     |    integer   |
| cve_mun                       |      NO     |    integer   |
| fecha_canalizacion            |      SI     |     date     |
| observaciones_incompetencia   |      NO     |    string    |
| observaciones                 |      SI     |    string    |
| acompanamiento                |      SI     |    boolean   |
| estatus                       |      NO     |    string    |
| nombre                        |      NO     |    string    |
| primer_apellido               |      NO     |    string    |
| segundo_apellido              |      NO     |    string    |
| tipo_id                       |      SI     |    integer   |
| servicios_id                  |      SI     |    integer   |
|||

#### Ejemplo de solicitud.
con curl.
```bash
    curl --location 'API_URL/api/registrar-canalizacion' \
    --header 'Accept: application/json' \
    --header 'Content-Type: application/json' \
    --header 'Authorization: Bearer TOKEN' \
    --data '{
        "hechos_id"                 : 5,
        "instancias_id"             : 251,
        "instancias_envia_id"       : 405,
        "cve_ent"                   : 21,
        "cve_mun"                   : 132,
        "fecha_canalizacion"        : "2022-03-2",
        "observaciones_incompetencia" : "nope",
        "observaciones"             : "nein",
        "acompanamiento"            : false,
        "estatus"                   : "ahí va",
        "nombre"                    : "JJJJ",
        "primer_apellido"           : "PPPP",
        "segundo_apellido"          : "ZZZZZ",
        "tipo_id"                   : 1,
        "servicios_id"              : 1
    }'
```
con PHP - cURL.
```php
    <?php

    $curl = curl_init();

    curl_setopt_array($curl, array(
    CURLOPT_URL => 'API_URL/api/registrar-canalizacion',
    CURLOPT_RETURNTRANSFER => true,
    CURLOPT_ENCODING => '',
    CURLOPT_MAXREDIRS => 10,
    CURLOPT_TIMEOUT => 0,
    CURLOPT_FOLLOWLOCATION => true,
    CURLOPT_HTTP_VERSION => CURL_HTTP_VERSION_1_1,
    CURLOPT_CUSTOMREQUEST => 'POST',
    CURLOPT_POSTFIELDS =>'{
        "hechos_id"                 : 5,
        "instancias_id"             : 251,
        "instancias_envia_id"       : 405,
        "cve_ent"                   : 21,
        "cve_mun"                   : 132,
        "fecha_canalizacion"        : "2022-03-2",
        "observaciones_incompetencia" : "nope",
        "observaciones"             : "nein",
        "acompanamiento"            : false,
        "estatus"                   : "ahí va",
        "nombre"                    : "JJJJ",
        "primer_apellido"           : "PPPP",
        "segundo_apellido"          : "ZZZZZ",
        "tipo_id"                   : 1,
        "servicios_id"              : 1
    }',
    CURLOPT_HTTPHEADER => array(
        'Accept: application/json',
        'Content-Type: application/json',
        'Authorization: Bearer TOKEN'
    ),
    ));

    $response = curl_exec($curl);

    curl_close($curl);
    echo $response;
```
con JavaScrip - fetch.
```javascript
    const myHeaders = new Headers();
    myHeaders.append("Accept", "application/json");
    myHeaders.append("Content-Type", "application/json");
    myHeaders.append("Authorization", "Bearer TOKEN");

    const raw = JSON.stringify({
    "hechos_id": 5,
    "instancias_id": 251,
    "instancias_envia_id": 405,
    "cve_ent": 21,
    "cve_mun": 132,
    "fecha_canalizacion": "2022-03-2",
    "observaciones_incompetencia": "nope",
    "observaciones": "nein",
    "acompanamiento": false,
    "estatus": "ahí va",
    "nombre": "JJJJ",
    "primer_apellido": "PPPP",
    "segundo_apellido": "ZZZZZ",
    "tipo_id": 1,
    "servicios_id": 1
    });

    const requestOptions = {
    method: "POST",
    headers: myHeaders,
    body: raw,
    redirect: "follow"
    };

    fetch("API_URL/api/registrar-canalizacion", requestOptions)
    .then((response) => response.text())
    .then((result) => console.log(result))
    .catch((error) => console.error(error));
```
#### La respuesta que se recibirá después de la petición.
con estatus 200  en formato json.
```json
    {
        "hechos_id": 5,
        "users_id": 1,
        "instancias_id": 251,
        "instancias_envia_id": 405,
        "cve_ent": 21,
        "cve_mun": 132,
        "fecha_canalizacion": "2022-03-2",
        "observaciones_incompetencia": "nope",
        "observaciones": "nein",
        "acompanamiento": false,
        "estatus": "ahí va",
        "nombre": "JJJJ",
        "primer_apellido": "PPPP",
        "segundo_apellido": "ZZZZZ",
        "tipo_id": 1,
        "servicios_id": 1,
        "updated_at": "2024-08-05T23:29:44.000000Z",
        "created_at": "2024-08-05T23:29:44.000000Z",
        "id": 56
    }
```
con estatus 422. Faltan parámetros..
```json
{
    "message": "El campo hechos id es requerido. (and 7 more errors)",
    "errors": {
        "hechos_id": [
            "El campo hechos id es requerido."
        ],
        "instancias_id": [
            "El campo instancias id es requerido."
        ],
        "instancias_envia_id": [
            "El campo instancias envia id es requerido."
        ],
        "fecha_canalizacion": [
            "El campo fecha canalizacion es requerido."
        ],
        "observaciones": [
            "El campo observaciones es requerido."
        ],
        "acompanamiento": [
            "El campo acompanamiento es requerido."
        ],
        "tipo_id": [
            "El campo tipo id es requerido."
        ],
        "servicios_id": [
            "El campo servicios id es requerido."
        ]
    }
}
```
--------------------------------------------------------------
### Registro de un agresor para un hecho de violencia
--------------------------------------------------------------
El registro requiere que exista un hecho de violencia, teniendo una dependencia funcional directa.
#### POST
```
    API_URL/api/registrar-agresor
```
catálogos: [sexo_id](https://docs.google.com/spreadsheets/d/1KhLoeg4tQSjiADualz6AvGSHHuA4hlr7FANCYOx2HyI/edit?gid=0#gid=0), [tipos_armas](https://drive.google.com/file/d/1yFUao4LN0Qcs0rP2OP_YeykQoDHZXP12/view), [vinculo_victima_id](https://drive.google.com/file/d/1_F5CDwHh3Tq7JtOho6v1upeZYnIZf8C6/view), [identidad_genero_id](https://drive.google.com/file/d/1K7jBCF4E6aiBfnrg4ioD5cqh6gbFOXcL/view), [orientacion_sexual_id](https://drive.google.com/file/d/1hc5Yhl2gr6_pWBFED7G0QMGyooVzIS5x/view), [cve_ent](https://drive.google.com/file/d/1Y163QX4ddN4J6w8ZGNUg_11-l4EM9V5w/view), [cve_mun](https://drive.google.com/file/d/19q9v31lH0Dgq7bsCBpO3hGfr_mT5VfNk/view), [cve_loc](https://drive.google.com/file/d/1VDHjmDqURqkLc5MQb84MKhI9NlVrg3m3/view), [colonias_id](https://drive.google.com/file/d/1wWVhPLa0zFgdoeQYEtCRNfODEcpQx6BH/view), [escolaridad_id](https://drive.google.com/file/d/1Pf_eJpt_S34Ipo908Ih7RS-kQHfwJQGr/view), [ingreso_economico_id](https://docs.google.com/spreadsheets/d/15yA5gPDJXZFZkW1fiFKhrCXeQT1uVGJB/edit?gid=717444154#gid=717444154), [ocupacion_id](https://drive.google.com/file/d/1tY37QRvcZa0c-vzlSsgEpIRJnN2BqCw_/view), [tipos_drogas](https://drive.google.com/file/d/1YDUsKIZLzOA32YrEa4LekGQ0DY2wqFl_/view).
#### Body raw (json)
```json
    {
        "hechos_id"                 : 5,
        "nombre"                    : "FFFF",
        "primer_apellido"           : "AAAA",
        "segundo_apellido"          : "AAAA",
        "curp"                      : "CURP000000XXXXXXX0",
        "sexo_id"                   : 1,
        "edad"                      : 50,
        "mismo_domicilio_victima"   : true,
        "acceso_a_armas"            : true,
        "tipos_armas"               : [1],
        "vinculo_victima_id"        : 5,
        "identidad_genero_id"       : null,
        "orientacion_sexual_id"     : 1,
        "calle"                     : "FFFFFFFF",
        "num_exterior"              : "23 A",
        "num_interior"              : "5B, piso 3",
        "cve_ent"                   : 21,
        "cve_mun"                   : 132,
        "cve_loc"                   : 1,
        "codigo_postal"             : "74000",
        "colonias_id"               : 16813,
        "entre_calle_uno"           : "entre_calle_uno",
        "entre_calle_dos"           : "entre_calle_dos",
        "referencia"                : null,
        "escolaridad_id"            : null,
        "ingreso_economico_id"      : null,
        "ocupacion_id"              : 3,
        "conoce_persona"            : true,
        "consume_drogas"            : true,
        "tipos_drogas"              : [1]
    }
```
#### Campos Obligatorios

|             Campo         | Obligatorio | Tipo de dato |
|:-------------------------:|:-----------:|:------------:|
| hechos_id                 |      SI     |    integer   |
| nombre                    |      SI     |    string    |
| primer_apellido           |      SI     |    string    |
| segundo_apellido          |      SI     |    string    |
| curp                      |      SI     |    string    |
| sexo_id                   |      SI     |    integer   |
| edad                      |      NO     |    integer   |
| mismo_domicilio_victima   |      NO     |    bolean    |
| acceso_a_armas            |      NO     |    boolean   |
| tipos_armas               |      NO     |   integer[]  | 
| vinculo_victima_id        |      NO     |    integer   | 
| identidad_genero_id       |      NO     |    integer   |
| orientacion_sexual_id     |      NO     |    integer   |
| calle                     |      NO     |    string    |
| num_exterior              |      NO     |    string    |
| num_interior              |      NO     |    string    |
| cve_ent                   |      NO     |    integer   |
| cve_mun                   |      NO     |    integer   |
| cve_loc                   |      NO     |    integer   |
| codigo_postal             |      NO     |    string    |
| colonias_id               |      NO     |    integer   |
| entre_calle_uno           |      NO     |    string    |
| entre_calle_dos           |      NO     |    string    |
| referencia                |      NO     |    string    |
| escolaridad_id            |      NO     |    integer   |
| ingreso_economico_id      |      NO     |    integer   |
| ocupacion_id              |      NO     |    integer   |
| conoce_persona            |      SI     |    boolean   |
| consume_drogas            |      NO     |    boolean   |
| tipos_drogas              |      NO     |   integer[]  |
|||

#### Ejemplo de solicitud.
con curl.
```bash
    curl --location 'API_URL/api/registrar-agresor' \
    --header 'Accept: application/json' \
    --header 'Content-Type: application/json' \
    --header 'Authorization: Bearer TOKEN' \
    --data '{
        "hechos_id"                 : 5,
        "nombre"                    : "FFFF",
        "primer_apellido"           : "AAAA",
        "segundo_apellido"          : "AAAA",
        "curp"                      : "CURP000000XXXXXXX0",
        "sexo_id"                   : 1,
        "edad"                      : 50,
        "mismo_domicilio_victima"   : true,
        "acceso_a_armas"            : true,
        "tipos_armas"               : [1],
        "vinculo_victima_id"        : 5,
        "identidad_genero_id"       : null,
        "orientacion_sexual_id"     : 1,
        "calle"                     : "FFFFFFFF",
        "num_exterior"              : "23 A",
        "num_interior"              : "5B, piso 3",
        "cve_ent"                   : 21,
        "cve_mun"                   : 132,
        "cve_loc"                   : 1,
        "codigo_postal"             : 74000,
        "colonias_id"               : 16813,
        "entre_calle_uno"           : null,
        "entre_calle_dos"           : null,
        "referencia"                :null,
        "escolaridad_id"            : null,
        "ingreso_economico_id"      : null,
        "ocupacion_id"              : 3,
        "conoce_persona"            : true,
        "consume_drogas"            : true,
        "tipos_drogas"              : [1]
    }'
```
con PHP - cURL.
```php
    <?php

    $curl = curl_init();

    curl_setopt_array($curl, array(
    CURLOPT_URL => 'API_URL/api/registrar-agresor',
    CURLOPT_RETURNTRANSFER => true,
    CURLOPT_ENCODING => '',
    CURLOPT_MAXREDIRS => 10,
    CURLOPT_TIMEOUT => 0,
    CURLOPT_FOLLOWLOCATION => true,
    CURLOPT_HTTP_VERSION => CURL_HTTP_VERSION_1_1,
    CURLOPT_CUSTOMREQUEST => 'POST',
    CURLOPT_POSTFIELDS =>'{
        "hechos_id"                 : 5,
        "nombre"                    : "FFFF",
        "primer_apellido"           : "AAAA",
        "segundo_apellido"          : "AAAA",
        "curp"                      : "CURP000000XXXXXXX0",
        "sexo_id"                   : 1,
        "edad"                      : 50,
        "mismo_domicilio_victima"   : true,
        "acceso_a_armas"            : true,
        "tipos_armas"               : [1],
        "vinculo_victima_id"        : 5,
        "identidad_genero_id"       : null,
        "orientacion_sexual_id"     : 1,
        "calle"                     : "FFFFFFFF",
        "num_exterior"              : "23 A",
        "num_interior"              : "5B, piso 3",
        "cve_ent"                   : 21,
        "cve_mun"                   : 132,
        "cve_loc"                   : 1,
        "codigo_postal"             : 74000,
        "colonias_id"               : 16813,
        "entre_calle_uno"           : null,
        "entre_calle_dos"           : null,
        "referencia"                :null,
        "escolaridad_id"            : null,
        "ingreso_economico_id"      : null,
        "ocupacion_id"              : 3,
        "conoce_persona"            : true,
        "consume_drogas"            : true,
        "tipos_drogas"              : [1]
    }',
    CURLOPT_HTTPHEADER => array(
        'Accept: application/json',
        'Content-Type: application/json',
        'Authorization: Bearer TOKEN'
    ),
    ));

    $response = curl_exec($curl);

    curl_close($curl);
    echo $response;
```
con JavaScrip - fetch.
```javascript
    const myHeaders = new Headers();
    myHeaders.append("Accept", "application/json");
    myHeaders.append("Content-Type", "application/json");
    myHeaders.append("Authorization", "Bearer TOKEN");

    const raw = JSON.stringify({
    "hechos_id": 5,
    "nombre": "FFFF",
    "primer_apellido": "AAAA",
    "segundo_apellido": "AAAA",
    "curp": "CURP000000XXXXXXX0",
    "sexo_id": 1,
    "edad": 50,
    "mismo_domicilio_victima": true,
    "acceso_a_armas": true,
    "tipos_armas": [1],
    "vinculo_victima_id": 5,
    "identidad_genero_id": null,
    "orientacion_sexual_id": 1,
    "calle": "FFFFFFFF",
    "num_exterior": "23 A",
    "num_interior": "5B, piso 3",
    "cve_ent": 21,
    "cve_mun": 132,
    "cve_loc": 1,
    "codigo_postal": 74000,
    "colonias_id": 16813,
    "entre_calle_uno": null,
    "entre_calle_dos": null,
    "referencia": null,
    "escolaridad_id": null,
    "ingreso_economico_id": null,
    "ocupacion_id": 3,
    "conoce_persona": true,
    "consume_drogas": true,
    "tipos_drogas": [
        1
    ]
    });

    const requestOptions = {
    method: "POST",
    headers: myHeaders,
    body: raw,
    redirect: "follow"
    };

    fetch("API_URL/api/registrar-agresor", requestOptions)
    .then((response) => response.text())
    .then((result) => console.log(result))
    .catch((error) => console.error(error));
```
#### La respuesta que se recibirá después de la petición.
con estatus 200  en formato json.
```json
{
    "hechos_id": 5,
    "users_id": 1,
    "nombre": "FFFF",
    "primer_apellido": "AAAA",
    "segundo_apellido": "AAAA",
    "curp": "CURP000000XXXXXXX0",
    "sexo_id": 1,
    "edad": 50,
    "mismo_domicilio_victima": true,
    "acceso_a_armas": true,
    "tipos_armas": "[1]",
    "vinculo_victima_id": 5,
    "identidad_genero_id": null,
    "orientacion_sexual_id": 1,
    "calle": "FFFFFFFF",
    "num_exterior": "23 A",
    "num_interior": "5B, piso 3",
    "cve_ent": 21,
    "cve_mun": 132,
    "cve_loc": 1,
    "codigo_postal": 74000,
    "colonias_id": 16813,
    "entre_calle_uno": null,
    "entre_calle_dos": null,
    "referencia": null,
    "escolaridad_id": null,
    "ingreso_economico_id": null,
    "ocupacion_id": 3,
    "conoce_persona": true,
    "consume_drogas": true,
    "tipos_drogas": "[1]",
    "updated_at": "2024-08-05T23:40:17.000000Z",
    "created_at": "2024-08-05T23:40:17.000000Z",
    "id": 54
}
```
con estatus 422. Faltan parámetros.
```json
{
    "message": "El campo hechos id es requerido. (and 6 more errors)",
    "errors": {
        "hechos_id": [
            "El campo hechos id es requerido."
        ],
        "nombre": [
            "El campo nombre es requerido."
        ],
        "primer_apellido": [
            "El campo primer apellido es requerido."
        ],
        "segundo_apellido": [
            "El campo segundo apellido es requerido."
        ],
        "curp": [
            "El campo curp es requerido."
        ],
        "sexo_id": [
            "El campo sexo id es requerido."
        ],
        "conoce_persona": [
            "El campo conoce persona es requerido."
        ]
    }
}
```
-----------------------------------------------------------------
### Editar hecho de violencia
-----------------------------------------------------------------
El hecho de violencia tiene una dependencia funcional, que requiere que la víctima esté registrada. Además de tener un registro previo del hecho.
#### POST
```json
    http://API_URL/api/edita-hecho-de-violencia
```
catálogos: [cve_ent](https://drive.google.com/file/d/1Y163QX4ddN4J6w8ZGNUg_11-l4EM9V5w/view?usp=sharing), [lugar_id](https://drive.google.com/file/d/1_rs52tWT5M-6U0AVn3texkHHpeSwtmRZ/view), [lugar_detalle_id](https://drive.google.com/file/d/1HvwJJpAHiQ7h5XnzVLAMT6tDnUCZZnZk/view), [pais_id](https://drive.google.com/file/d/1-5gPflCkSALusWLp_pUN20NJbqEWE6ir/view), [cve_mun](https://drive.google.com/file/d/19q9v31lH0Dgq7bsCBpO3hGfr_mT5VfNk/view), [cve_loc](https://drive.google.com/file/d/1VDHjmDqURqkLc5MQb84MKhI9NlVrg3m3/view), [tipo_violencia](https://drive.google.com/file/d/1Vf4-VABIMH5LIK308ubKLfjfFKwfnhOk/view), [modalidad_violencia](https://drive.google.com/file/d/1_Oq77ueBKXeV6e9848S7Y4i-Xa2WGUG-/view), [efectos_fisicos](https://drive.google.com/file/d/1TFijVmvWgGEAzjrAE1WVW-WQveIg-UXy/view), [consecuencias_sexuales](https://drive.google.com/file/d/1qG8ny2idjFkaGjYP_piFKFb30Dgg4jhR/view), [efectos_psicologicos](https://drive.google.com/file/d/1F-R-n6QdIxWPczrw_ZFcSvzQzu_FPXY0/view), [efectos_economicos_y_patrimoniales](https://drive.google.com/file/d/1o9U4ySp0xzFkItncnuLDeqliUOhHIB1L/view), [agente_de_lesion](https://drive.google.com/file/d/1RjXu3Ea0osZeesVqZPWAQXP3iVVIY2Kh/view) y [area_anatomica_lesionada](https://drive.google.com/file/d/1R0fblWPIU9-bc74GapgzEnBwFjP4T11p/view).
#### Body raw (json)
```json
{
    "id" : 1,
    "fecha_hechos" : "2022-10-24",
    "hora_hechos"  : "10:20",
    "descripcion_hechos" : "hechos",
    "lugar_id" : 2,
    "lugar_detalle_id" : "19",
    "en_domicilio_victima" : true,
    "pais_id" : 153,
    "cve_ent" : 30,
    "cve_mun" : 2,
    "calle" : "sdsd",
    "num_exterior_km" : "0",
    "num_interior" : "interior",
    "cve_loc" : 2,
    "cp" : "55120",
    "lat" : 0.253,
    "lng" : 0.9326,
    "es_festivo" : true,
    "conoce_la_autoridad" : true,
    "conoce_la_autoridad_detalle" : "al juez",
    "tipo_violencia" : [1],
    "modalidad_violencia" : 2,
    "es_victima_de_delincuencia_organizada" : true,
    "hay_denuncia" : true,
    "efectos_fisicos" : [1,3],
    "consecuencias_sexuales" : [1],
    "efectos_psicologicos" : [1],
    "efectos_economicos_y_patrimoniales" : [2,1],
    "agente_de_lesion" : [2,3],
    "area_anatomica_lesionada": [2,1],
    "es_relacionada_con_orientacion_o_identidad" : true,
    "colonias_id" : 19727,
    "culmino_en_muerte" : true,
    "tipo_muerte_id" : 1,
    "tipo_muerte_otro" : "otra muerte",
    "tipo_conducta_violencia_sexual_id" : 1,
    "tipo_violencia_sexual" : "q",
    "tipo_agresor_sexual_id" : 2,
    "tipo_agresor_sexual_otro"            : "Otro agresor",
    "lugar_detalle_otro"                  : "lugar_detalle_otro",
    "tipo_violencia_otro"                 : "tipo_violencia_otro",
    "efecto_fisico_otro"                  : "efecto_fisico_otro",
    "consecuencia_sexual_otro"            : "consecuencia_sexual_otro" ,          
    "efecto_psicologico_otro"             : "efecto_psicologico_otro",           
    "efecto_economico_patrimonial_otro"   : "efecto_economico_patrimonial_otro",           
    "agente_lesion_otro"                  : "agente_lesion_otro",
    "area_anatomica_lesionada_otro"       : "area_anatomica_lesionada_otro" 
}
```
#### Campos Obligatorios

|                    Campo                   | Obligatorio |Tipo de dato |
|:------------------------------------------:|:-----------:|:-----------:|
| id                                         |     SI      |   integer   | 
| fecha_hechos                               |     NO      |    date     | 
| hora_hechos                                |     NO      |    time     |
| descripcion_hechos                         |     NO      |   string    | 
| lugar_id                                   |     NO      |   integer   | 
| lugar_detalle_id                           |     NO      |   integer   | 
| en_domicilio_victima                       |     NO      |   boolean   | 
| pais_id                                    |     NO      |   integer   | 
| cve_ent                                    |     NO      |   integer   | 
| cve_mun                                    |     NO      |   integer   | 
| calle                                      |     NO      |   string    | 
| num_exterior_km                            |     NO      |   string    | 
| num_interior                               |     NO      |   string    | 
| cve_loc                                    |     NO      |   integer   | 
| cp                                         |     NO      |   string    | 
| lat                                        |     NO      |   number    | 
| lng                                        |     NO      |   number    | 
| es_festivo                                 |     NO      |   boolean   | 
| conoce_la_autoridad                        |     NO      |   boolean   |
| conoce_la_autoridad_detalle                |     NO      |   string    |
| tipo_violencia                             |     NO      |  integer[]  | 
| modalidad_violencia                        |     NO      |   integer   | 
| es_victima_de_delincuencia_organizada      |     NO      |   boolean   | 
| hay_denuncia                               |     NO      |   boolean   | 
| efectos_fisicos                            |     NO      |  integer[]  | 
| consecuencias_sexuales                     |     NO      |  integer[]  | 
| efectos_psicologicos                       |     NO      |  integer[]  | 
| efectos_economicos_y_patrimoniales         |     NO      |  integer[]  | 
| agente_de_lesion                           |     NO      |  integer[]  | 
| area_anatomica_lesionada                   |     NO      |  integer[]  | 
| es_relacionada_con_orientacion_o_identidad |     NO      |   boolean   | 
| colonias_id                                |     NO      |   integer   | 
| culmino_en_muerte                          |     NO      |   boolean   | 
| tipo_muerte_id                             |     NO      |   integer   | 
| tipo_muerte_otro                           |     NO      |   string    | 
| tipo_conducta_violencia_sexual_id          |     NO      |   integer   | 
| tipo_violencia_sexual                      |     NO      |   string    | 
| tipo_agresor_sexual_id                     |     NO      |   integer   | 
| tipo_agresor_sexual_otro                   |     NO      |   string    |
| lugar_detalle_otro                         |     NO      |   string    |  
| tipo_violencia_otro                        |     NO      |   string    | 
| efecto_fisico_otro                         |     NO      |   string    | 
| consecuencia_sexual_otro                   |     NO      |   string    | 
| efecto_psicologico_otro                    |     NO      |   string    | 
| efecto_economico_patrimonial_otro          |     NO      |   string    | 
| agente_lesion_otro                         |     NO      |   string    | 
| area_anatomica_lesionada_otro              |     NO      |   string    | 
| | |
Las opciones del campo tipo_violencia_sexual de tipo string son: Acoso/Hostigamiento  

#### Ejemplo de solicitud.
con curl.
```bash
curl --location 'API_URL/api/edita-hecho-de-violencia' \
--header 'Accept: application/json' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer TOKEN' \
--data '{
    "id" : 1,
    "fecha_hechos" : "2022-10-24",
    "hora_hechos"  : "10:20",
    "descripcion_hechos" : "hechos",
    "lugar_id" : 2,
    "lugar_detalle_id" : "19",
    "en_domicilio_victima" : true,
    "pais_id" : 153,
    "cve_ent" : 30,
    "cve_mun" : 2,
    "calle" : "sdsd",
    "num_exterior_km" : "0",
    "num_interior" : "interior",
    "cve_loc" : 2,
    "cp" : "55120",
    "lat" : 0.253,
    "lng" : 0.9326,
    "es_festivo" : true,
    "conoce_la_autoridad" : true,
    "conoce_la_autoridad_detalle" : "al juez",
    "tipo_violencia" : [1],
    "modalidad_violencia" : 2,
    "es_victima_de_delincuencia_organizada" : true,
    "hay_denuncia" : true,
    "efectos_fisicos" : [1,3],
    "consecuencias_sexuales" : [1],
    "efectos_psicologicos" : [1],
    "efectos_economicos_y_patrimoniales" : [2,1],
    "agente_de_lesion" : [2,3],
    "area_anatomica_lesionada": [2,1],
    "es_relacionada_con_orientacion_o_identidad" : true,
    "colonias_id" : 19727,
    "culmino_en_muerte" : true,
    "tipo_muerte_id" : 1,
    "tipo_muerte_otro" : "otra muerte",
    "tipo_violencia_sexual" : "Hostigamiento",
    "tipo_agresor_sexual_id" : 2,
    "tipo_agresor_sexual_otro"            : "Otro agresor",
    "lugar_detalle_otro"                  : "lugar_detalle_otro",
    "tipo_violencia_otro"                 : "tipo_violencia_otro",
    "efecto_fisico_otro"                  : "efecto_fisico_otro",
    "consecuencia_sexual_otro"            : "consecuencia_sexual_otro" ,          
    "efecto_psicologico_otro"             : "efecto_psicologico_otro",           
    "efecto_economico_patrimonial_otro"   : "efecto_economico_patrimonial_otro",           
    "agente_lesion_otro"                  : "agente_lesion_otro",
    "area_anatomica_lesionada_otro"       : "area_anatomica_lesionada_otro" 
}
'
```
con PHP - cURL.
```php
<?php

$curl = curl_init();

curl_setopt_array($curl, array(
  CURLOPT_URL => 'API_URL/api/edita-hecho-de-violencia',
  CURLOPT_RETURNTRANSFER => true,
  CURLOPT_ENCODING => '',
  CURLOPT_MAXREDIRS => 10,
  CURLOPT_TIMEOUT => 0,
  CURLOPT_FOLLOWLOCATION => true,
  CURLOPT_HTTP_VERSION => CURL_HTTP_VERSION_1_1,
  CURLOPT_CUSTOMREQUEST => 'POST',
  CURLOPT_POSTFIELDS =>'{
    "id" : 1,
    "fecha_hechos" : "2022-10-24",
    "hora_hechos"  : "10:20",
    "descripcion_hechos" : "hechos",
    "lugar_id" : 2,
    "lugar_detalle_id" : "19",
    "en_domicilio_victima" : true,
    "pais_id" : 153,
    "cve_ent" : 30,
    "cve_mun" : 2,
    "calle" : "sdsd",
    "num_exterior_km" : "0",
    "num_interior" : "interior",
    "cve_loc" : 2,
    "cp" : "55120",
    "lat" : 0.253,
    "lng" : 0.9326,
    "es_festivo" : true,
    "conoce_la_autoridad" : true,
    "conoce_la_autoridad_detalle" : "al juez",
    "tipo_violencia" : [1],
    "modalidad_violencia" : 2,
    "es_victima_de_delincuencia_organizada" : true,
    "hay_denuncia" : true,
    "efectos_fisicos" : [1,3],
    "consecuencias_sexuales" : [1],
    "efectos_psicologicos" : [1],
    "efectos_economicos_y_patrimoniales" : [2,1],
    "agente_de_lesion" : [2,3],
    "area_anatomica_lesionada": [2,1],
    "es_relacionada_con_orientacion_o_identidad" : true,
    "colonias_id" : 19727,
    "culmino_en_muerte" : true,
    "tipo_muerte_id" : 1,
    "tipo_muerte_otro" : "otra muerte",
    "tipo_violencia_sexual" : "Hostigamiento",
    "tipo_agresor_sexual_id" : 2,
    "tipo_agresor_sexual_otro"            : "Otro agresor",
    "lugar_detalle_otro"                  : "lugar_detalle_otro",
    "tipo_violencia_otro"                 : "tipo_violencia_otro",
    "efecto_fisico_otro"                  : "efecto_fisico_otro",
    "consecuencia_sexual_otro"            : "consecuencia_sexual_otro" ,          
    "efecto_psicologico_otro"             : "efecto_psicologico_otro",           
    "efecto_economico_patrimonial_otro"   : "efecto_economico_patrimonial_otro",           
    "agente_lesion_otro"                  : "agente_lesion_otro",
    "area_anatomica_lesionada_otro"       : "area_anatomica_lesionada_otro" 
}
',
  CURLOPT_HTTPHEADER => array(
    'Accept: application/json',
    'Content-Type: application/json',
    'Authorization: Bearer TOKEN'
  ),
));

$response = curl_exec($curl);

curl_close($curl);
echo $response;

```
con JavaScrip - fetch.
```javascript
    const myHeaders = new Headers();
    myHeaders.append("Accept", "application/json");
    myHeaders.append("Content-Type", "application/json");
    myHeaders.append("Authorization", "Bearer TOKEN");

    const raw = JSON.stringify({
    "id": 1,
    "fecha_hechos": "2022-10-24",
    "hora_hechos": "10:20",
    "descripcion_hechos": "hechos",
    "lugar_id": 2,
    "lugar_detalle_id": "19",
    "en_domicilio_victima": true,
    "pais_id": 153,
    "cve_ent": 30,
    "cve_mun": 2,
    "calle": "sdsd",
    "num_exterior_km": "0",
    "num_interior": "interior",
    "cve_loc": 2,
    "cp": "55120",
    "lat": 0.253,
    "lng": 0.9326,
    "es_festivo": true,
    "conoce_la_autoridad": true,
    "conoce_la_autoridad_detalle": "al juez",
    "tipo_violencia": [
        1
    ],
    "modalidad_violencia": 2,
    "es_victima_de_delincuencia_organizada": true,
    "hay_denuncia": true,
    "efectos_fisicos": [
        1,
        3
    ],
    "consecuencias_sexuales": [
        1
    ],
    "efectos_psicologicos": [
        1
    ],
    "efectos_economicos_y_patrimoniales": [
        2,
        1
    ],
    "agente_de_lesion": [
        2,
        3
    ],
    "area_anatomica_lesionada": [
        2,
        1
    ],
    "es_relacionada_con_orientacion_o_identidad": true,
    "colonias_id": 19727,
    "culmino_en_muerte": true,
    "tipo_muerte_id": 1,
    "tipo_muerte_otro": "otra muerte",
    "tipo_violencia_sexual": "Hostigamiento",
    "tipo_agresor_sexual_id": 2,
    "tipo_agresor_sexual_otro": "Otro agresor",
    "lugar_detalle_otro": "lugar_detalle_otro",
    "tipo_violencia_otro": "tipo_violencia_otro",
    "efecto_fisico_otro": "efecto_fisico_otro",
    "consecuencia_sexual_otro": "consecuencia_sexual_otro",
    "efecto_psicologico_otro": "efecto_psicologico_otro",
    "efecto_economico_patrimonial_otro": "efecto_economico_patrimonial_otro",
    "agente_lesion_otro": "agente_lesion_otro",
    "area_anatomica_lesionada_otro": "area_anatomica_lesionada_otro"
    });

    const requestOptions = {
    method: "POST",
    headers: myHeaders,
    body: raw,
    redirect: "follow"
    };

    fetch("API_URL/api/edita-hecho-de-violencia", requestOptions)
    .then((response) => response.text())
    .then((result) => console.log(result))
    .catch((error) => console.error(error));
```
#### La respuesta que se recibirá después de la petición.
con estatus 200  en formato json.
```json
{
    "id": 1,
    "victimas_id": 22,
    "users_id": 1,
    "fecha_hechos": "2022-10-24",
    "hora_hechos": "10:20",
    "descripcion_hechos": "hechos",
    "lugar_id": 2,
    "lugar_detalle_id": "19",
    "en_domicilio_victima": true,
    "pais_id": 153,
    "cve_ent": 30,
    "cve_mun": 2,
    "calle": "sdsd",
    "num_exterior_km": "0",
    "num_interior": "interior",
    "cve_loc": 2,
    "cp": "55120",
    "lat": 0.253,
    "lng": 0.9326,
    "es_festivo": true,
    "conoce_la_autoridad": true,
    "conoce_la_autoridad_detalle": "al juez",
    "tipo_violencia": [
        1
    ],
    "modalidad_violencia": 2,
    "es_victima_de_delincuencia_organizada": true,
    "hay_denuncia": true,
    "efectos_fisicos": [
        1,
        3
    ],
    "consecuencias_sexuales": [
        1
    ],
    "efectos_psicologicos": [
        1
    ],
    "efectos_economicos_y_patrimoniales": [
        2,
        1
    ],
    "agente_de_lesion": [
        2,
        3
    ],
    "area_anatomica_lesionada": [
        2,
        1
    ],
    "es_relacionada_con_orientacion_o_identidad": true,
    "created_at": "2024-08-08T22:55:02.000000Z",
    "updated_at": "2024-11-15T01:06:27.000000Z",
    "deleted_at": null,
    "folio": "e293236c-6755-3987-8573-d8f70b1095d0-1",
    "colonias_id": 19727,
    "culmino_en_muerte": true,
    "tipo_muerte_id": 1,
    "tipo_muerte_otro": "otra muerte",
    "tipo_violencia_sexual": "Hostigamiento",
    "tipo_agresor_sexual_id": 2,
    "tipo_agresor_sexual_otro": "Otro agresor",
    "tipos_conductas_violencia_sexual": null,
    "lugar_detalle_otro": "lugar_detalle_otro",
    "tipo_violencia_otro": "tipo_violencia_otro",
    "efecto_fisico_otro": "efecto_fisico_otro",
    "consecuencia_sexual_otro": "consecuencia_sexual_otro",
    "efecto_psicologico_otro": "efecto_psicologico_otro",
    "efecto_economico_patrimonial_otro": "efecto_economico_patrimonial_otro",
    "agente_lesion_otro": "agente_lesion_otro",
    "area_anatomica_lesionada_otro": "area_anatomica_lesionada_otro",
    "victima": {
        "id": 22,
        "nombre": "Garland Walsh",
        "primer_apellido": "Schimmel",
        "segundo_apellido": "x",
        "fecha_nacimiento": "2005-03-01",
        "cve_ent": 21,
        "created_at": "2023-10-31T19:50:17.000000Z",
        "updated_at": "2023-10-31T19:50:17.000000Z",
        "curp": "false",
        "folio_euv": "e293236c-6755-3987-8573-d8f70b1095d0",
        "sexo": "M",
        "extranjera": true,
        "users_id": 931,
        "instancias_id": null,
        "areas_id": null,
        "sedes_id": null,
        "nacionalidad_id": 88,
        "deleted_at": null,
        "sexo_id": null,
        "from_api": false,
        "identifica_mujer": null
    },
    "lugar": {
        "id": 2,
        "pk_catalogo": 4,
        "descripcion": "Transporte privado",
        "orden": 4,
        "es_activo": true,
        "created_at": "2023-12-01T17:04:30.000000Z",
        "updated_at": "2023-12-01T17:04:30.000000Z",
        "detalles_localidad": [
            {
                "id": 11,
                "pk_catalogo": 26,
                "descripcion": "Empresa",
                "fk_lugar_ocurrencia": 4,
                "orden": 1,
                "es_activo": true,
                "created_at": "2023-12-01T17:04:30.000000Z",
                "updated_at": "2023-12-01T17:04:30.000000Z"
            },
            {
                "id": 10,
                "pk_catalogo": 27,
                "descripcion": "Particular",
                "fk_lugar_ocurrencia": 4,
                "orden": 2,
                "es_activo": true,
                "created_at": "2023-12-01T17:04:30.000000Z",
                "updated_at": "2023-12-01T17:04:30.000000Z"
            },
            {
                "id": 9,
                "pk_catalogo": 28,
                "descripcion": "Otro",
                "fk_lugar_ocurrencia": 4,
                "orden": 3,
                "es_activo": true,
                "created_at": "2023-12-01T17:04:30.000000Z",
                "updated_at": "2023-12-01T17:04:30.000000Z"
            }
        ]
    },
    "lugar_detalle": {
        "id": 19,
        "pk_catalogo": 18,
        "descripcion": "Otro",
        "fk_lugar_ocurrencia": 2,
        "orden": 15,
        "es_activo": true,
        "created_at": "2023-12-01T17:04:30.000000Z",
        "updated_at": "2023-12-01T17:04:30.000000Z"
    },
    "seguimientos": [
        {
            "id": 20,
            "users_id": 1,
            "hechos_id": 1,
            "fecha_seguimiento": "2023-10-27",
            "hora_seguimiento": "08:12:00",
            "tipo_id": 1,
            "servicios_id": 1,
            "acciones": "Se realizo la accion de prueba",
            "observaciones": "Se realizo la observacion de prueba",
            "created_at": "2024-09-06T02:42:51.000000Z",
            "updated_at": "2024-09-06T02:42:51.000000Z",
            "deleted_at": null,
            "servicio": {
                "id": 1,
                "tipo_servicio_id": 1,
                "descripcion": "INTERVENCIÓN EN CRISIS",
                "esactivo": true,
                "created_at": "2023-12-01T17:04:35.000000Z",
                "updated_at": "2023-12-01T17:04:35.000000Z"
            },
            "tipo_servicio": {
                "id": 1,
                "descripcion": "Servicios de Psicología",
                "esactivo": true,
                "created_at": "2023-12-01T17:04:35.000000Z",
                "updated_at": "2023-12-01T17:04:35.000000Z"
            }
        },
        {
            "id": 341,
            "users_id": 1,
            "hechos_id": 1,
            "fecha_seguimiento": "2023-10-27",
            "hora_seguimiento": "08:12:00",
            "tipo_id": 1,
            "servicios_id": 1,
            "acciones": "Se realizo la accion de prueba",
            "observaciones": "Se realizo la observacion de prueba",
            "created_at": "2024-11-14T22:39:36.000000Z",
            "updated_at": "2024-11-14T22:39:36.000000Z",
            "deleted_at": null,
            "servicio": {
                "id": 1,
                "tipo_servicio_id": 1,
                "descripcion": "INTERVENCIÓN EN CRISIS",
                "esactivo": true,
                "created_at": "2023-12-01T17:04:35.000000Z",
                "updated_at": "2023-12-01T17:04:35.000000Z"
            },
            "tipo_servicio": {
                "id": 1,
                "descripcion": "Servicios de Psicología",
                "esactivo": true,
                "created_at": "2023-12-01T17:04:35.000000Z",
                "updated_at": "2023-12-01T17:04:35.000000Z"
            }
        }
    ],
    "canalizaciones": [],
    "ordenes": [],
    "victimas_trata": [],
    "mujeres_prision": [
        {
            "id": 13,
            "privada_de_libertad": true,
            "calidad_legal_id": 1,
            "delitos": "robo",
            "victima_de_tortura": true,
            "protocolo_de_estambul": true,
            "victima_de_tortura_despues_del_protocolo_de_estambul": true,
            "tortura_tipo_id": 1,
            "tortura_momento_id": 5,
            "autoridad_id": 1,
            "created_at": "2024-10-25T20:28:54.000000Z",
            "updated_at": "2024-10-25T20:28:54.000000Z",
            "users_id": 1,
            "hechos_id": 1,
            "deleted_at": null,
            "calidad_legal": {
                "id": 1,
                "descripcion": "Procesada",
                "es_activo": true,
                "created_at": "2023-12-01T17:04:37.000000Z",
                "updated_at": "2023-12-01T17:04:37.000000Z"
            }
        },
        {
            "id": 14,
            "privada_de_libertad": true,
            "calidad_legal_id": 1,
            "delitos": "robo",
            "victima_de_tortura": true,
            "protocolo_de_estambul": true,
            "victima_de_tortura_despues_del_protocolo_de_estambul": true,
            "tortura_tipo_id": 1,
            "tortura_momento_id": 5,
            "autoridad_id": 1,
            "created_at": "2024-10-25T20:43:25.000000Z",
            "updated_at": "2024-10-25T20:43:25.000000Z",
            "users_id": 1,
            "hechos_id": 1,
            "deleted_at": null,
            "calidad_legal": {
                "id": 1,
                "descripcion": "Procesada",
                "es_activo": true,
                "created_at": "2023-12-01T17:04:37.000000Z",
                "updated_at": "2023-12-01T17:04:37.000000Z"
            }
        },
        {
            "id": 16,
            "privada_de_libertad": true,
            "calidad_legal_id": 1,
            "delitos": "robo",
            "victima_de_tortura": true,
            "protocolo_de_estambul": true,
            "victima_de_tortura_despues_del_protocolo_de_estambul": true,
            "tortura_tipo_id": 1,
            "tortura_momento_id": 5,
            "autoridad_id": 1,
            "created_at": "2024-10-28T21:59:09.000000Z",
            "updated_at": "2024-10-28T21:59:09.000000Z",
            "users_id": 1,
            "hechos_id": 1,
            "deleted_at": null,
            "calidad_legal": {
                "id": 1,
                "descripcion": "Procesada",
                "es_activo": true,
                "created_at": "2023-12-01T17:04:37.000000Z",
                "updated_at": "2023-12-01T17:04:37.000000Z"
            }
        }
    ],
    "colonia": {
        "id": 19727,
        "cve_ent": 15,
        "cve_mun": 33,
        "cp": 55120,
        "descripcion": "CIUDAD AZTECA SECCIÓN ORIENTE",
        "created_at": "2024-08-08T22:49:37.000000Z",
        "updated_at": "2024-08-08T22:49:37.000000Z"
    }
}
```
con estatus 422. Error de tipo de dato.
```json
{
    "message": "El campo NOMBRE_CAMPO debe ser una TIPO_DATO.",
    "errors": {
        "otra_discapacidad": [
            "El campo NOMBRE_CAMPO debe ser una TIPO_DATO."
        ]
    }
}
con estatus 422. .
```json
```
-----------------------------------------------------------------
### Editar registro de mujeres en prisión para un hecho de violencia
-----------------------------------------------------------------
El registro requiere que exista un hecho de violencia, teniendo una dependencia funcional directa.
#### POST
```
    API_URL/api/edita-mujer-en-prision
```
catálogos: calidad_legal_id, tortura_tipo_id, tortura_momento_id, autoridad_id
#### Body raw (json)
```json
    {
        "id"                                                  : 1,
        "privada_de_libertad"                                 : true,
        "calidad_legal_id"                                    : 2,
        "delitos"                                             : "listo",
        "victima_de_tortura"                                  : true,
        "protocolo_de_estambul"                               : true,
        "victima_de_tortura_despues_del_protocolo_de_estambul": true,
        "tortura_tipo_id"                                     : 2,
        "tortura_momento_id"                                  : 1,
        "autoridad_id"                                        : 7
    }
```
#### Campos Obligatorios

|                           Campo                       | Obligatorio | Tipo de dato |
|:-----------------------------------------------------:|:-----------:|:------------:|
| id                                                    |     SI      |    integer   |
| privada_de_libertad                                   |     NO      |    boolean   | 
| calidad_legal_id                                      |     NO      |    integer   | 
| delitos                                               |     NO      |    string    | 
| victima_de_tortura                                    |     NO      |    boolean   | 
| protocolo_de_estambul                                 |     NO      |    boolean   | 
| victima_de_tortura_despues_del_protocolo_de_estambul  |     NO      |    boolean   | 
| tortura_tipo_id                                       |     NO      |    integer   | 
| tortura_momento_id                                    |     NO      |    integer   | 
| autoridad_id                                          |     NO      |    integer   | 
|||| 

#### Ejemplo de solicitud.
con curl.
```bash
    curl --location 'API_URL/api/edita-mujer-en-prision' \
    --header 'Accept: application/json' \
    --header 'Content-Type: application/json' \
    --header 'Authorization: Bearer TOKEN' \
    --data '{
        "id" : 1,
        "privada_de_libertad": true,
        "calidad_legal_id" : 2,
        "delitos" : "listo",
        "victima_de_tortura": true,
        "protocolo_de_estambul": true,
        "victima_de_tortura_despues_del_protocolo_de_estambul": true,
        "tortura_tipo_id": 2,
        "tortura_momento_id": 1,
        "autoridad_id": 7
    }'
```
con PHP - cURL.
```php
    <?php

    $curl = curl_init();

    curl_setopt_array($curl, array(
    CURLOPT_URL => 'API_URL/api/edita-mujer-en-prision',
    CURLOPT_RETURNTRANSFER => true,
    CURLOPT_ENCODING => '',
    CURLOPT_MAXREDIRS => 10,
    CURLOPT_TIMEOUT => 0,
    CURLOPT_FOLLOWLOCATION => true,
    CURLOPT_HTTP_VERSION => CURL_HTTP_VERSION_1_1,
    CURLOPT_CUSTOMREQUEST => 'POST',
    CURLOPT_POSTFIELDS =>'{
        "id" : 1,
        "privada_de_libertad": true,
        "calidad_legal_id" : 2,
        "delitos" : "listo",
        "victima_de_tortura": true,
        "protocolo_de_estambul": true,
        "victima_de_tortura_despues_del_protocolo_de_estambul": true,
        "tortura_tipo_id": 2,
        "tortura_momento_id": 1,
        "autoridad_id": 7
    }',
    CURLOPT_HTTPHEADER => array(
        'Accept: application/json',
        'Content-Type: application/json',
        'Authorization: Bearer TOKEN'
    ),
    ));

    $response = curl_exec($curl);

    curl_close($curl);
    echo $response;
```
con JavaScrip - fetch.
```javascript
    const myHeaders = new Headers();
    myHeaders.append("Accept", "application/json");
    myHeaders.append("Content-Type", "application/json");
    myHeaders.append("Authorization", "Bearer TOKEN");

    const raw = JSON.stringify({
    "id": 1,
    "privada_de_libertad": true,
    "calidad_legal_id": 2,
    "delitos": "listo",
    "victima_de_tortura": true,
    "protocolo_de_estambul": true,
    "victima_de_tortura_despues_del_protocolo_de_estambul": true,
    "tortura_tipo_id": 2,
    "tortura_momento_id": 1,
    "autoridad_id": 7
    });

    const requestOptions = {
    method: "POST",
    headers: myHeaders,
    body: raw,
    redirect: "follow"
    };

    fetch("API_URL/api/edita-mujer-en-prision", requestOptions)
    .then((response) => response.text())
    .then((result) => console.log(result))
    .catch((error) => console.error(error));
```
#### La respuesta que se recibirá después de la petición.
con estatus 200  en formato json.
```json
    {
        "id": 1,
        "privada_de_libertad": true,
        "calidad_legal_id": 2,
        "delitos": "listo",
        "victima_de_tortura": true,
        "protocolo_de_estambul": true,
        "victima_de_tortura_despues_del_protocolo_de_estambul": true,
        "tortura_tipo_id": 2,
        "tortura_momento_id": 1,
        "autoridad_id": 7,
        "created_at": "2024-09-04T23:58:16.000000Z",
        "updated_at": "2024-10-25T20:20:33.000000Z",
        "users_id": 1098,
        "hechos_id": 49,
        "deleted_at": null,
        "calidad_legal": {
            "id": 2,
            "descripcion": "Sentenciada",
            "es_activo": true,
            "created_at": "2023-12-01T17:04:37.000000Z",
            "updated_at": "2023-12-01T17:04:37.000000Z"
        }
    }
```
con estatus 422. El id es necesario y debe ser enviado
```json
    {
        "message": "El campo id es requerido.",
        "errors": {
            "id": [
                "El campo id es requerido."
            ]
        }
    }
```
-----------------------------------------------------------------
### Editar registro de mujeres víctimas de trata
-----------------------------------------------------------------

-----------------------------------------------------------------
### Infoname
-----------------------------------------------------------------
Método de personalizado para obtener los datos de niñas adolecentes de madres embarazadas menores de 15 años (name).
#### POST
```
    API_URL/api/custom/infoname
```
#### Body raw (json)
```json
    {
        "curp" : "CURP000000XXXXXXX0"
    }
```
#### Campos Obligatorios

|   Campo  | Obligatorio | Tipo de dato |
|:--------:|:-----------:|:------------:|
|  curp    |      SI     |    string    |

#### Ejemplo de solicitud.
con curl.
```bash
    curl --location 'API_URL/api/custom/infoname' \
    --header 'Accept: application/json' \
    --header 'Content-Type: application/json' \
    --header 'Authorization: Bearer TOKEN' \
    --data '{
        "curp" : "CURP000000XXXXXXX0"
    }'
```
con PHP - cURL.
```php
    <?php

    $curl = curl_init();

    curl_setopt_array($curl, array(
    CURLOPT_URL => 'API_URL/api/custom/infoname',
    CURLOPT_RETURNTRANSFER => true,
    CURLOPT_ENCODING => '',
    CURLOPT_MAXREDIRS => 10,
    CURLOPT_TIMEOUT => 0,
    CURLOPT_FOLLOWLOCATION => true,
    CURLOPT_HTTP_VERSION => CURL_HTTP_VERSION_1_1,
    CURLOPT_CUSTOMREQUEST => 'POST',
    CURLOPT_POSTFIELDS =>'{
        "curp" : "CURP000000XXXXXXX0"
    }',
    CURLOPT_HTTPHEADER => array(
        'Accept: application/json',
        'Content-Type: application/json',
        'Authorization: Bearer TOKEN'
    ),
    ));

    $response = curl_exec($curl);

    curl_close($curl);
    echo $response;
```
con JavaScrip - fetch.
```javascript
    const myHeaders = new Headers();
    myHeaders.append("Accept", "application/json");
    myHeaders.append("Content-Type", "application/json");
    myHeaders.append("Authorization", "Bearer TOKEN");

    const raw = JSON.stringify({
    "curp": "CURP000000XXXXXXX0"
    });

    const requestOptions = {
    method: "POST",
    headers: myHeaders,
    body: raw,
    redirect: "follow"
    };

    fetch("API_URL/api/custom/infoname", requestOptions)
    .then((response) => response.text())
    .then((result) => console.log(result))
    .catch((error) => console.error(error));
```
#### La respuesta que se recibirá después de la petición.
con estatus 200  en formato json.
```json
{
    "datosGenerales": {
        "fechaDeAtencion": null,
        "fechaDeRegistro": null,
        "municipioDeLaAtencion": null,
        "entidadDeLaAtencion": null,
        "lugarDeLaAtencion": null,
        "noDeAtencion": null,
        "NivelDeRiesgo": null
    },
    "datosDeLaNAME": {
        "Nombre": {
            "PrimerApellido": "PrimerApellido",
            "SegundoApellido": "SegundoApellido",
            "Nombre": "Nombre"
        },
        "Edad": 22,
        "domicilio": {
            "calle": "calle",
            "numeroExterior": "numeroExterior",
            "numeroInterior": "numeroInterior",
            "cve_ent": 1,
            "cve_mun": 231,
            "cve_loc": 456,
            "colonia": null,
            "codigoPostal": null
        },
        "telefono": null,
        "conQuienVive": null,
        "curp": "CURP000000XXXXXXX0",
        "entidadDeNacimiento": 11,
        "discapacidad": null,
        "nacionalidad": null,
        "puebloIndigena": null,
        "redDeApoyo": null,
        "escolaridad": null,
        "sigueEstudiando": null,
        "hijos": null,
        "nombreDelPadre": null,
        "edadDelPadre": null,
        "viveConElPadre": null,
        "domicilioDelPadre": null,
        "seguroSocial": null,
        "embarazada": null,
        "datosGeneralesEmbarazo": {
            "numeroDeSemana": null
        }
    },
    "datosDeQuienCaptura": {
        "nombre": null,
        "profesion": null,
        "email": null,
        "telefono": null,
        "cve_ent": null,
        "cve_mun": null,
        "enlaceDelGrupoRector": null,
        "dependencia": {
            "pk_dependencia": null,
            "descripcion": null,
            "siglas": null,
            "fk_area_dependencia": null,
            "fk_estado": null,
            "es_gubernamental": null,
            "clave": null,
            "fk_municipio": null
        }
    },
    "seguimiento": {
        "Seguimientos": null,
        "Canalizaciones": null,
        "OrdenesDeProteccion": null
    },
    "comentarionGenerales": null
}
```
con estatus 422. El curp es necesario ser enviado.
```json
    {
        "message": "El campo curp es requerido.",
        "errors": {
            "curp": [
                "El campo curp es requerido."
            ]
        }
    }
```
-----------------------------------------------------------------
### Eliminar CURP mediante CURP
-----------------------------------------------------------------
El consumo de este método requiere del curp de la víctima. Este endpoint estará disponible unicamente para las puebas en el desarrollo de sus soluciones para el consumo de la Api.
#### POST
```
    API_URL/api/delete-curp
```
#### Body raw (json)
```json
{
    "curp" : "CURP000000XXXXXXX0"
}
```
#### Campos Obligatorios

|   Campo  | Obligatorio | Tipo de dato |   
|:--------:|:-----------:|:------------:|
|  curp    |      SI     |    string    |

#### Ejemplo de solicitud.
con curl.
```bash
    curl --location 'API_URL/api/delete-curp' \
    --header 'Accept: application/json' \
    --header 'Content-Type: application/json' \
    --header 'Authorization: Bearer TOKEN' \
    --data '{
            "curp" : "CURP000000XXXXXXX0"
    }'
```
con PHP - cURL.
```php
    $curl = curl_init();

    curl_setopt_array($curl, array(
    CURLOPT_URL => 'API_URL/api/delete-curp',
    CURLOPT_RETURNTRANSFER => true,
    CURLOPT_ENCODING => '',
    CURLOPT_MAXREDIRS => 10,
    CURLOPT_TIMEOUT => 0,
    CURLOPT_FOLLOWLOCATION => true,
    CURLOPT_HTTP_VERSION => CURL_HTTP_VERSION_1_1,
    CURLOPT_CUSTOMREQUEST => 'POST',
    CURLOPT_POSTFIELDS =>'{
        "curp" : "CURP000000XXXXXXX0"
    }',
    CURLOPT_HTTPHEADER => array(
        'Accept: application/json',
        'Content-Type: application/json',
        'Authorization: Bearer TOKEN'
    ),
    ));

    $response = curl_exec($curl);

    curl_close($curl);
    echo $response;
```
con JavaScrip - fetch.
```javascript
    const myHeaders = new Headers();
    myHeaders.append("Accept", "application/json");
    myHeaders.append("Content-Type", "application/json");
    myHeaders.append("Authorization", "Bearer TOKEN");

    const raw = JSON.stringify({
    "curp": "CURP000000XXXXXXX0"
    });

    const requestOptions = {
    method: "POST",
    headers: myHeaders,
    body: raw,
    redirect: "follow"
    };

    fetch("API_URL/api/delete-curp", requestOptions)
    .then((response) => response.text())
    .then((result) => console.log(result))
    .catch((error) => console.error(error));
```
#### La respuesta que se recibirá después de la petición.
con estatus 200  en formato json.
```json
    {
        "message": "CURP eliminado",
        "folio_euv": "74162382-c38a-3c6d-a995-e455294e1167"
    }
```
con estatus 422. Error con el curp.
```json
    {
        "message": "El campo curp seleccionado no es válido.",
        "errors": {
            "curp": [
                "El campo curp seleccionado no es válido."
            ]
        }
    }
```
con estatus 422. En caso de no recibir el dato.
```json
{
    "message": "El campo curp es requerido.",
    "errors": {
        "curp": [
            "El campo curp es requerido."
        ]
    }
}
```
-----------------------------------------------------------------
### Eliminar CURP mediante EUV
-----------------------------------------------------------------
El consumo de este método requiere del folio euv de la víctima. Este endpoint estará disponible unicamente para las puebas en el desarrollo de sus soluciones para el consumo de la Api.
#### POST
```
    API_URL/api/delete-curp-from-euv
```
#### Body raw (json)
```json
    {
        "folio_euv" : "EUV00000000000XX0X0XX0XX"
    }
```
#### Campos Obligatorios

|   Campo  | Obligatorio | Tipo de dato | 
|:--------:|:-----------:|:------------:|
|folio_euv |      SI     |    string    |

#### Ejemplo de solicitud.
con curl.
```bash
    curl --location 'API_URL/api/delete-curp-from-euv' \
    --header 'Accept: application/json' \
    --header 'Content-Type: application/json' \
    --header 'Authorization: Bearer TOKEN' \
    --data '{
        "folio_euv" : "EUV00000000000XX0X0XX0XX"
    }'
```
con PHP - cURL.
```php
    <?php

    $curl = curl_init();

    curl_setopt_array($curl, array(
    CURLOPT_URL => 'API_URL/api/delete-curp-from-euv',
    CURLOPT_RETURNTRANSFER => true,
    CURLOPT_ENCODING => '',
    CURLOPT_MAXREDIRS => 10,
    CURLOPT_TIMEOUT => 0,
    CURLOPT_FOLLOWLOCATION => true,
    CURLOPT_HTTP_VERSION => CURL_HTTP_VERSION_1_1,
    CURLOPT_CUSTOMREQUEST => 'POST',
    CURLOPT_POSTFIELDS =>'{
        "folio_euv" : "EUV00000000000XX0X0XX0XX"
    }',
    CURLOPT_HTTPHEADER => array(
        'Accept: application/json',
        'Content-Type: application/json',
        'Authorization: Bearer TOKEN'
    ),
    ));

    $response = curl_exec($curl);

    curl_close($curl);
    echo $response;
```
con JavaScrip - fetch.
```javascript
    const myHeaders = new Headers();
    myHeaders.append("Accept", "application/json");
    myHeaders.append("Content-Type", "application/json");
    myHeaders.append("Authorization", "Bearer TOKEN");

    const raw = JSON.stringify({
    "folio_euv": "EUV00000000000XX0X0XX0XX"
    });

    const requestOptions = {
    method: "POST",
    headers: myHeaders,
    body: raw,
    redirect: "follow"
    };

    fetch("API_URL/api/delete-curp-from-euv", requestOptions)
    .then((response) => response.text())
    .then((result) => console.log(result))
    .catch((error) => console.error(error));
```
#### La respuesta que se recibirá después de la petición.
con estatus 200  en formato json.
```json
    {
        "message": "CURP eliminado",
        "folio_euv": "EUV00000000000XX0X0XX0XX"
    }
```
con estatus 422. Error con el folio euv.
```json
    {
        "message": "El campo folio euv seleccionado no es válido.",
        "errors": {
            "folio_euv": [
                "El campo folio euv seleccionado no es válido."
            ]
        }
    }
```
con estatus 422. En caso de no recibir el dato.
```json
    {
        "message": "El campo folio euv es requerido.",
        "errors": {
            "folio_euv": [
                "El campo folio euv es requerido."
            ]
        }
    }
```
-----------------------------------------------------------------
### Restaurar CURP mediante EUV
-----------------------------------------------------------------
El consumo de este método requiere del curp y el folio euv de la víctima. Este endpoint estará disponible unicamente para las puebas en el desarrollo de sus soluciones para el consumo de la Api.
#### POST
```
    API_URL/api/restore-curp-from-euv
```
#### Body raw (json)
```json
    {
        "folio_euv" : "EUV00000000000XX0X0XX0XX",
        "curp" : "CURP000000XXXXXXX0"
    }
```
#### Campos Obligatorios

|   Campo   | Obligatorio | Tipo de dato |
|:---------:|:-----------:|:------------:|
| folio_euv |      SI     |    string    |
| curp      |      SI     |    string    | 

#### Ejemplo de solicitud.
con curl.
```bash
    curl --location 'API_URL/api/restore-curp-from-euv' \
    --header 'Accept: application/json' \
    --header 'Content-Type: application/json' \
    --header 'Authorization: Bearer TOKEN' \
    --data '{
        "folio_euv" : "EUV00000000000XX0X0XX0XX",
        "curp" : "CURP000000XXXXXXX0"
    }'
```
con PHP - cURL.
```php
    <?php

    $curl = curl_init();

    curl_setopt_array($curl, array(
    CURLOPT_URL => 'API_URL/api/restore-curp-from-euv',
    CURLOPT_RETURNTRANSFER => true,
    CURLOPT_ENCODING => '',
    CURLOPT_MAXREDIRS => 10,
    CURLOPT_TIMEOUT => 0,
    CURLOPT_FOLLOWLOCATION => true,
    CURLOPT_HTTP_VERSION => CURL_HTTP_VERSION_1_1,
    CURLOPT_CUSTOMREQUEST => 'POST',
    CURLOPT_POSTFIELDS =>'{
        "folio_euv" : "EUV00000000000XX0X0XX0XX",
        "curp" : "CURP000000XXXXXXX0"
    }',
    CURLOPT_HTTPHEADER => array(
        'Accept: application/json',
        'Content-Type: application/json',
        'Authorization: Bearer TOKEN'
    ),
    ));

    $response = curl_exec($curl);

    curl_close($curl);
    echo $response;
```
con JavaScrip - fetch.
```javascript
    const myHeaders = new Headers();
    myHeaders.append("Accept", "application/json");
    myHeaders.append("Content-Type", "application/json");
    myHeaders.append("Authorization", "Bearer TOKEN");

    const raw = JSON.stringify({
    "folio_euv": "EUV00000000000XX0X0XX0XX",
    "curp": "CURP000000XXXXXXX0"
    });

    const requestOptions = {
    method: "POST",
    headers: myHeaders,
    body: raw,
    redirect: "follow"
    };

    fetch("API_URL/api/restore-curp-from-euv", requestOptions)
    .then((response) => response.text())
    .then((result) => console.log(result))
    .catch((error) => console.error(error));
```
#### La respuesta que se recibirá después de la petición.
con estatus 200  en formato json.
```json
    {
        "message": "CURP restaurado",
        "folio_euv": "EUV000000000005302092024"
    }
```
con estatus 422. Error con el folio euv.
```json
    {
        "message": "El campo folio euv seleccionado no es válido.",
        "errors": {
            "folio_euv": [
                "El campo folio euv seleccionado no es válido."
            ]
        }
    }
```
con estatus 422. Error con el curp debido a que ya existe en la base de datos.
```json
{
    "message": "El campo curp ya ha sido tomado.",
    "errors": {
        "curp": [
            "El campo curp ya ha sido tomado."
        ]
    }
}
```
con estatus 422. Error con el curp debido a que ya existe en la base de datos y folio euv incorrecto.
```json
    {
        "message": "El campo folio euv seleccionado no es válido. (and 1 more error)",
        "errors": {
            "folio_euv": [
                "El campo folio euv seleccionado no es válido."
            ],
            "curp": [
                "El campo curp ya ha sido tomado."
            ]
        }
    }
```
con estatus 422. En caso de no recibir el datos.
```json
    {
        "message": "El campo folio euv es requerido. (and 1 more error)",
        "errors": {
            "folio_euv": [
                "El campo folio euv es requerido."
            ],
            "curp": [
                "El campo curp es requerido."
            ]
        }
    }
```