# ?? GU�A COMPLETA - RENDER.COM CON DOCKER

## ? CONFIGURACI�N CORRECTA

Render **S� soporta .NET PERO SOLO con Docker**.

Tu proyecto **YA TIENE** todos los Dockerfiles configurados ?

---

## ??? TU BASE DE DATOS (Ya configurada)

```
Server:   db31651.public.databaseasp.net
Database: db31651
Usuario:  db31651
Password: prueba2020d
```

? Ya est� en `Shared.Data/DatabaseConfig.cs`

---

## ?? PASOS PARA PUBLICAR (50 minutos)

### PASO 1: Subir a GitHub (si no lo has hecho)

```powershell
cd "D:\Jossue\Desktop\RETO 3\BACK\V1\Microservicios"

# Crea repositorio en GitHub: https://github.com/new
# Nombre: hotel-microservices

.\init-github.ps1 -RepoUrl "https://github.com/PierreMV04/HotelCampestre-Microservicios.git"
```

---

### PASO 2: Crear cuenta en Render

1. Ve a: **https://render.com**
2. **"Get Started"**
3. **"Sign up with GitHub"**
4. Autoriza Render
5. ? NO pide tarjeta de cr�dito

---

### PASO 3: Crear los 5 Web Services con Docker

Vamos a crear un servicio por cada microservicio.

#### **3.1 ApiGateway**

```
1. Dashboard ? "New +" ? "Web Service"
2. "Connect Repository" ? Selecciona: hotel-microservices
3. Configurar:

   Name:                apigateway
   Region:              Oregon (US West)
   Branch:              main
   Root Directory:      (dejar vac�o - usa ra�z del repo)
   Runtime:             Docker
   Dockerfile Path:     ApiGateway/Dockerfile

4. Instance Type:       Free

5. Environment Variables (Click "Advanced"):

   ASPNETCORE_ENVIRONMENT = Production
   ASPNETCORE_URLS = http://0.0.0.0:$PORT
   JWT_SECRET_KEY = HotelMicroservicesSecretKey2024!@#$%^&*()_+

6. "Create Web Service"
7. ? Espera 5-7 minutos
```

---

#### **3.2 CatalogosService**

```
1. "New +" ? "Web Service"
2. Repo: hotel-microservices
3. Configurar:

   Name:                catalogos-service
   Region:              Oregon (US West)
   Branch:              main
   Root Directory:      (vac�o)
   Runtime:             Docker
   Dockerfile Path:     CatalogosService/Dockerfile

4. Instance Type:       Free

5. Environment Variables:

   ASPNETCORE_ENVIRONMENT = Production
   ASPNETCORE_URLS = http://0.0.0.0:$PORT
   JWT_SECRET_KEY = HotelMicroservicesSecretKey2024!@#$%^&*()_+
   RABBITMQ_URL = 

6. "Create Web Service"
```

---

#### **3.3 HabitacionesService**

```
1. "New +" ? "Web Service"
2. Repo: hotel-microservices
3. Configurar:

   Name:                habitaciones-service
   Region:              Oregon (US West)
   Branch:              main
   Root Directory:      (vac�o)
   Runtime:             Docker
   Dockerfile Path:     HabitacionesService/Dockerfile

4. Instance Type:       Free

5. Environment Variables:

   ASPNETCORE_ENVIRONMENT = Production
   ASPNETCORE_URLS = http://0.0.0.0:$PORT
   JWT_SECRET_KEY = HotelMicroservicesSecretKey2024!@#$%^&*()_+
   RABBITMQ_URL = 

6. "Create Web Service"
```

---

#### **3.4 ReservasService**

```
1. "New +" ? "Web Service"
2. Repo: hotel-microservices
3. Configurar:

   Name:                reservas-service
   Region:              Oregon (US West)
   Branch:              main
   Root Directory:      (vac�o)
   Runtime:             Docker
   Dockerfile Path:     ReservasService/Dockerfile

4. Instance Type:       Free

5. Environment Variables:

   ASPNETCORE_ENVIRONMENT = Production
   ASPNETCORE_URLS = http://0.0.0.0:$PORT
   JWT_SECRET_KEY = HotelMicroservicesSecretKey2024!@#$%^&*()_+
   RABBITMQ_URL = 

6. "Create Web Service"
```

