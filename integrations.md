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

# Paso 5 (ver Generar un template desde cero)
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
* Reiniciamos nginx `sudo service nginx restart`
* Reiniciamos integrations `sudo service integrations restart`

# Paso 14
Verificar que la url este activa en `integrations.blick.mx/<url dada de alta>`

# Generar un template desde cero

En el paso 5 buscar una clase que tenga algo parecido a esto: 

```
class RERContactWithCompanyView(MailgunGenericContactView):
    KEY = settings.MAILGUN_API_KEY
    DOMAIN = settings.RER_MAILGUN_DOMAIN
    RECIPIENT = settings.RER_MAILGUN_RECIPIENT
    EMAIL_TEMPLATE = 'email/rer_contact.html'
    FROM_TEXT = 'RER Energy Group'
    SUBJECT = 'Nuevo contacto desde página web'

    @method_decorator(csrf_exempt)
    def dispatch(self, request, *args, **kwargs):
        return super(RochaLanderoCarrerView, self) \
            .dispatch(request, *args, **kwargs)

    def post(self, request):
        ctx = {
            'name': request.POST.get('name'),
            'email': request.POST.get('email'),
            'phone': request.POST.get('phone'),
            'birthday': request.POST.get('birthday'),
            'master': request.POST.get('master'),
            'languages': request.POST.get('languages'),
        }

        body = loader.render_to_string(self.EMAIL_TEMPLATE, ctx)

        endpoint = 'https://api.mailgun.net/v3/{0}/messages'.format(self.DOMAIN)
        response = requests.post(
            endpoint, auth=('api', self.KEY), data={
                'from': '{0} <postmaster@{1}>'.format(self.FROM_TEXT, self.DOMAIN),
                'to': self.RECIPIENT,
                'subject': self.SUBJECT,
                'html': body
            })

        if response.status_code != 200:
            value = '0'
        else:
            value = '1'

        return HttpResponse(value)
```
# Paso 5.1

Agregar los campos requeridos por el formulario

```
...
ctx = {
            'name': request.POST.get('name'),
            'email': request.POST.get('email'),
            'phone': request.POST.get('phone'),
            'birthday': request.POST.get('birthday'),
            'master': request.POST.get('master'),
            'languages': request.POST.get('languages'),
        }
...
```
# Paso 5.2

Agregar el template en la siguiente dirección: `/integrations/templates/email` con el mismo nombre que esta en el email_template

# Paso 5.3

Agregar en el `dispatch` el mismo nombre de la clase


