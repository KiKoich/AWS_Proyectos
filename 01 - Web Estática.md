# 1. **Crear una página web estática con S3 y CloudFront**
- **Descripción**: Aloja una página web estática en Amazon S3 y distribúyela globalmente usando Amazon CloudFront.
- **Servicios usados**: S3, CloudFront.
- **Habilidades aprendidas**: Almacenamiento estático, CDN, permisos de S3.
- **Guía**: Sube archivos HTML, CSS y JS a un bucket de S3, habilita el hosting estático y configura CloudFront para mejorar el rendimiento.
### **Requisitos previos**:
1. **Cuenta de AWS**: Si no tienes una, créala en [aws.amazon.com](https://aws.amazon.com/). Asegúrate de usar el **Free Tier** para evitar cargos.
2. **Archivos de la página web**: Prepara archivos HTML, CSS y JavaScript para tu página web estática (puedes usar una plantilla gratuita o crear una página simple).
### **Paso a paso**:
#### **Paso 1: Crear un bucket en Amazon S3**
1. Inicia sesión en la **Consola de AWS**.
2. Busca **S3** en la barra de búsqueda y selecciona el servicio.
3. Haz clic en **"Create bucket"**.
    - **Bucket name**: Elige un nombre único (por ejemplo, `mi-pagina-web-2023`).
    - **Region**: Selecciona una región cercana a ti (por ejemplo, `us-east-1`).
    - **Block Public Access settings**: Desmarca la opción **"Block all public access"** y marca la casilla de confirmación.
    - **Bucket Versioning**: Opcional, puedes habilitarlo si quieres mantener versiones de tus archivos.
    - **Tags**: Opcional, añade etiquetas si lo deseas.
    - Haz clic en **"Create bucket"**.
#### **Paso 2: Subir los archivos de la página web al bucket**
1. En la lista de buckets, selecciona el bucket que acabas de crear.
2. Haz clic en **"Upload"**.
    - Arrastra y suelta los archivos de tu página web (HTML, CSS, JS, imágenes, etc.).
    - Asegúrate de que el archivo principal de tu página se llame **`index.html`**.
3. Haz clic en **"Upload"** para subir los archivos.
_index.html_
```html
<html>
    <head>
        <meta http-equiv="Content-Type" content="text/html; charset=utf-8"/>
        <title>Proyecto de Web Estática en AWS</title>
    </head>
    <body>
        <h1>AWS - Proyecto 1 - Web Estática</h1>
        <p>Contenido de la web estática</p>
    </body>
</html>
```
#### **Paso 3: Habilitar el hosting estático en S3**
1. Dentro de tu bucket, ve a la pestaña **"Properties"**.
2. Desplázate hacia abajo y busca la sección **"Static website hosting"**.
3. Haz clic en **"Edit"**.
    - Selecciona **"Enable"**.
    - En **"Index document"**, escribe `index.html`.
    - (Opcional) En **"Error document"**, puedes escribir `error.html` si tienes una página de error personalizada.
4. Haz clic en **"Save changes"**.
#### **Paso 4: Hacer público el bucket**
1. Ve a la pestaña **"Permissions"** de tu bucket.
2. En la sección **"Bucket Policy"**, haz clic en **"Edit"**.
3. Copia y pega la siguiente política (reemplaza `nombre-del-bucket` con el nombre de tu bucket):
    json
    Copy
    {
        "Version": "2012-10-17",
        "Statement": [
            {
                "Sid": "PublicReadGetObject",
                "Effect": "Allow",
                "Principal": "*",
                "Action": "s3:GetObject",
                "Resource": "arn:aws:s3:::nombre-del-bucket/*"
            }
        ]
    }
4. Haz clic en **"Save changes"**.
#### **Paso 5: Configurar Amazon CloudFront**
1. Busca **CloudFront** en la barra de búsqueda de la consola de AWS y selecciona el servicio.
2. Haz clic en **"Create distribution"**.
3. En **"Origin domain"**, selecciona tu bucket de S3 (aparecerá en la lista).
4. En **"Default cache behavior"**, deja las opciones predeterminadas.
5. En "Web Application Firewall (WAF)" no activamos protecciones de seguridad.
6. En **"Settings"**:
    - **Price class**: Selecciona **"Use only North America and Europe"** para mantenerlo gratuito.
    - **Default root object**: Escribe `index.html`.
7. Haz clic en **"Create distribution"**.
8. Espera a que el estado de la distribución cambie a **"Deployed"** (puede tardar unos minutos).
#### **Paso 6: Acceder a tu página web**
1. Una vez que la distribución de CloudFront esté desplegada, copia el **Domain Name** (por ejemplo, `d1234abcd.cloudfront.net`).
2. Pega el dominio en tu navegador y verás tu página web estática.
**Nota** - CloudFront tiene en cache 24 horas el fichero y no recarga nuevos cambios en dicho tiempo. Si quieres hacerlo inmediatamente tienes que irte a la distribución de CloudFront e invalidar los ficheros que quieras recargar. Cuidado!!!, solo tienes 1000 al mes gratis.
### **Verificación y pruebas**:
- **Prueba de acceso**: Asegúrate de que la página web se carga correctamente.
- **Prueba de velocidad**: CloudFront mejora el rendimiento al distribuir el contenido globalmente.
- **Prueba de permisos**: Verifica que el bucket de S3 y CloudFront sean accesibles públicamente.
### **Costos (Free Tier)**:
- **S3**: Los primeros 5 GB de almacenamiento y 20,000 solicitudes GET son gratuitos.
- **CloudFront**: Los primeros 1 TB de transferencia de datos y 10,000 solicitudes HTTP/HTTPS son gratuitos.
### **Consejos adicionales**:
- **Dominio personalizado**: Si tienes un dominio, puedes configurarlo en CloudFront para acceder a tu página web con una URL personalizada.
- **Seguridad**: Si quieres añadir HTTPS, CloudFront ya lo incluye de forma gratuita.
- **Monitoreo**: Usa **CloudWatch** para monitorear el tráfico y el rendimiento de tu sitio.