---

#### **3.5 UsuariosPagosService**

```
1. "New +" ? "Web Service"
2. Repo: hotel-microservices
3. Configurar:

   Name:                usuarios-pagos-service
   Region:              Oregon (US West)
   Branch:              main
   Root Directory:      (vac�o)
   Runtime:             Docker
   Dockerfile Path:     UsuariosPagosService/Dockerfile

4. Instance Type:       Free

5. Environment Variables:

   ASPNETCORE_ENVIRONMENT = Production
   ASPNETCORE_URLS = http://0.0.0.0:$PORT
   JWT_SECRET_KEY = HotelMicroservicesSecretKey2024!@#$%^&*()_+
   RABBITMQ_URL = 
   RESERVAS_SERVICE_URL = (dejar vac�o por ahora)

6. "Create Web Service"
```

---

### PASO 4: Copiar URLs generadas

Cuando todos terminen de desplegar (?? verde), copia sus URLs:

```
ApiGateway:           https://apigateway.onrender.com
CatalogosService:     https://catalogos-service.onrender.com
HabitacionesService:  https://habitaciones-service.onrender.com
ReservasService:      https://reservas-service.onrender.com
UsuariosPagosService: https://usuarios-pagos-service.onrender.com
```

---

### PASO 5: Actualizar Variables de Entorno

#### **5.1 Actualizar UsuariosPagosService**

```
1. Click en "usuarios-pagos-service"
2. Sidebar ? "Environment"
3. Edita RESERVAS_SERVICE_URL:

   RESERVAS_SERVICE_URL = https://reservas-service.onrender.com

4. "Save Changes"
5. Render redesplegar� autom�ticamente (2-3 min)
```

---

#### **5.2 Actualizar ApiGateway**

```
1. Click en "apigateway"
2. Sidebar ? "Environment"
3. Agregar estas variables (con TUS URLs reales):

   CATALOGOS_SERVICE_URL = https://catalogos-service.onrender.com
   HABITACIONES_SERVICE_URL = https://habitaciones-service.onrender.com
   RESERVAS_SERVICE_URL = https://reservas-service.onrender.com
   USUARIOS_PAGOS_SERVICE_URL = https://usuarios-pagos-service.onrender.com

4. "Save Changes"
5. Espera redespliegue (2-3 min)
```

---

### PASO 6: Verificar que funciona

```
1. Abre: https://apigateway.onrender.com/swagger

   ? Si tarda 30-60 segundos, es normal (cold start)

2. Prueba endpoint:
   
   GET /api/catalogos/ciudades

3. ? Deber�a retornar tus ciudades de SQL Server
```

---

## ?? SOLUCI�N AL "DORMIDO" (Cold Starts)

Los servicios gratis duermen despu�s de 15 min de inactividad.

### **Soluci�n: UptimeRobot** (Gratis)

```
1. Ve a: https://uptimerobot.com
2. Sign up (gratis - 50 monitores)
3. "Add New Monitor" ? HTTP(s)
4. Configurar para cada servicio:

   Friendly Name:       ApiGateway
   URL or IP:           https://apigateway.onrender.com
   Monitoring Interval: 5 minutes

5. Repetir para los 5 servicios
6. ? Tus servicios se mantendr�n activos 24/7
```

---

## ?? COSTOS

- **Render.com:** $0 USD/mes (plan Free)
- **SQL Server:** Ya tienes (Somee.com)
- **UptimeRobot:** $0 USD/mes (50 monitores gratis)

**Total: $0 USD/mes** ??

---

## ?? TIEMPO ESTIMADO

| Paso | Tiempo |
|------|--------|
| Subir a GitHub | 3 min |
| Crear cuenta Render | 2 min |
| Crear 5 servicios | 20 min |
| Esperar builds Docker | 30 min (paralelo) |
| Actualizar variables | 5 min |
| Verificar | 3 min |
| **TOTAL** | **50 minutos** |

---

## ?? LIMITACIONES DEL PLAN FREE

| Limitaci�n | Detalle | Soluci�n |
|------------|---------|----------|
| **Cold starts** | 30-60 seg despu�s de 15 min | UptimeRobot |
| **512 MB RAM** | Por servicio | Suficiente para tu proyecto |
| **CPU Shared** | Compartido | Normal en plan Free |
| **Build time** | 10 min max | Tus Dockerfiles compilan en ~5 min |

