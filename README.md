# guia_api


# Guía de uso para el API  

## Índice

* [Cómo identificarse con el sistema](#cómo-identificarse-con-el-sistema)
    * [Cómo obtener un token](#1-cómo-obtener-un-token)
    * [Cómo usar el token](#2-cómo-usar-el-token)
* [Métodos de consulta](#métodos-de-consulta-disponibles)
    * [Buscar víctima por CURP](#buscar-víctima-por-curp)
    * [Datos de la víctima por hecho id](#datos-de-la-víctima-por-hecho-id)
    * [Datos de los agresores por hecho id](#datos-de-los-agresores-por-hecho-id)
    * [Seguimientos por folio EUV / id de Hecho de violencia](#seguimientos-por-folio-euv--id-de-hecho-de-violencia)
    * [Seguimientos por folio EUV](#seguimientos-por-folio-euv)
    * [Folios EUV por enlace estatal](#folios-euv-por-enlace-estatal)
    * [Folios EUV por estado](#folios-euv-por-estado)
    * [Hecho por id](#hecho-por-id)
    * [Hechos por entidad y rango de fecha](#hechos-de-violencia-por-entidad-y-rango-de-fecha)
* [Métodos de registro disponibles](#métodos-de-registro-disponibles)
    * [Registro de víctima con CURP](#registro-de-víctima-con-curp)
    * [Registro de víctima con Datos](#registro-de-víctima-con-datos)
    * [Registro de víctima sin CURP](#registro-de-víctima-sin-curp)
    * [Registro de hecho de violencia](#registro-de-hecho-de-violencia)
    * [Registro de mujeres en prisión para un hecho de violencia](#registro-de-mujeres-en-prisión-para-un-hecho-de-violencia)
    * [Registro de mujeres víctimas de trata para un hecho de violencia](#registro-de-mujeres-víctimas-de-trata-para-un-hecho-de-violencia)
    * [Registro de mujeres desaparecidas para un hecho de violencia](#registro-de-mujeres-desaparecidas-para-un-hecho-de-violencia)
    * [Registro una canalización para un hecho de violencia](#registro-una-canalización-para-un-hecho-de-violencia)
    * [Registro de un agresor para un hecho de violencia](#registro-de-un-agresor-para-un-hecho-de-violencia)
* [Métodos de edición disponibles](#métodos-de-edición-disponibles)
    * [Editar hecho de violencia](#editar-hecho-de-violencia)
    * [Editar registro de mujeres en prisión para un hecho de violencia](#editar-registro-de-mujeres-en-prisión-para-un-hecho-de-violencia)
    * [Editar registro de mujeres víctimas de trata](#editar-registro-de-mujeres-víctimas-de-trata)
* [Método de consulta personalizados](#métodos-de-consulta-personalizados-disponibles)
    * [Infoname](#infoname)
* [Métodos de prueba disponibles](#métodos-de-prueba)
    * [Eliminar CURP mediante CURP](#eliminar-curp-mediante-curp)
    * [Eliminar CURP mediante EUV](#eliminar-curp-mediante-folio-euv)
    * [Restaurar CURP mediante EUV](#restaura-el-curp-mediante-folio-euv)


## Cómo identificarse con el sistema

### 1. Cómo obtener un token
Para obtener un token, se debe hacer una petición a __/api/tokens/create__ enviando dos parámetros: email y password. Si las credenciales son correctas, se obtendrá un objeto json con la llave para realizar peticiones durante un día.

Con curl se puede realizar así: