# Paso 1
Loguearse con las cuentas de more a mailchimp (https://us1.admin.mailchimp.com/)

# Paso 2
Generar una lista de difusión dandole click a Lists
![Lista](http://www.clipular.com/c/5308738503442432.png?k=5ZepQeMbpyRGXTROaA_vRLagwCo)

Aparecera algo como esto

![New](http://www.clipular.com/c/5840796367716352.png?k=9Zi4CEqJRD83_Ryz4F_ZyjcBqC0)

Una vez creada se verá algo como esto

![Creación](http://www.clipular.com/c/6394703242330112.png?k=c_tIX7-7-8_tlfaRDld_63RfW28)

# Paso 3
En la tab de Settings dar click en `list name and defaults`

![list](http://www.clipular.com/c/4997629326131200.png?k=t5yfN_Uw8b99fsXMDQpbxuLVuS0)

Ubicamos el ID de la lista que acabamos de crear de lado derecho
![list](http://www.clipular.com/c/6620852086112256.png?k=-9BSA_a_HxTFP3iV7d3vC2rMVMQ)

# Paso 4
Crear una API key para la lista seleccionada llendo a la parte superior derecha y en `Account` y despues en la tab de `extras` ir a API KEYS

![list](http://www.clipular.com/c/5604048081518592.png?k=ruhyJadHEz05muLoIwdCLqI5SSg)

# Paso 5
Una vez generada saldrá algo así

![list](http://www.clipular.com/c/5811906807070720.png?k=65SRycSxJB-IWq1nn0cLj_a4nbk)

el SHARD viene al final de la API-KEY por ejemplo `asldiygsaldqhwi89dohasuhdv-us1`, `us1` es el shard

# Paso 6

Abrir el archivo de base.py que esta en integrations/config/settings/base.py y agregar las siguientes líneas

```
WORKING_LABS_MAILCHIMP_API_KEY = env('WORKING_LABS_MAILCHIMP_API_KEY', default='CHANGEME!!!')
WORKING_LABS_MAILCHIMP_SHARD = env('WORKING_LABS_MAILCHIMP_SHARD', default='CHANGEME!!!')
WORKING_LABS_MAILCHIMP_LIST_ID = env('WORKING_LABS_MAILCHIMP_LIST_ID', default='CHANGEME!!!')
```

Donde
**WORKING_LABS_MAILCHIMP_API_KEY** Es la API-KEY sin el shard
**WORKING_LABS_MAILCHIMP_SHARD** Es el shard
**WORKING_LABS_MAILCHIMP_LIST_ID** Es el id de la lista

# Paso 7
Ir a la carpeta de integrations/apps/mailchimp/ y abrir el archivo urls.py y agregar la siguientes líneas según nombre de proyecto dentro del arreglo de `urlpatterns = [`

```
urlpatterns = [
...


url(regex='^florelia/newsletter/$',
        view=views.FloreliaNewsletterView.as_view(),
        name='florelia_newsletter'),
        
        
...]

```

# Paso 7
En la misma carpeta (integrations/apps/mailchimp/) abrir el archivo views.py agregar un registro similar al siguiente:

```
class HigiaNewsletterView(MailchimpGenericNewsletterView):
    KEY = settings.HIGIA_MAILCHIMP_API_KEY
    LIST_ID = settings.HIGIA_MAILCHIMP_LIST_ID
    SHARD = settings.HIGIA_MAILCHIMP_SHARD
```

# Paso 8
Hacer commits correspondientes en tu branch y hacer tu Pull Request

# Paso 9
Tener la llave de .pem de blick `blick.pem`

# Paso 10
Conectarse vía SSH a la cuenta donde esta integraciones.mx con la llave pem

# Paso 11
Loguearse como integrations `sudo su integrations`

# Paso 12
Colocarse en la ubicación del repositorio `cd`, hacer un `cd integrations` y jalar los cambios del repo `git pull origin master` ya que se aprobo el PR

# Paso 13
Hacemos un `cd` y accedemos a `cd deploy` y abrimos con el editor de textos favorito (vim, vi o nano) el archivo deploy y agregamos las variables necesarias dependiendo del proyecto

```
export HIGIA_MAILCHIMP_API_KEY="API_KEY"
export HIGIA_MAILCHIMP_SHARD="SHARD"
export HIGIA_MAILCHIMP_LIST_ID="LISTID"
```

# Paso 14
Reiniciamos nginx `sudo service nginx restart`
Reiniciamos integrations `sudo service integrations restart`

# Paso 15
Verificar que la url este activa en `integrations.blick.mx/<url dada de alta>`