---

## ?? REDEPLOY AUTOM�TICO

Cada vez que hagas `git push`:

```powershell
git add .
git commit -m "Actualizaci�n"
git push
```

Render detectar� el cambio y **redesplegar� autom�ticamente** los servicios.

---

## ?? ARQUITECTURA FINAL

```
Frontend (Angular)
      ?
      ? HTTPS
      ?
API Gateway (Render - Docker)
      ?
??????????????????????????????????
?     ?     ?         ?          ?
Cat�logos Habitac. Reservas Usuarios/Pagos
(Render)  (Render)  (Render)  (Render)
 Docker    Docker    Docker    Docker
?     ?     ?         ?          ?
??????????????????????????????????
            ?
     SQL Server (Somee.com)
   db31651.public.databaseasp.net
```

---

## ?? TROUBLESHOOTING

### "Build failed"

```
1. Ve a Render ? Logs
2. Busca el error
3. Errores comunes:
   - Dockerfile path incorrecto
   - Problemas en dotnet restore
   - Falta alg�n proyecto compartido
```

### "Application failed to start"

```
1. Verifica Environment Variables
2. Aseg�rate de tener: ASPNETCORE_URLS=http://0.0.0.0:$PORT
3. Revisa logs para ver error espec�fico
```

### "Cannot connect to SQL Server"

```
1. Verifica que Somee.com est� activo
2. Prueba conexi�n desde SSMS local
3. Revisa que DatabaseConfig.cs tenga la connection string correcta
```

### "Service is slow / unavailable"

```
1. Es cold start (normal despu�s de 15 min)
2. Espera 30-60 segundos
3. Configura UptimeRobot para mantener activo
```

---

## ?? CONECTAR CON ANGULAR

```typescript
// environment.prod.ts
export const environment = {
  production: true,
  apiUrl: 'https://apigateway.onrender.com',
  endpoints: {
    auth: '/api/usuarios-pagos/auth',
    catalogos: '/api/catalogos',
    habitaciones: '/graphql',
    integracion: '/api/integracion'
  }
};
```

**Implementa loading para cold starts:**

```typescript
// app.component.ts
ngOnInit() {
  // Ping cada 10 minutos para mantener activo
  setInterval(() => {
    this.http.get(`${environment.apiUrl}/health`).subscribe();
  }, 10 * 60 * 1000);
}
```

---

## ? VENTAJAS DE ESTA CONFIGURACI�N

1. ? **Gratis 100%** ($0 USD/mes)
2. ? **Docker** (profesional y portable)
3. ? **SQL Server existente** (datos reales)
4. ? **5 microservicios** funcionando
5. ? **Auto-deploy** desde GitHub
6. ? **SSL/HTTPS** gratis
7. ? **Sin tarjeta** de cr�dito

---

## ?? CHECKLIST

- [ ] C�digo subido a GitHub
- [ ] Cuenta creada en Render
- [ ] 5 servicios creados con Docker
- [ ] URLs p�blicas generadas
- [ ] Variables de entorno configuradas
- [ ] ApiGateway con URLs actualizadas
- [ ] Swagger funcionando
- [ ] Endpoints retornan datos de SQL Server
- [ ] (Opcional) UptimeRobot configurado

---

## ?? DOCUMENTACI�N �TIL

- **Render Docs:** https://render.com/docs
- **Render Docker:** https://render.com/docs/docker
- **Docker Hub:** https://hub.docker.com/_/microsoft-dotnet
- **UptimeRobot:** https://uptimerobot.com

---

## ?? PR�XIMO PASO

```powershell
# Si no has subido el c�digo:
.\init-github.ps1 -RepoUrl "https://github.com/TU_USUARIO/hotel-microservices.git"

# Luego ve a:
start https://render.com
```

---

<div align="center">

# ? **�RENDER + DOCKER CONFIGURADO!** ?

**Tu �nica opci�n GRATIS con .NET en Render**

**50 minutos � $0 USD/mes � Sin tarjeta**

**Con Docker para m�xima compatibilidad**

</div>
