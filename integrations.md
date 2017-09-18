# Paso 1
Clonar repositorio de [integrations](https://github.com/BlickLabs/integrations) y abrir en tu editor favorito.

`cd integrations`

# Paso 2
Generar nuestro issue con la descripción de los proyecto(s) que se van a integrar

# Paso 3
Generar rama de desarrollo con el issue asociado
```
<número issue>_integrations/<descripción>_<nombre del colaborador>

Por ejemplo:

80_integrations/rer_contact_enriquelc
```

# Paso 4
Ir a la carpeta de `integrations/apps/mailgun/` y abrir el archivo `urls.py` y agregar la siguientes líneas según nombre de proyecto dentro del arreglo de `urlpatterns = [`

```
urlpatterns = [
...
url(regex='^finacero/email/$',
        view=views.FinaceroContactView.as_view(),
        name='finacero_email'),
        
...]
```

Where:

**regex** es la expresión regular en donde se aplicará el filtro de la url

**view** es la vista que se genera en el archivo de `views.py` (ver el siguiente paso)

**name** es el nombre de la url

# Paso 5
En la misma carpeta (integrations/apps/mailgun/) abrir el archivo `views.py` agregar un registro similar al siguiente:

```
class FinaceroContactView(MailgunGenericContactView):
    KEY = settings.MAILGUN_API_KEY
    DOMAIN = settings.FINACERO_MAILGUN_DOMAIN
    RECIPIENT = settings.FINACERO_MAILGUN_RECIPIENT
    EMAIL_TEMPLATE = 'email/generic_contact.html'
    FROM_TEXT = 'Finacero'
    SUBJECT = 'Nuevo contacto desde pagina web'
```

Where:

**KEY** es la llave de la cuenta de mailgun (ver alta de dominios en mailgun) por default utilizamos la misma para todas las cuentas actuales

**DOMAIN** es el dominio que esta dado en la cuenta de mailgun

**RECIPIENT** es la dirección a la que van a llegar los correos

**EMAIL_TEMPLATE** es el template que se va a renderear con los datos del contacto `/integrations/integrations/templates/email/generic_contact.html`

**FROM_TEXT** es el nombre que aparece en el encabezado del correo

**SUBJECT** es el título del mail


_Recuerda la identación debe de ser la misma para que el compilador de python reconozca los cambios_
_El nombre de FinaceroContactView debe de ser el mismo de la clase `class`_

# Paso 6
Abrir el archivo de `base.py` que esta en `integrations/config/settings/base.py` y agregar las siguientes líneas

```
FINACERO_MAILGUN_DOMAIN = env('FINACERO_MAILGUN_DOMAIN', default='CHANGEME!!!')
FINACERO_MAILGUN_RECIPIENT = env('FINACERO_MAILGUN_RECIPIENT', default='CHANGEME!!!')
```

# Paso 7
Hacer commits correspondientes en tu branch y hacer tu Pull Request

# Paso 8
Tener la llave de .pem de blick `blick.pem`

# Paso 9
Conectarse vía SSH a la cuenta donde esta integraciones.mx con la llave pem

# Paso 10
Loguearse como integrations `sudo su integrations`

# Paso 11
Colocarse en la ubicación del repositorio `cd`, hacer un `cd integrations` y jalar los cambios del repo `git pull origin master` ya que se aprobo el PR

# Paso 12
Hacemos un `cd` y accedemos a `cd deploy` y abrimos con el editor de textos favorito (vim, vi o nano) el archivo deploy y agregamos las variables necesarias dependiendo del proyecto

```
export FINACERO_MAILGUN_DOMAIN="mg.finacero.mx"
export FINACERO_MAILGUN_RECIPIENT="info@finacero.mx"
```
# Paso 13
Reiniciamos nginx `sudo service nginx restart`

# Paso 14
Verificar que la url este activa en `integrations.blick.mx/<url dada de alta>`






