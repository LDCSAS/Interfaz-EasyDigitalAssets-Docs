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
Parametro|Requerido|Valores|Uso
---|---|---|---
digital-asset-code|Sí|Id de un activo digital|El Id del activo digital a verificar
return-url|No|URL|URL a la que se retornara los datos del activo digital, una vez esté este verificado                                  |
reload-on-return|No|Booleano|Determina si se recarga la página web completa o solo el modal de EDA, al momento de retornar los datos del activo digital

### Generate

Parametro|Requerido|Valores|Uso
---|---|---|---
user-access-key|Sí|Llave de Acceso un Usuario de EDA|Llave de sesión vigente de un usuario activo de Easy Digitial Assets
digital-asset-name|No|String|Nombre del activo digital que se va a generar
return-url|No|URL|URL a la que se retornara los datos del activo digital, una vez esté este registrado
reload-on-return|No|Booleano|Determina si se recarga la página web completa o solo el modal de EDA, al momento de retornar los datos del activo digital


# Licencia
