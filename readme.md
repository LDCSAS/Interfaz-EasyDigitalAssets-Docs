# Interfaz Para Easy Digital Assets

Custom HTML Element de uso sencillo para poder acceder a los 
servicios de Easy Digital Assets. Desarrollado por Línea de Código SAS.

# Funcionamiento

La interfaz va a poder acceder a los servicios de Easy Digital Assets. 
Los cuales, variando por la acción a realizar, tienen el siguiente flujo de desarrollo.

1. Verifica que se tenga una acción valida.
2. Verifica que se tenga todos los parametros requeridos para dicha acción.
3. Genera un botón el cual activa un modal que permite conectarse a los servicos de EDA.
4. Dentro del Modal, se usan los servicios de EDA.
5. El usuario puede retornar los resultados del servicio para su procesamiento y uso. * Si se determina en los parametros
6. Se puede cerrar el Modal seguramente, cumpliendo su uso y funcionamiento.

# Uso

Para usar esta interfaz solo necesitamos incluir en nuestro html el siguiente archivo Javascript.
```html
<script src="https://lineadecodigo.net/cdn/eda/eda-interface.js"></script>
```

Una vez importado y incluido este script, podemos hacer uso dentro de nuestro html, cómo un elemento 
normal, de la Interfaz de Easy Digital Assets. Un Ejemplo básico de validar activos digitales.

```html
<eda-interface
  digital-asset-code="63fcecb5a7bd6165bbdd3cd0"
  asset-action="validate">
</eda-interface>
```

También si es necesario, se puede incluir creando el elemento mismo desde Javascript.
Solo va a iniciar su acción una vez se hayan definido todos los atributos requeridos.
```javascript
const edaInterface = document.createElement('eda-interface');
edaInterface.setAttribute('asset-action', 'validate');
edaInterface.setAttribute('digital-asset-code', '63fcecb5a7bd6165bbdd3cd0');
```

También apartir de JavaScript, se pueden cambiar los atributos de la Interfaz,
este verificara que no haya ningún error con cada cambio, 
si no hay errores recargara EDA con los cambios hechos.

```html
<eda-interface
  id="eda-interface"
  digital-asset-code="63fcecb5a7bd6165bbdd3cd0"
  asset-action="validate">
</eda-interface>
```
```javascript
const edaInterface = document.getElementById('eda-interface');
edaInterface.setAttribute('digital-asset-code', '{Nuevo Código activo digital}');
```
Una vez verificado que el Nuevo Código sea valido, se podra usar de nuevo la Interfaz de EDA,
en caso de que no lo sea se puede volver a cambiar el código del Activo Digital hasta que esté sea valido.

## Retorno de Datos

Despues de completar una acción, la Interfaz de EDA puede retornar los datos del activo digital.
Estos datos se pueden retornar por una redirección del navegador, pasandolos cómo un `queryParameter`, cambiando de página
definida en `return-url`.
```html
<eda-interface
  digital-aset-code="63fcecb5a7bd6165bbdd3cd0"
  return-url="https://mypage/"
  asset-action="validate">
</eda-interface>
```

O pasar los datos a la misma página donde este la interfaz apartir de un `CustomEvent`. Agregando un `EventLister` a 
`returnEda`.

```html
<eda-interface
  id="eda-interface"
  digital-asset-code="63fcecb5a7bd6165bbdd3cd0"
  asset-action="validate">
</eda-interface>
```
```javascript
const edaInterface = document.getElementById('eda-interface');
edaInterface.addEventListener('returnEda', (data) => {
  console.log(data);
});
```
### Modelo de Datos: Validate
Valor|Tipo|Descripción
---|---|---
assetCode|`Codigo Activo Digital`|Código del Activo Digital
assetName|`string`|Nombre del Activo Digital
hash|`string`|Huella Digital o Hash del Activo Digital
txId|`string`|Id de la transacción de Blockchain
url|`URL`|URL de la transacción en la Blockchain
createdAt|`date`|Fecha del momento de la creación del Activo Digital
validated|`boolean`|Define si el activo digital fue validado
method|`string`|Accíon de EDA realizada

### Modelo de Datos: Generate
Valor|Tipo|Descripción
---|---|---
assetCode|`string`|Código del Activo Digital
assetName|`string`|Nombre del Activo Digital
validateUrl|`EDA URL`|URL para validar el Activo Digital en EDA
user|`Token Usuario de EDA`|Token del Usuario que Registro el Activo Digital
method|`string`|Acción de EDA realizada

__Los datos retornados no varian su contenido si son pasados por el `Custom Event` o por el `return-url`. Pero puede que cambie su formato__

## Configuración

La interfaz de Easy Digital Assets usa de atributos personalizados para 
pasar los valores de configuración al Custom HTML Element. Por eso 
se debe pasar cómo atributo cada parametro que se requiera.

```html
<eda-interface parametro="valor"></eda-interface>
```

La configuración de la Interfaz de EDA depende de la acción que esta vaya a realizar.

### Lista de Acciones
Parametro|Requerido|Valores|Uso
---|---|---|---
asset-action|Sí|`validate` \|\| `generate`|Metodo que se va a realizar la Interfaz de EDA

### Acciones Disponibles
Metodo|Nombre|Uso
---|---|---
validate|Validar Activo|Valida un activo digital con los servicios de EDA
generate|Generar Activo|Genera un activo digital con los servicios de EDA

### Lista de Parametros por Acción
Cada acción puede tener parametros requeridos para su funcionamiento y parametros opcionales.

### Validate
Parametro|Requerido|Atributo No Compatible|Valores|Uso
---|---|---|---|---
digital-asset-code|Sí||`Codigo Activo Digital`|El Id del activo digital a verificar

### Generate
Parametro|Requerido|Atributo No Compatible|Valores|Uso
---|---|---|---|---
user-access-key|Sí||`Token Usuario de EDA`|Llave de sesión vigente de un usuario activo de Easy Digitial Assets
digital-asset-name|No||`string`|Determina el nombre del activo digital que se va a generar
restart-on-return|No|`restart-on-close` `reload-on-return` `close-on-return`|`boolean`|Determina si se va a recargar EDA para crear otro activo digital una vez se retorne los datos del `CustomEvent`

### Parametros Generales (Disponibles para cualquier Acción)
Parametro|Requerido|Atributo No Compatible|Valores|Uso
---|---|---|---|---
interface-id|Sí||`string`|ID único de la interfaz, si no se pasa un valor, este se generará automáticamente. Lanzara error si hay una interfaz con el mismo ID
return-url|No||`URL`|URL a la que se retornara los datos del activo digital, una vez la acción sea completada
reload-on-return|No|`close-on-return` `restart-on-return`|`boolean`|Determina si se recarga la página web completa o solo el modal de EDA, al momento de retornar los datos del activo digital
close-on-return|No|`reload-on-return` `restart-on-return`|`boolean`|Determina si se cierra el Modal una vez se complete la acción de EDA
restart-on-close|No|`restart-on-return`|`boolean`|Determina si se va a recargar EDA si el modal se llega a cerrar
dev-env|No||`local` \|\| `sandbox` \|\| `production`|Determina si va a usar recursos, cómo iconos y otros, locales u remotos.


# Licencia
MIT - 2023 Linea de Código SAS
