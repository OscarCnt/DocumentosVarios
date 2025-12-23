---
title: Guía de integración del plugin Milestone Event MIP
slug: iris-plus-integrations/milestone-event-mip-plugin-integration-guide
createdAt: 2024-08-20T10:12:57.576Z
updatedAt: 2025-09-30T09:46:15.289Z
---

## Ediciones y versiones compatibles de Milestone XProtect y productos IRIS+™ compatibles

La integración de IRIS+ se alinea con tu despliegue de Milestone XProtect para cubrir tus necesidades. Es compatible con Milestone XProtect Express, Milestone XProtect Corporate y con despliegues de Milestone Open Network Bridge.

Consulta [aquí](https://docs.irisity.com/iris-plus-integrations#1mztU) las versiones compatibles de Milestone XProtect.

## Compatibilidad del sistema

El plugin Milestone Event MIP solo es compatible con versiones de **Microsoft Windows Server**. No se admite la instalación en versiones de escritorio de Windows 10 o Windows 11.

## Compatibilidad con Milestone Interconnect

Milestone Interconnect es un hub central de videovigilancia que integra instalaciones más pequeñas y remotas de Milestone XProtect. Por lo tanto, funciona como un sitio central para el acceso a streams de video. Integrado con la solución analítica IRIS+, permite una vista centralizada de las alarmas de IRIS+ provenientes de sitios Milestone dispersos (integrados individualmente en IRIS+) dentro de la herramienta Smart Client de Milestone Interconnect. Para más detalles, consulta la [**documentación de Milestone**](https://doc.milestonesys.com/latest/en-US/feature_flags/ff_interconnectedproducts/sc_miinterconnect.htm?Highlight=INTERCONNECT).

## Sincronización de hora

IRIS+™ ofrece opciones de sincronización de hora soportadas como parte de la integración con Milestone XProtect. Los eventos detectados en IRIS+™ pueden sincronizarse con una de las siguientes referencias de tiempo:

- **Eventos detectados sincronizados con la hora del dispositivo IRIS+™ Edge™**: sincroniza las cámaras de Milestone XProtect con la hora del dispositivo IRIS+™ Edge™. Es relevante cuando Milestone XProtect **no** tiene desplegado Open Network Bridge.

- **Recomendado: eventos detectados sincronizados con la hora del stream de video (cuando está disponible)**: soportado para cámaras con Milestone Open Network Bridge desplegado. Este método proporciona la mejor sincronización entre IRIS+™ y Milestone XProtect.

Nota: Para más información sobre la solución Milestone Open Network Bridge, consulta esta [**documentación**](https://doc.milestonesys.com/latest/en-US/portal/htm/chapter-page-onvif.htm).

# Mapear cámaras

## Antes de empezar

La integración IRIS+™ con Milestone XProtect toma como objetivo cámaras (o los streams de video de las cámaras) definidos en Milestone XProtect y los mapea a IRIS+™. Este procedimiento de mapeo implica que la cámara debe estar definida en IRIS+™ y que se requiere el ID de la cámara en Milestone XProtect, comúnmente llamado GUID (Globally Unique Identifier), para identificar y enlazar la cámara en IRIS+™. De este modo, el plugin MIP puede identificar las cámaras origen y sus eventos detectados.

Este procedimiento también es compatible con Milestone Interconnect. Al recuperar el GUID de la cámara, asegúrate de hacerlo para la versión y las herramientas de Milestone Interconnect correspondientes.

## Obtener el ID de la cámara

### XProtect Professional

1. Abre **systeminfo.xml** en un navegador web aquí: *http\://localhost//rcserver/systeminfo.xml*
2. Busca el nombre de la cámara en el archivo **systeminfo.xml**.
3. El GUID de la cámara se muestra debajo del nombre de la cámara.
4. Copia el GUID y consérvalo para usarlo más adelante en el Portal IRIS+™.
5. Repite la configuración para todas las cámaras que desees conectar a IRIS+™.

### XProtect Corporate, XProtect Professional+

Los GUID de cámara están disponibles en este archivo:

C:\Program Files\Milestone\MIPPlugins\IRIS+EventMonitoring\MilestoneCameras.ini

**Portal IRIS+™**

- Abre el Portal IRIS+™ y localiza la cámara correspondiente.
- En la pestaña **Settings** de la cámara, haz clic en **Edit**.
- En el campo **External ID**, ingresa el ID de cámara que recuperaste para esta cámara en el XProtect Management Client. Este campo distingue mayúsculas y minúsculas, por lo que debes ingresar el ID exactamente como fue obtenido de Milestone XProtect.

![](https://api.archbee.com/api/optimize/0qwUIPTrS6-vWLcHFHOFA-Wh6g_ZWhGtbZqo3BWJHBg-20240730-141309.png)

- Para un despliegue con Milestone Interconnect, ingresa el ID de cámara específico de Interconnect en el campo **External ID**. Si esto se agrega como soporte adicional sobre el VMS Milestone XProtect, separa ambas entradas con una coma.

:::hint{type="info"}
Se pueden aplicar múltiples *External IDs* a una sola cámara; asegúrate de presionar **Enter** en tu teclado para registrar múltiples *External IDs* antes de guardar.
:::

- Agrega la cadena de conexión de la cámara en **Video stream source** dentro de **Video Source**. **Solo para despliegue Open Network Bridge**, utiliza el siguiente formato de URI en el campo **Video Stream Source**:

  RTSP://\[username]:\[password]@\[hostname/IP]:\[Open Network Bridge RTSP port]/live/\[Milestone camera ID]

  Para más información, consulta este tutorial de Milestone: [**https://www.youtube.com/watch?v=-LIRbga2LOk**](https://www.youtube.com/watch?v=-LIRbga2LOk)

- Asegúrate de que el interruptor **Sync time to stream** esté habilitado (solo para despliegue Open Network Bridge). Ver imagen:

![](https://api.archbee.com/api/optimize/0qwUIPTrS6-vWLcHFHOFA-QS6MUIAxY8RurNOY09bn1-20240730-143003.png)

Puedes integrar IRIS+™ con **dos** sistemas Milestone diferentes. Debes agregar el segundo GUID en las cámaras dentro de IRIS+™ separado por una coma.

# Habilitar la integración con Milestone en el Portal IRIS+™

## Antes de empezar

Esta sección describe cómo crear una **cuenta de servicio** y un **token** de IRIS+™ en el Portal IRIS+™ para posteriormente enlazarlos con el Milestone XProtect Management Client.

Estos pasos asumen que tu cuenta de IRIS+™ ya fue configurada y que las carpetas, dispositivos y cámaras ya están definidos. Si no es así, primero accede a los tutoriales de IRIS+™ desde el **Support hub** de IRIS+™ para configurar tu cuenta.

En IRIS+™, se requiere una cuenta de servicio para enlazar posteriormente el plugin MIP de Milestone XProtect con IRIS+™. La cuenta de servicio de IRIS+™ proporciona un token, que es el identificador utilizado para enlazar la cuenta con el plugin MIP.

Si vas a conectar múltiples cuentas IRIS+™ a un solo despliegue de VMS Milestone, este procedimiento debe realizarse para **cada** cuenta IRIS+™.

### Para generar un token de Cuenta de Servicio de IRIS+™, realiza lo siguiente:

- Navega a tu cuenta de IRIS+™, pestaña **Settings**.

![](https://api.archbee.com/api/optimize/0qwUIPTrS6-vWLcHFHOFA-OaIctd9uyD-MkeiwaiPla-20240730-143617.png)

- Haz clic en la pestaña **Users**.

![](https://api.archbee.com/api/optimize/0qwUIPTrS6-vWLcHFHOFA-wzfVpa9BVGm2nGKtpcVHY-20240730-143839.png)

- Haz clic en **Add** y selecciona **Create service account**.

![](https://api.archbee.com/api/optimize/0qwUIPTrS6-vWLcHFHOFA-BampbT0BkxZ9-QvbtkWzj-20240730-144355.png)

::Image[]{src="https://api.archbee.com/api/optimize/0qwUIPTrS6-vWLcHFHOFA-C4RpnLA145-YYWP0bRJwm-20240730-144153.png" signedSrc size="44" width="452" height="532" position="center" caption}

- Ingresa un nombre de cuenta de servicio, por ejemplo, “Milestone Integration”. Ingresa una descripción (opcional).
- Haz clic en **Create Service Account**.
- En la lista de usuarios, selecciona el usuario de cuenta de servicio creado, haz clic en la flecha junto al nombre para expandir la vista y selecciona **Get Token**.

::Image[]{src="https://api.archbee.com/api/optimize/0qwUIPTrS6-vWLcHFHOFA-77S0ldn21TIegZnYosnAU-20240730-145021.png" signedSrc size="88" width="872" height="322" position="center" caption}

- Define la expiración del token si es necesario (por defecto, el token nunca expira).

::Image[]{src="https://api.archbee.com/api/optimize/0qwUIPTrS6-vWLcHFHOFA-114Hrpf5DDNKvt_weue12-20240730-145658.png" signedSrc size="56" width="523" height="363" position="center" caption}

- Haz clic en **Get token** y anota el token generado (se utilizará en la configuración del plugin MIP Event Monitoring):

::Image[]{src="https://api.archbee.com/api/optimize/0qwUIPTrS6-vWLcHFHOFA-ttz26S9CaNnL9_OJnz50K-20240730-145847.png" signedSrc size="56" width="526" height="341" position="center" caption}

- Cierra la ventana.

# Instalar y configurar el plugin MIP IRIS+™ Event Monitoring

La integración de IRIS+™ y Milestone XProtect, basada en Milestone Integration Platform (MIP), ofrece los siguientes beneficios:

- Configuración sencilla: se requieren solo unos pasos para recibir eventos para cualquier número de cámaras y cualquier número de reglas analíticas por cámara.
- Puedes ver eventos pasados, navegar a una grabación de video asociada a un evento específico y visualizar el tracking analítico de ese evento.

Nota: Para recibir eventos de IRIS+™ en Milestone XProtect, asegúrate de que todas las máquinas con **Milestone XProtect Event Server** y/o las máquinas con **Milestone XProtect Management Client** tengan acceso saliente TCP hacia los servidores de IRIS+:
- \*.irisity.io:443 (alojado por Irisity), o
- \*.irisplus.app:443 (despliegue On‑Prem).

## Descargar e instalar el plugin MIP IRIS+™ Event Monitoring

**Antes de empezar**

Instala el plugin MIP IRIS+™ Event Monitoring en todas las PC que hospeden:

- Milestone XProtect Event Server
- Cliente Milestone (Management Client o Smart Client)
- Servidor Milestone Interconnect y aplicaciones de administración

Nota: Si ya hay instalada una versión anterior del plugin MIP Event Monitoring, instala la nueva versión **encima** de la existente (es decir, actualización).

**Para instalar el plugin MIP IRIS+™ Event Monitoring (o actualizar desde una versión previa), realiza lo siguiente:**

- Cierra la aplicación **Milestone XProtect Management Client**.
- Descarga la versión más reciente del plugin desde [**Available Downloads**](docId\:M2kgcn_L71j33O0fw92Mq). (1.0.0.143)
- Ejecuta el asistente de instalación del plugin MIP IRIS+™ Event Monitoring. Sigue las instrucciones hasta completar la instalación.

## Inicializar IRIS+™ en Milestone XProtect Management Client

La integración soporta los siguientes métodos:

**IRIS+™ Core**  
Envía eventos analíticos y overlay (superposición) de eventos desde IRIS+™ Core hacia Milestone XProtect.  
Envía eventos de salud (health) desde IRIS+™ Core hacia Milestone XProtect.

**IRIS+™ Edge™ Device**  
Envía eventos analíticos desde el dispositivo IRIS+™ Edge™ hacia Milestone XProtect.  
El overlay en vivo está disponible en Milestone XProtect Smart Client.

- **Cuando Core está disponible**  
  El overlay de eventos está disponible en Milestone XProtect Smart Client.

- **Cuando Core NO está disponible**  
  El overlay de eventos está disponible en Milestone XProtect Smart Client durante 4 segundos antes del tiempo del evento.

Notas:  
*1. El propósito de esta opción (Core no disponible) es mantener el soporte de transferencia de eventos hacia Milestone XProtect si el Core no está disponible durante periodos cortos.*  

*2. Si el dispositivo Edge™ se reinicia por cualquier motivo mientras el Core no está disponible, se pierde la conexión con las cámaras. El Core debe estar disponible para renovar la conexión.*  

*3. Los horarios de las reglas se actualizan en el dispositivo Edge™ cada 24 horas. Si el Core no está disponible por más de 24 horas, el dispositivo Edge™ deja de generar eventos.*

### Requisitos para la integración con IRIS+™ Edge™ Device

- Ejecuta Milestone XProtect Smart Client como **administrador**.
- En la pestaña **Properties / Logon** del servicio **Milestone XProtect Event Server**, selecciona un usuario administrador.
- **Cambios de External ID**:
  - Los cambios en el **External ID** de la cámara pueden realizarse en Milestone XProtect y/o en IRIS+™. Procede como sigue:
    - Cambio en Milestone XProtect: reinicia el servicio **Milestone XProtect Event Server** para aplicar el cambio de inmediato, o espera hasta 15 minutos para que el cambio surta efecto.
    - Cambio en IRIS+™: reinicia el servicio **Milestone XProtect Event Server** para reflejar el cambio.
    - Cierra y vuelve a abrir Milestone XProtect Smart Client.
    - Después de un cambio en el tipo de integración (IRIS+™ Core / IRIS+™ Edge™ Device), reinicia el servicio **Milestone XProtect Event Server** y cierra y vuelve a abrir Milestone XProtect Smart Client.

### Para inicializar la integración, realiza lo siguiente:

1. Abre **Milestone XProtect Management Client**.

2. En el árbol de navegación, expande **MIP Plug-ins → IRIS+ Event Monitoring**.

::Image[]{src="https://api.archbee.com/api/optimize/0qwUIPTrS6-vWLcHFHOFA-eEV-lJlWVreYiwOzIc55--20240801-114238.png" signedSrc size="38" width="299" height="807" position="center" caption}

3. Haz clic derecho sobre el servidor **IRIS+ Event Monitoring** y selecciona **Add New**.

*Nota*: Si conectas múltiples cuentas IRIS+™ a un solo despliegue de VMS Milestone, realiza este paso y los siguientes para cada cuenta IRIS+™.

4. Aparecerá la ventana **IRIS+ Event Monitoring Information**:

![](https://api.archbee.com/api/optimize/0qwUIPTrS6-vWLcHFHOFA-H1SQ_vvYi07Qu0yNJfKzU-20240801-114513.png)

5. Selecciona el método de integración requerido.

**Para integración Core**

- Completa los siguientes campos en la ventana:

  - **Name**: define el nombre según tu preferencia.
  - **Server URL**:
    - Despliegue alojado por Irisity: [**https://api.irisplus.ai**](https://api.irisity.io)
    - Despliegue alojado por el cliente (on‑premise): [**https://api.irisplus.app**](https://api.irisplus.app)
  - **Token**: pega el token de IRIS+ que guardaste como parte de tu cuenta de servicio.

- Marca los eventos que deseas recibir desde IRIS+:

  - **Analytics events**
  - **Health events**
    - **All health events**
    - **User-defined health event**

*Nota*: consulta el capítulo **Configuring health events** para una descripción detallada de cómo configurar eventos de salud, alarmas y acciones en el cliente de Milestone XProtect.

- **XProtect event Server IP Address**:  
  ingresa la IP correspondiente. Los campos restantes no aplican.

Ejemplo:

![](https://api.archbee.com/api/optimize/0qwUIPTrS6-vWLcHFHOFA-yANe5o5R64tEg1BeQCQ4d-20240801-115521.png)

**Para integración Edge**

Completa únicamente el campo **Name** en la ventana **IRIS+ Event Monitoring Information**:

- **XProtect event Server IP Address**: ingresa la IP correspondiente.
- **Events Integration Port**: ingresa el puerto correspondiente. Al hacerlo, el valor del campo **Events Integration URI** se actualiza automáticamente. Si realizas esta configuración en el Milestone XProtect Event Server, conserva el valor de **Events Integration URI** para usarlo en la definición de integración Edge en IRIS+ (consulta el siguiente capítulo).
- **Metadata Integration Port**: ingresa el puerto correspondiente. Al hacerlo, el valor del campo **Metadata Integration URI** se actualiza automáticamente. Si realizas esta configuración en el Milestone XProtect Event Server, conserva el valor de **Metadata Integration URI** para usarlo en la definición de integración Edge en IRIS+ (consulta el siguiente capítulo).

Ejemplo:

![](https://api.archbee.com/api/optimize/0qwUIPTrS6-vWLcHFHOFA-42ffkJTRGCqHBbdmkCSB6-20240801-115759.png)

6. Si te interesa enviar snapshots de eventos a Milestone XProtect, marca esta casilla.

Ejemplo:

![](https://api.archbee.com/api/optimize/0qwUIPTrS6-vWLcHFHOFA-GRnDIwWsBHMnR1JeBvGzN-20240801-120119.png)

7. Sal de la pantalla y selecciona **Save** cuando se te solicite.

8. Asegúrate de que **Analytics Events** estén habilitados en XProtect Management Client, haciendo lo siguiente:

- En la pestaña **Tools** (arriba), selecciona **Options**. Se abre el panel de opciones.
- Selecciona la pestaña **Analytics Events** y verifica que el campo **Enabled** esté marcado.

::Image[]{src="https://api.archbee.com/api/optimize/0qwUIPTrS6-vWLcHFHOFA-_4qi2CFIUR9T7pgJJIu8W-20240801-120453.png" signedSrc size="80" width="643" height="529" position="center" caption}

# Habilitar la integración del dispositivo Edge™ en IRIS+™

Realiza las siguientes acciones al integrar IRIS+™ con Milestone XProtect utilizando la opción de integración Edge.

Los **Integration Targets** en IRIS+™ son las definiciones a nivel sistema requeridas para que IRIS+™ integre con Milestone XProtect. Aquí definirás los endpoints que pueden recibir los eventos y metadatos enviados por IRIS+™.

## Definir Integration Targets en IRIS+™

Para definir Integration Targets en IRIS+™, realiza lo siguiente:

- Inicia sesión en tu cuenta de IRIS+™.
- En la barra superior de módulos, selecciona **Settings**.

![](https://api.archbee.com/api/optimize/0qwUIPTrS6-vWLcHFHOFA-6P3BrOfC3-Mhb84FZ8_Be-20240801-121706.png)

- Selecciona **Integration Targets**.

![](https://api.archbee.com/api/optimize/0qwUIPTrS6-vWLcHFHOFA-MAd41PvSCeNZ3LKUUuRWQ-20240801-121847.png)

- Haz clic en **Add**. Aparecerá la pantalla **Integration Target**.

![](https://api.archbee.com/api/optimize/0qwUIPTrS6-vWLcHFHOFA-rZvY9PMGa5kSpvJ0NU6mB-20240801-122101.png)

![](https://api.archbee.com/api/optimize/0qwUIPTrS6-vWLcHFHOFA-VmbrTpwtMPXi9zG8-aE_r-20240801-122211.png)

- Define lo siguiente:

**Integración de eventos (Events integration)**

- **Target name**: define un nombre significativo, por ejemplo, *Milestone Events*.
- Asegúrate de que el interruptor esté en **Enabled**.
- **Type**: conserva el valor por defecto **HTTP**.
- **Method**: conserva el valor por defecto **POST**.
- **URI**: ingresa el mismo URI que definiste en el plugin MIP instalado en el Milestone XProtect Event Server.
- Haz clic en **Save**. El Integration Target queda definido y se muestra en la lista de Integration Targets.

Ejemplo:

![](https://api.archbee.com/api/optimize/0qwUIPTrS6-vWLcHFHOFA-Du25OIPflAlFgQTFCAv9c-20240801-122559.png)

**Integración de metadatos (Metadata integration)**

- **Target name**: define un nombre significativo, por ejemplo, *Milestone Overlay*.
- Asegúrate de que el interruptor esté en **Enabled**.
- **Type**: establece **Type** en **WS**.
- **Method**: establece **Method** en **GET**.
- **URI**: ingresa el mismo URI que definiste en el plugin MIP instalado en el Milestone XProtect Event Server.
- Haz clic en **Save**. El Integration Target queda definido y se muestra en la lista de Integration Targets.

Ejemplo:

![](https://api.archbee.com/api/optimize/0qwUIPTrS6-vWLcHFHOFA-5j6vIjyMSv3bGofz7ISLR-20240801-122942.png)

## Definir integraciones del dispositivo Edge™ en IRIS+™

Las **Edge™ Device Integrations** en IRIS+™ son las definiciones específicas de Edge requeridas para que IRIS+™ integre con Milestone XProtect. Realiza lo siguiente para cada dispositivo Edge™ que desees integrar con Milestone XProtect.

Para definir la integración del dispositivo Edge™ en IRIS+™, realiza lo siguiente:

- Inicia sesión en tu cuenta de IRIS+™.
- Selecciona **Physical View**.

![](https://api.archbee.com/api/optimize/0qwUIPTrS6-vWLcHFHOFA-ThGbUNby8O5DJePwxn0Md-20240801-130513.png)

- Selecciona el IRIS+™ Edge para el cual deseas definir la integración.
- Selecciona **Integrations**.

![](https://api.archbee.com/api/optimize/0qwUIPTrS6-vWLcHFHOFA-3geqgDsEcTiCA19JX8a5s-20240801-131249.png)

- Haz clic en **Edit**.

![](https://api.archbee.com/api/optimize/0qwUIPTrS6-vWLcHFHOFA-bIzuMj4aGmzgctO1TTlp6-20240801-131405.png)

- Habilita **Events Integration** y selecciona el Integration Target correspondiente que definiste previamente (en este ejemplo: *Milestone Events*).

![](https://api.archbee.com/api/optimize/0qwUIPTrS6-vWLcHFHOFA-3pwEYPRQ2ci8qTDu0ZsE_-20240801-131533.png)

- Habilita **Metadata Integration** y selecciona el Integration Target correspondiente que definiste previamente (en este ejemplo: *Milestone Overlay*).

![](https://api.archbee.com/api/optimize/0qwUIPTrS6-vWLcHFHOFA-PavBfeVHQtI-9SUWCQVnQ-20240801-131623.png)

1. Haz clic en **Save**.

# Sincronizar la hora del Management Server de Milestone XProtect con la hora del Edge IRIS+™

### Antes de empezar

IRIS+™ soporta sincronización de hora con la hora del dispositivo IRIS+™ Edge™. Por lo tanto, las definiciones de hora del dispositivo IRIS+™ Edge™ y del **Milestone XProtect Management Server** deben estar sincronizadas. Verifica que los NTP (Network Time Protocol) estén sincronizados en ambos servidores.

Nota: Si tu despliegue está sincronizado con la hora del stream de video utilizando la solución Milestone Open Network Bridge, omite esta sección.

# Habilitar overlays (superposiciones)

Para habilitar overlays (superposiciones) en Milestone XProtect Smart Client, realiza lo siguiente:

- Abre **Milestone XProtect Smart Client**.
- Selecciona **Settings**.

::Image[]{src="https://api.archbee.com/api/optimize/0qwUIPTrS6-vWLcHFHOFA-QURhnqF4-WFW_VaCQCiIh-20240801-132425.png" signedSrc size="46" width="341" height="281" position="center" caption}

- Selecciona **IRIS+™ Event Monitoring**.
  Habilita los siguientes campos según sea necesario:
  - **Show Live Overlay**
  - **Show Playback Overly** (reproducción)

![](https://api.archbee.com/api/optimize/0qwUIPTrS6-vWLcHFHOFA-KeTm517srZJ_kKJ6XbLVp-20240801-132539.png)

# Configurar evento y alarma predeterminados de IRIS+™ en Milestone XProtect

La configuración por defecto descrita en esta sección permite que **cada evento** enviado desde IRIS+™ se reporte como una alarma en Milestone XProtect Smart Client.

El flujo de disparo es:

::Image[]{src="https://api.archbee.com/api/optimize/JzHznijrWxPLJLDUaV5A1/GKULrESfspU1ZV1QgyXUQ_image.png" size="88" width="497" height="68" caption="Flujo de disparo" position="flex-start"}

## Definir el evento analítico de IRIS+™ en XProtect (Analytics Event)

Para definir un evento analítico de IRIS+™ en XProtect, realiza lo siguiente:

1. En el árbol de navegación del sitio en XProtect Management Client, navega a **Rules and Events** (o, en XProtect Management Application, a **Events and Output**) y selecciona **Analytics Events**.

2. Haz clic derecho en **Analytics Events** y selecciona **Add New**.

::Image[]{src="https://api.archbee.com/api/optimize/0qwUIPTrS6-vWLcHFHOFA-BNA7BWkyfwNtX6rs7L5dP-20240801-133305.png" signedSrc size="50" width="532" height="393" position="center" caption}

3. En la sección **Properties**, en el campo **Name**, ingresa **IRIS+ Event**.

::Image[]{src="https://api.archbee.com/api/optimize/0qwUIPTrS6-vWLcHFHOFA-NHquq2KqgdEBeWtIeywrY-20240801-133625.png" signedSrc size="50" width="691" height="309" position="center" caption}

## Definir la alarma IRIS+™ en Milestone XProtect

Para definir una alarma IRIS+™ en Milestone XProtect, realiza lo siguiente:

1. En el árbol de navegación del sitio, expande **Alarms** y selecciona **Alarm Data Settings**.
2. Selecciona la pestaña **Alarm List Configuration**.
3. Asegúrate de que al menos los siguientes campos estén incluidos en las columnas seleccionadas:
   - **Time**
   - **Source**
   - **Tag**
   - **Message**

::Image[]{src="https://api.archbee.com/api/optimize/0qwUIPTrS6-vWLcHFHOFA-JSTr8Q71JWWarqvgeAzkt-20240801-134037.png" signedSrc size="90" width="784" height="653" position="center" caption}

4. En el árbol de navegación del sitio, expande **Alarms** y selecciona **Alarm Definitions**.
5. Haz clic derecho en **Alarm Definitions** y selecciona **Add New**.

::Image[]{src="https://api.archbee.com/api/optimize/0qwUIPTrS6-vWLcHFHOFA-yp5yPI7WYCeIB-Awbf4kJ-20240801-134307.png" signedSrc size="80" width="730" height="655" position="center" caption}

6. En **Alarm Definition Information**, ingresa lo siguiente:

- **Name**: IRIS+ Alarm
- **Triggering event**: selecciona:
  1. **Analytics Events** en la primera línea, seguido de
  2. **IRIS+ Event** en la segunda línea

- **Sources**: haz clic en **Select**
  1. En la ventana **Select Sources** que se abre, abre la pestaña **Servers**, elige **All cameras** y agrégalo a la lista **Selected**.

::Image[]{src="https://api.archbee.com/api/optimize/0qwUIPTrS6-vWLcHFHOFA-SSNsp1GZ2YD9WIASCQ_YH-20240801-135650.png" signedSrc size="60" width="610" height="347" position="center" caption}

Ejemplo:

![](https://api.archbee.com/api/optimize/0qwUIPTrS6-vWLcHFHOFA-TOh-AsfHOXGsywH3oaie8-20240801-135742.png)

7. Sal de la pantalla y selecciona **Save** cuando se te solicite.

## Reiniciar el servicio Milestone XProtect Event Server

**Antes de empezar**  
Para que la configuración surta efecto, debes reiniciar el servicio **Milestone XProtect Event Server** en las PC correspondientes.

::Image[]{src="https://api.archbee.com/api/optimize/0qwUIPTrS6-vWLcHFHOFA-9gr0FlTp6hF_VVtLHWuPC-20240801-140007.png" signedSrc size="50" width="413" height="265" position="center" caption}

# Configurar y visualizar alarmas en Milestone XProtect Smart Client

**Antes de empezar**  
Los siguientes pasos explican cómo visualizar las alarmas de IRIS+™ en la aplicación Milestone XProtect Smart Client.

**Para configurar y ver alarmas IRIS+™ en Milestone XProtect Smart Client, realiza lo siguiente:**

1. Abre la aplicación **Milestone XProtect Smart Client**.
2. Define una vista, de la siguiente manera:
   - Selecciona la pestaña **Live** en el lado izquierdo de la ventana.
   - Haz clic en el botón **Setup** en el lado derecho de la ventana.
   - Define un nuevo grupo usando el ícono **New Group**.
   - Haz clic derecho sobre el grupo recién creado y define una nueva vista; por ejemplo, *(1 + 2\*)*. Asegúrate de seleccionar una vista lo suficientemente amplia para contener la lista de alarmas.
   - En **System Overview**, arrastra el elemento **Alarm List** hacia la parte amplia de tu nueva vista.

     *Nota*: puedes cambiar el orden de las columnas de **Alarm List**. Se recomienda mover la columna **Tag** hacia la derecha para que su valor sea visible, ya que contiene la descripción del evento.

   - En **System Overview**, expande la lista de cámaras y arrastra las cámaras correspondientes a las vistas restantes.

3. En la lista de alarmas, revisa la columna **Tag**, que contiene la descripción del evento analítico (por ejemplo, “Vehicle moving in an area”). Si la columna **Tag** no está disponible, haz clic derecho en el encabezado de la tabla para agregarla. Si aun así no puedes agregarla en Milestone XProtect Corporate, consulta la sección **Alarm Data Settings** en XProtect Management Client descrita anteriormente.

## Configuración del overlay de eventos

Para sincronizar correctamente los tiempos del overlay y de la alarma, realiza lo siguiente:

1. Abre **Milestone XProtect Smart Client**.
2. Selecciona **Settings**.

::Image[]{src="https://api.archbee.com/api/optimize/0qwUIPTrS6-vWLcHFHOFA-QURhnqF4-WFW_VaCQCiIh-20240801-132425.png" signedSrc size="46" width="341" height="281" position="center" caption}

3. Selecciona **Alarm Manager**.
4. Establece el valor **Start video playback** en **8 seconds before the alarm** (8 segundos antes de la alarma).

![](https://api.archbee.com/api/optimize/0qwUIPTrS6-vWLcHFHOFA-4_0BBWBNkuSST8c8FRQox-20240801-141858.png)

# Disparar acciones específicas con Milestone XProtect Corporate

**Antes de empezar**

Esta sección explica cómo manejar escenarios más avanzados para disparar una acción cuando ocurre un evento. Esta capacidad está disponible en **Milestone XProtect Corporate Edition** y se logra enlazando el evento analítico con un **user-defined event** y una **user-defined rule** de XProtect.

El flujo de disparo es:

![](https://api.archbee.com/api/optimize/JzHznijrWxPLJLDUaV5A1/xbwJEN9HcJBMe1yI5eLjA_image.png)

Para configurar IRIS+™ y Milestone XProtect Corporate para el disparo de acciones, realiza lo siguiente:

1. En el portal IRIS+™, selecciona la cámara correspondiente y luego selecciona la regla de detección correspondiente.
2. Define un **External ID** para la regla; se utilizará en la configuración de Milestone XProtect.

![](https://api.archbee.com/api/optimize/0qwUIPTrS6-vWLcHFHOFA-s1EpvzIrzleVUQm44G4pq-20240801-142836.png)

3. En el árbol de navegación de Milestone XProtect Management Client, selecciona **Analytics Events**, haz clic derecho y selecciona **Add New**.
4. Ingresa el nombre idéntico al **External ID** definido en IRIS+™. En este ejemplo: *code1*.

![](https://api.archbee.com/api/optimize/0qwUIPTrS6-vWLcHFHOFA-unt6KB-DU8bocOR4i4K_B-20240801-143326.png)

5. En el árbol de navegación de Milestone XProtect Management Client, selecciona **User-defined Events**, haz clic derecho y selecciona **Add User-defined Event**.
6. Ingresa un nombre para el nuevo User-defined Event y guarda.

![](https://api.archbee.com/api/optimize/0qwUIPTrS6-vWLcHFHOFA-LP-pdQl6TSnPfrBn7MzfZ-20240801-143750.png)

7. Agrega una nueva alarma que enlace el **Analytics Event** y el **User-defined Event**. En el árbol de navegación de Milestone XProtect Management Client, selecciona **Alarm Definitions**, haz clic derecho y selecciona **Add New**.

8. En **Alarm Definition Information**, ingresa lo siguiente:

- **Name**: un nombre significativo
- **Triggering event**: selecciona los Analytics Events definidos en los pasos anteriores (*code1*)
- **Sources**: haz clic en **Select**; en la ventana **Select Sources**, abre la pestaña **Servers**, elige la(s) cámara(s) y agrégalas a la lista **Selected**
- **Events triggered by alarm**: selecciona el user-defined event definido en los pasos anteriores (*myEvent*)

![](https://api.archbee.com/api/optimize/0qwUIPTrS6-vWLcHFHOFA-EbycIWgF5mhsVrmjwzADv-20240801-150152.png)

9. Finaliza la definición de la alarma, sal de la pantalla y selecciona **Save** cuando se te solicite.
10. Después de completar los pasos anteriores, reinicia el servicio **Milestone XProtect Event Server** para que la configuración surta efecto.

Ejemplo:

![](https://api.archbee.com/api/optimize/0qwUIPTrS6-vWLcHFHOFA-XiPTWGfd84to-J9Z0wbb8-20240801-150301.png)

# Configuración de eventos de salud (Health events)

Esta sección explica cómo configurar eventos de salud en Milestone XProtect. Hay dos opciones para recibir eventos de salud en Milestone:

## Recibir todos los eventos de salud

Esta opción permite recibir todos los eventos de salud de IRIS+™ como un solo tipo de evento de salud (genérico) en Milestone XProtect. Realiza lo siguiente:

1. Agrega un nuevo **Analytics Event** en Milestone XProtect Management Client.
2. Establece el nombre como **IRIS+ Health Event**.

![](https://api.archbee.com/api/optimize/0qwUIPTrS6-vWLcHFHOFA-IgZcD4jzt7IO4ZtBqDXVV-20240801-150520.png)

3. Agrega una nueva **Alarm**, que será disparada por **IRIS+ Health Event**.

4. En **Alarm Definition Information**, ingresa lo siguiente:

- **Name**: IRIS+ Health Alarm
- **Triggering event**: selecciona **Analytics Events** en la lista superior y **IRIS+ Health Event** en la lista inferior, como se muestra
- **Sources**: haz clic en **Select**; en la ventana **Select Sources**, abre la pestaña **Servers**, elige **All cameras** y agrégalo a la lista **Selected**
- Sal de la pantalla y selecciona **Save** cuando se te solicite

![](https://api.archbee.com/api/optimize/0qwUIPTrS6-vWLcHFHOFA-L_LK0NNBGbxB14z_c1TA6-20240801-150850.png)

## Recibir eventos de salud definidos por el usuario

Esta opción permite recibir eventos de salud específicos de IRIS+™ según la preferencia del usuario.

Se puede configurar uno o más de los siguientes eventos en Milestone XProtect Management Client, y las alarmas asociadas se mostrarán en Milestone XProtect Smart Client:

1. Communication error  
2. Camera error  
3. Source error  
4. Unsupported URI  
5. Unsupported video codec  
   - Descripción (texto mostrado): “Unsupported video codec, change the source stream configuration” — Códec de video no soportado; cambia la configuración del stream de origen.
6. Unsupported video resolution  
   - Descripción (texto mostrado): “Unsupported video resolution, change the source stream configuration” — Resolución de video no soportada; cambia la configuración del stream de origen.
7. Large time gap in the stream  
   - Descripción (texto mostrado): “Large time gap in the stream, check the source stream” — Brecha de tiempo grande en el stream; revisa la fuente de video.
8. Critical framerate  
9. High framerate  
10. Low framerate  
11. Image blocked  
12. Image dark  
13. Image saturated  
14. Suspended sensor  
    - Descripción (texto mostrado): “Suspended (banned) sensor - when it is detached from the appliance, the sensor configuration and rules still exist, the sensor is not connected to any appliance” — Sensor suspendido (bloqueado): al separarlo del appliance, la configuración y reglas del sensor permanecen, pero el sensor no está conectado a ningún appliance.
15. Disabled sensor  
    - Descripción (texto mostrado): “The sensor is disabled by the user (or by Arm/Disarm command)” — El sensor fue deshabilitado por el usuario (o por un comando Arm/Disarm).
16. Inactive sensor  
    - Descripción (texto mostrado): “The sensor is not active due to user action (enable/disable, attach/detach)” — El sensor no está activo por acción del usuario (enable/disable, attach/detach).
17. Sensor active error  
    - Descripción (texto mostrado): “The sensor is enabled by the user, active, and in error state” — El sensor está habilitado por el usuario, activo y en estado de error.
18. Sensor active warning  
    - Descripción (texto mostrado): “The sensor is enabled by the user, active, and in a warning state” — El sensor está habilitado por el usuario, activo y en estado de advertencia.
19. ONVIF error  
    - Descripción (texto mostrado): “ONVIF error, contact Irisity customer support” — Error ONVIF; contacta a soporte de Irisity.
20. ONVIF not reachable  
    - Descripción (texto mostrado): “ONVIF host not reachable, check the address and user\password” — Host ONVIF inaccesible; verifica la dirección y usuario\contraseña.
21. RTSP authentication error  
    - Descripción (texto mostrado): “RTSP authentication error, check the user and password” — Error de autenticación RTSP; verifica usuario y contraseña.
22. RTSP not reachable  
    - Descripción (texto mostrado): “RTSP host not reachable, check the host and port address and try toggling the multicast support setting” — Host RTSP inaccesible; verifica host y puerto e intenta alternar el ajuste de soporte multicast.
23. RTSP stream issue  
    - Descripción (texto mostrado): “RTSP stream issue, try opening with VLC player” — Problema con el stream RTSP; intenta abrirlo con VLC.
24. RTSP timeout  
    - Descripción (texto mostrado): “RTSP timeout, try toggling the multicast support setting” — Timeout RTSP; intenta alternar el ajuste de soporte multicast.
25. Clip error  
    - Descripción (texto mostrado): “Failed to download clip, check the path” — No se pudo descargar el clip; verifica la ruta.
26. Insufficient Calibration  
    - Descripción (texto mostrado): “Insufficient auto-calibration” — Auto-calibración insuficiente.

La descripción del evento de salud se mostrará en el campo **Tag** de la lista de alarmas en Milestone XProtect Smart Client. Será uno de los eventos definidos por el usuario soportados listados anteriormente.

El siguiente ejemplo muestra cómo agregar un evento de salud definido por el usuario:

- Agrega un nuevo **Analytics Event** en Milestone XProtect Management Client. El nombre debe establecerse como uno de los eventos listados anteriormente.

![](https://api.archbee.com/api/optimize/0qwUIPTrS6-vWLcHFHOFA-d4hfVU5i51gK6owncQd2c-20240801-151453.png)

- Agrega un nuevo **User-defined Event** en Milestone XProtect Management Client:

![](https://api.archbee.com/api/optimize/0qwUIPTrS6-vWLcHFHOFA-eBmaMcXJg0SjitmMAkqiQ-20240801-151622.png)

- Agrega una nueva **Alarm**, enlazando el **Analytics Event** y el **User-defined Event**.

::Image[]{src="https://api.archbee.com/api/optimize/0qwUIPTrS6-vWLcHFHOFA-k75kACi1wH4Qk1skHSLkq-20240801-151826.png" signedSrc size="64" width="436" height="176" position="center" caption}

- En **Alarm Definition Information**, ingresa lo siguiente (para este ejemplo específico):
  - **Name**: Communication error Alarm
  - **Triggering event**: selecciona **Analytics Events** en la lista superior y **Communication error** en la lista inferior, como se muestra
  - **Sources**: haz clic en **Select**; en la ventana **Select Sources**, abre la pestaña **Servers**, elige **All cameras** y agrégalo a la lista **Selected**
  - **Events triggered by alarm**: selecciona el user-defined event agregado arriba: *Communication error User-Defined Event*
  - Sal de la pantalla y selecciona **Save** cuando se te solicite

::Image[]{src="https://api.archbee.com/api/optimize/0qwUIPTrS6-vWLcHFHOFA-Izvc-c44glRjR9UIs-Zg4-20240801-152037.png" signedSrc size="90" width="548" height="622" position="center" caption}

- Para ambas opciones:
  - Los nuevos eventos de salud se muestran en la lista de alarmas con **New State Names**.
  - Los eventos de salud cerrados se muestran en la lista de alarmas con **Closed State Name**.
  - Haz doble clic en un evento de salud para ver detalles adicionales del evento.

![](https://api.archbee.com/api/optimize/JzHznijrWxPLJLDUaV5A1/q3SP8n4heluwD0ldf3lxB_image.png)

![](https://api.archbee.com/api/optimize/JzHznijrWxPLJLDUaV5A1/DxdozMe-TXLB8y5FOpddf_image.png)

# Mostrar snapshot de evento en Milestone XProtect Smart Client

*Solo aplica si “Send event image” está habilitado en la configuración de reglas de IRIS+™.*

Si está marcado **Send event analytics snapshot**, IRIS+™ envía snapshots de eventos a Milestone XProtect.

- **Integración Core**  
  Recibe el snapshot del evento con overlay desde IRIS+™ y lo envía al Milestone XProtect Event Server.

- **Integración Edge**  
  Recibe la imagen del evento y metadatos desde IRIS+™ Edge y, después de dibujar el overlay en la imagen recibida, envía el snapshot al Milestone XProtect Event Server.

El snapshot se muestra en el lado derecho de la reproducción del evento:

![](https://api.archbee.com/api/optimize/JzHznijrWxPLJLDUaV5A1/142Y4oTmCiNOCpnPCE7a5_image.png)

Puedes usar el ajuste **Default for camera title bar** en Milestone XProtect Smart Client para mostrar u ocultar el título de la cámara:

![](https://api.archbee.com/api/optimize/JzHznijrWxPLJLDUaV5A1/lptXDk3F7-EuoXUFNYzBa_image.png)

Para los tipos de reglas **Grouping** y **Occupancy**, el overlay del snapshot y de la reproducción se dibuja usando el overlay “common”, que contiene todos los objetos individuales relevantes del evento. Ver el siguiente ejemplo:

# Mostrar IRIS+™ en Milestone XProtect Smart Client

Una vez instalado el plugin MIP IRIS+™ Event Monitoring, se habilita una pestaña **IRIS+™** en Milestone XProtect Smart Client. Al seleccionarla, se muestra una ventana de inicio de sesión. Inicia sesión en IRIS+™ usando tus credenciales.

# Solución de problemas de la integración del plugin MIP IRIS+™ Event Monitoring

| **Problema** | **Acción correctiva** |
| --- | --- |
| Milestone XProtect Management Client<br />**IRIS+™ no se muestra bajo el nodo de MIP plugins en Milestone XProtect Management Client** | Verifica que el plugin MIP IRIS+™ Event Monitoring esté instalado |
| **Milestone XProtect Management Client** | Verifica que el servicio **Milestone XProtect Event Server** esté en ejecución |
| Milestone XProtect Smart Client:<br />No hay alarmas analíticas en Milestone XProtect Smart Client |  |
| Milestone XProtect Smart Client:<br />**No hay metadatos (o solo aparecen metadatos parciales) al reproducir video grabado en Smart Client** | Haz clic en el botón **Play** nuevamente, en caso de que no se haya hecho clic la primera vez |
| Milestone XProtect Smart Client:<br />No hay alarmas en la lista de alarmas, el encabezado está en rojo y se muestra un mensaje sobre privilegios de usuario en Milestone XProtect Smart Client | Verifica que el usuario conectado a Milestone XProtect Smart Client tenga privilegios suficientes. En **XProtect Management Client**, revisa las propiedades del usuario bajo **Advanced Configuration > Users** |
| Milestone XProtect Smart Client:<br />Ninguna de las acciones anteriores ayudó; aún no puedes ver eventos analíticos en Milestone XProtect. Sigue las instrucciones bajo la columna **Acción correctiva** a la derecha para obtener los archivos de log de Milestone MIP. |  |
| Milestone XProtect Smart Client:<br />No existe la posibilidad de agregar la columna **Tag** a la lista de alarmas |  |
| Milestone XProtect Smart Client:<br />Ocurre un error al abrir Milestone XProtect Smart Client en Windows Server 2008 |  |
| El video grabado no está sincronizado con los overlays de metadatos de objetos | Configura el mismo endpoint NTP en el dispositivo Edge, en las cámaras y en Milestone XProtect |

# Contactar soporte de Irisity

Soporte al cliente: [**technicalsupport@irisity.com**](mailto\:technicalsupport@irisity.com)
