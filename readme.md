# Interfaz Para Easy Digital Assets

Custom HTML Element de uso sencillo para poder acceder a los 
servicios de Easy Digital Assets. Desarrollado por Línea de Código SAS.

## Índice 
1. [Funcionamiento](#functionality)
    - [Acciones de EDA](#functionality-actions)
      - [Generar Activos Digitales](#functionality-actions-generate)
      - [Validar Activos Digitales](#functionality-actions-validate)
2. [Descarga](#download)
3. [Uso](#usage)
    - [Implementación con HTML](#usage-html)
    - [Implementación con JS](#usage-js)
    - [Retorno de Datos](#usage-dataReturn)
      - [Retorno por returnEda](#usage-dataReturn-returnEda)
      - [Retorno por URL](#usage-dataReturn-url)
      - [Datos de Retorno de Generate](#usage-dataReturn-generateModel)
      - [Datos de Retorno de Validate](#usage-dataReturn-validateModel)
4. [Configuración](#config)
    - [Cambiar o Asignar Parametros](#config-params-howTo)
    - [Parametros Para Generate - Generar](#config-params-generate)
    - [Parametros Para Validate - Validar](#config-params-validate)
    - [Parametros Generales - Ambas Acciones](#config-params-general)
5. [Contacto](#contact)
6. [Licencias](#licence)

# <span id="functionality">Funcionamiento</span>

La interfaz va a poder acceder a los servicios de Easy Digital Assets. 
Estos estan definidos cómo `Acciones de EDA`.

## <span id="functionality-actions">Acciones de EDA</span>

Las `Acciones de EDA` son diferentes servicios que EDA puede ofrecer a travez de la interfaz.
Estas son necesarias para definir y configurar la interfaz para que su funcionamiento sea correcto.
Se tienen disponibles las Siguientes Acciones:

### <span id="functionality-actions-generate">`Generate` - Generar Activos Digitales</span>

Nos permite generar un Activo Digital ingresando su nombre, y el archivo de esté para
generar su huella digital, o hash. 

Una vez generado, nos mostrará el certificado del registro en la blockchain con los datos generados y 
ingresados. Estos datos se retornaran por un medio definido en la interfaz.

### <span id="functionality-actions-validate">`Validate` - Validar Activos Digitales</span>

Nos permite verificar apartir de su Código. Si un Activo Digital esta registrado en la Blockchain, los datos que se subieron a la 
Blockchain, y el certificado del registro del Activo Digital. 

Además, si tenemos el archivo con el cual fue generado el Activo Digital, podemos verificar si este archivo ha sido alterado verificando su huella digital
con la huella digital registrada en la Blockchain.

Una vez verificado que esté Registrado y su Huella Digital. Se retornara los datos del Activo Digital por un medio definido en la interfaz.

# <span id="download">Descarga</span>
Para implementar la interfaz se debe agregar en el Body del HTML el siguiente archivo Javascript.
```html
<script src="https://lineadecodigo.net/cdn/eda/eda-interface.js"></script>
```

# <span id="usage">Uso</span>

Una vez agregado el Script, se puede usar el [Custom Element](https://developer.mozilla.org/en-US/docs/Web/API/Web_components/Using_custom_elements) de 
`eda-interface` para realizar una `Acción de EDA`.

### <span id="usage-html">Implementación con HTML</span>
La Interfaz se puede crear directamente desde el HTML, usando el elemento `<eda-interface></eda-interface>`.
Además de agregar los parametros necesarios para su funcionamiento.

```html
<eda-interface
  digital-asset-code="63fcecb5a7bd6165bbdd3cd0"
  asset-action="validate">
</eda-interface>
```

### <span id="usage-js">Implementación con Javascript</span>
La Interfaz se puede crear usando `document.createElement` desde un Script, generando un elemento 
`eda-interface`. Además de agregar sus parametros necesarios para su funcionamiento.

```javascript
const edaInterface = document.createElement('eda-interface');
edaInterface.setAttribute('asset-action', 'validate');
edaInterface.setAttribute('digital-asset-code', '63fcecb5a7bd6165bbdd3cd0');
```

## <span id="usage-dataReturn">Retorno de Datos</span>

Después de completar una `Acción de EDA` la Interfaz retornara los datos de está mediante un medio 
especificado en su (configuración)[#config-params-general]. Los medios de retorno son:

### <span id="usage-dataReturn-returnEda">Retorno por `returnEda`</span>

Este medio nos permite usar los datos retornados en la misma página web donde se implemente al interfaz de EDA.
Dando una mejor integración en la página web.

La forma más sencilla de implementarlo es agregar un `id` a la interfaz y desde un script agregarle un `Event Listener`
para el [Custom Event](https://developer.mozilla.org/en-US/docs/Web/API/CustomEvent) de `returnEda`.

El cual va a ser ejecutado cuando EDA le envíe una señal de completación a la Interfaz.
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
Los datos que se retornarán dependran de la `Acción de EDA` que se haya realizado.
- [Datos de `Generate`](#usage-dataReturn-generateModel)
- [Datos de `Validate`](#usage-dataReturn-validateModel)

### <span id="usage-dataReturn-url">Retorno por `return-url`</span>

Este medio nos permite enviar los datos a una URL, definida en el [parametro](#config-params-howTo) mediante un método HTTP GET, 
cuando el usuario quiera retornar al sitio web dandole click a un botón.
```html
<eda-interface
  digital-aset-code="63fcecb5a7bd6165bbdd3cd0"
  return-url="https://mypage/"
  asset-action="validate">
</eda-interface>
```
Los datos que se retornarán dependran de la `Acción de EDA` que se haya realizado.
- [Datos de `Generate`](#usage-dataReturn-generateModel)
- [Datos de `Validate`](#usage-dataReturn-validateModel)

__Los datos retornados por `return-url` no varian frente a `returnEda`. Pero puede que cambie su formato__
  
### <span id="usage-dataReturn-generateModel">Modelo de Datos Retorno: `Generate`</span>
Valor|Tipo|Descripción
---|---|---
assetCode|`string`|Código del Activo Digital
assetName|`string`|Nombre del Activo Digital
hash|`string`|Huella Digital o Hash del Activo Digital
txId|`string`|Id de la transacción de Blockchain
url|`URL`|URL de la transacción en la Blockchain
validateUrl|`EDA URL`|URL para validar el Activo Digital en EDA
user|`Token Usuario de EDA`|Token del Usuario que Registro el Activo Digital
createdAt|`date`|Fecha de creación del Activo Digital
method|`string`|Acción de EDA realizada

### <span id="usage-dataReturn-validateModel">Modelo de Datos Retorno: `Validate`</span>
Valor|Tipo|Descripción
---|---|---
assetCode|`Codigo Activo Digital`|Código del Activo Digital
assetName|`string`|Nombre del Activo Digital
hash|`string`|Huella Digital o Hash del Activo Digital
txId|`string`|Id de la transacción de Blockchain
url|`URL`|URL de la transacción en la Blockchain
createdAt|`date`|Fecha de creación del Activo Digital
validated|`boolean`|Define si el activo digital fue validado
method|`string`|Accíon de EDA realizada

# <span id="config">Configuración</span>

La Interfaz de EDA tiene que ser configurada para su uso adecuado.
__Principalmente definir la `Acción de EDA`__. 
Esta configuración se hace pasando parametros al Elemento de la interfaz.
Estos parametros son customizados, creados usando los [Custom Elements](https://developer.mozilla.org/en-US/docs/Web/API/Web_components/Using_custom_elements)

## <span id="config-params-howTo">Cómo usar Parametros en la Interfaz de EDA</span>

Definir los parametros de la interfaz no tienen gran diferencia a su forma de 
definirlos en un Elemento HTML cualquiera.

Directamente desde el HTML
```html
<eda-interface 
  parametro="valor"
  parametroBooleano>
</eda-interface>
```
Usando un Javascript para definirlos
```html
<eda-interface 
  id="eda-interface">
</eda-interface>
```
```javascript
document.getElementById('eda-interface').setAttribute('parametro', 'valor');
```

__El cambio entre los Parametros Generales y los Parametros de la Interfaz es 
que se reinicia la Interfaz cada vez que se define o se cambia un Parametro. 
Comportamiento heredado de los [Custom Elements](https://developer.mozilla.org/en-US/docs/Web/API/Web_components/Using_custom_elements).__

La lista de Parametros de la Interfaz varia según la `Acción de EDA`. 
__Cada Acción tiene Parametros Obligatorios, sin estos, la interfaz no va a funcionar correctamente.__

Hay una lista de Parametros Generales que son permitidos en cualquier `Acción de EDA`, principalmente sobre 
el uso y interacción de la Interfaz.

### <span id="config-params-generate">Parametros de `Generate`</span>
Parametro|Requerido|Atributo No Compatible|Valores|Uso
---|---|---|---|---
user-access-key|Sí||`Token Usuario de EDA`|Llave de sesión vigente de un usuario activo de Easy Digitial Assets
digital-asset-name|No||`string`|Determina el nombre predefinido del activo digital que se va a generar
restart-on-return|No|`restart-on-close` `reload-on-return` `close-on-return`|`boolean`|Determina si se va a recargar EDA para crear otro activo digital una vez se retorne los datos a `returnEda`

### <span id="config-params-validate">Parametros de `Validate`</span>
Parametro|Requerido|Atributo No Compatible|Valores|Uso
---|---|---|---|---
digital-asset-code|Sí||`Codigo Activo Digital`|El Id del activo digital a verificar

### <span id="config-params-general">Parametros Generales (Disponibles para cualquier Acción)</span>
Parametro|Requerido|Atributo No Compatible|Valores|Uso
---|---|---|---|---
interface-id|Sí||`string`|ID único de la interfaz, si no se pasa un valor, este se generará automáticamente. Lanzara error si hay una interfaz con el mismo ID
interface-asset-word|No||`string`|Cambia en los metodos la palabra "Activo Digital" por el valor ingresado. Por defecto, la palabra es "Activo Digital" 
return-url|No||`URL`|URL a la que se retornara los datos del activo digital, una vez la acción sea completada
reload-on-return|No|`close-on-return` `restart-on-return`|`boolean`|Determina si se recarga la página web completa o solo el modal de EDA, al momento de retornar los datos del activo digital
close-on-return|No|`reload-on-return` `restart-on-return`|`boolean`|Determina si se cierra el Modal una vez se complete la acción de EDA
restart-on-close|No|`restart-on-return`|`boolean`|Determina si se va a recargar EDA si el modal se llega a cerrar
dev-env|No||`local` \|\| `sandbox` \|\| `production`|Determina si va a usar recursos, cómo iconos y otros, locales u remotos.

# <span id="contact">Contacto</span>

- [Página Web de LDC](https://lineadecodigo.net/home)
- Email: ventas@lineadecodigo.net
- Télefonos: 
    - +57 301-641-5626
    - +57 304-482-7833

## <span id="licence">Licencia</span>
MIT - 2023 Linea de Código SAS
