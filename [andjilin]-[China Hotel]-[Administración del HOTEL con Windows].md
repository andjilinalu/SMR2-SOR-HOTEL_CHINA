# Proyecto: Gestión de Usuarios y Grupos en el Dominio

## Hotel Gran Descanso Plus – RA2

---

## 🎯 Objetivo General

Implementar una **gestión completa de identidades** en el dominio del *Hotel Gran Descanso Plus*, asegurando seguridad, eficiencia operativa y cumplimiento del **Resultado de Aprendizaje RA2** mediante la correcta administración de **usuarios, grupos, equipos y perfiles**.

---

# 🧩 Relación con Resultados de Aprendizaje

**RA2.** Gestiona usuarios y grupos de sistemas operativos en red, interpretando especificaciones y aplicando herramientas del sistema.

Se cubren los criterios **CE a → CE i** a través de las tareas T1–T9.

---

# 🏨 Contexto del Proyecto

El hotel necesita una estructura de dominio avanzada para gestionar el acceso diferenciado de:

* Recepción
* Administración
* Limpieza
* Cocina
* Mantenimiento
* Dirección

La solución se basa en **Active Directory** como servicio de directorio central.

---

# 🧑‍💼 Tarea 1: Creación de Cuentas Operativas (CE a)

## Objetivo

Configurar y gestionar cuentas de usuario para empleados temporales y operativos.

## Pasos

1. Abrir **Usuarios y equipos de Active Directory**.
2. Navegar a la OU correspondiente.
3. Clic derecho → **Nuevo → Usuario**.
4. Crear usuarios (ejemplo):

```
recep01_temp
camarero01_temp
```
<img width="433" height="376" alt="image" src="https://github.com/user-attachments/assets/66bb0ba9-e0bb-455f-8b28-b887947a7185" />

5. Configurar contraseña inicial.
6. En **Propiedades → Cuenta**:
   * Deseleccionamos **El usuario debe cambiar la contraseña en el siguiente inicio de sesión.**.
   * Definir **caducidad de contraseña(nunca expira)**.
   * Añadir contraseña segura **Pa55w.rdPa55w.rd**.
<img width="438" height="374" alt="image" src="https://github.com/user-attachments/assets/2753a206-d62d-4553-9b00-a79bb2da6af8"/>

7. Terminamos creando varios usuarios:
<img width="432" height="376" alt="image" src="https://github.com/user-attachments/assets/fedb3655-f96d-4fea-9fe4-7edfc18c90c7" />
<img width="435" height="372" alt="image" src="https://github.com/user-attachments/assets/87f76c5a-c118-43e8-86d2-2afe5f22ad03" />


✅ Criterio cubierto: CE a

---

# 🖥️ Tarea 2: Definición de Entornos de Trabajo (CE b)

## Objetivo

Gestionar perfiles de usuario según el rol.

### Perfil Controller (Finanzas)

1. Usuario: `controller_finanzas`.
<img width="427" height="371" alt="image" src="https://github.com/user-attachments/assets/417c946d-4fe3-4206-ab43-1f10f81d9def" />
<img width="436" height="377" alt="image" src="https://github.com/user-attachments/assets/d06f8863-9b8a-4937-9326-e97e4c287801" />

3. Acceso restringido a carpetas financieras.
4. Escritorio con:

   * Software contable
   * Sin acceso a PMS

controller.finanzas:
❌ No accede a carpetas de recepción
❌ No accede a PMS
✅ Solo accede a:
Carpetas financieras
Herramientas contables
Esto se controla siempre con grupos, nunca directamente con usuarios.

El usuario controller.finanzas dispone de un entorno de trabajo restringido mediante pertenencia a grupos de seguridad, garantizando el principio de mínimo privilegio y la protección de la información financiera del hotel
Es recomendable crear un grupo para finanzas, simplemente habria que darle a Usuarios -> Nuevo -> Grupo, añadir un nombre y entrar en miembros y agregar el usuario que creamos posteriormente.
  
<img width="431" height="451" alt="image" src="https://github.com/user-attachments/assets/e8b8dc3b-4937-4959-b298-73ee89d36aac" />


✅ Criterio cubierto: CE b

---

# 🖨️ Tarea 3: Registro de Dispositivos (CE c)

## Objetivo

Gestionar cuentas de equipo del dominio.

## Pasos

1. Unir equipos al dominio:

```
tailwindtraders.internal
```
<img width="357" height="62" alt="image" src="https://github.com/user-attachments/assets/1d32e98d-ffb4-4007-829c-d063e9e4cd74" />


<img width="364" height="60" alt="image" src="https://github.com/user-attachments/assets/fd0b00d0-7bdd-4784-92a1-85cb8aecb3d9" />


✅ Criterio cubierto: CE c

---

# 🧱 Tarea 4: Diseño de la Estructura de Acceso (CE d)

## Objetivo

Definir correctamente tipos y ámbitos de grupos.

## Creamos un grupo para cada apartado del hotel

<img width="359" height="80" alt="image" src="https://github.com/user-attachments/assets/43cdeaa5-a502-4305-a85c-785f4dae0a0c" />

```
GR_Recepción
GR_Finanzas
GR_Mantenimiento
GR_Restaurante
```


✅ Criterio cubierto: CE d

---

# 👥 Tarea 5: Implementación de Grupos Funcionales (CE e)

## Objetivo

Creamos y gestionamos los grupos
Para que sea funcional de verdad habría que crear los archivos y darles ciertos permisos a cada grupo.
Por ejemplo: Solo el Grupo Finanzas puede acceder al archivo "Cuentas Financieras"
## Pasos

- Añadimos a cada usuario con los permisos que querramos para cada apartado


```
camarero01_temp
recep01_temp
mant01_temp
Controller.Finanzas
```

2. Tipo: Seguridad
3. Ámbito: Global
Debería de quedarnos algo así:
<img width="751" height="532" alt="image" src="https://github.com/user-attachments/assets/ea175640-7b51-4677-a200-7e1e27664e78" />

Lo que he hecho aparte es crear una UO para tenerlo todo más ordenado, poniendo en Trabajadores-Hotel los grupos de trabajos, y creando otra carpeta para añadir los trabajadores.
✅ Criterio cubierto: CE e

---

# 🔑 Tarea 6: Asignación de Permisos (CE f)

## Objetivo

Gestionar la pertenencia de usuarios a grupos.

## Pasos

1. Creamos la carpeta confindencial y una vez creada vamos a:
   Propiedades -> Seguridad
2. Una vez ahí, tendrémos que deshabilitar la herencia y permitir solo al grupo que queramos el acceso a esa carpeta confidencial
   <img width="767" height="520" alt="image" src="https://github.com/user-attachments/assets/b4cdbf8b-f960-4792-9cef-1d2697203f27" />
3. Añadiremos el grupo que tendrá permisos para esa carpeta
   <img width="354" height="445" alt="image" src="https://github.com/user-attachments/assets/09e72310-4538-4dfa-8473-2d27dca3adfa" />


```
GR_Finanzas -> Carpeta "Finanzas"
```

3. Asignar permisos y recursos compartidos a grupos.
También crearé unos usuarios invitados para los ordenadores, para el caso en el que alguien quiera realizar algo sin ser un trabajador.
<img width="428" height="372" alt="image" src="https://github.com/user-attachments/assets/6b45d703-dad0-43d5-8b6c-ad062d433fc5" />


La contraseña de estos se les dará a los usuarios que quieran utilizarlo y seran cambiados periodicamente.
```
Guest01 -> Pass: akL%w?q315Xm...
```

✅ Criterio cubierto: CE f

---

# 🔍 Tarea 7: Auditoría de Seguridad Inicial (CE g)

## Objetivo

Identificar usuarios y grupos especiales.

## Pasos

- Revisar cuentas:
   * Invitados (Guest) → Deshabilitar
    <img width="359" height="455" alt="image" src="https://github.com/user-attachments/assets/902b6164-1b64-4467-8f7b-436e5f819a16" />

En un entorno real y no ficticio como este tendremos que revisar que solo puedan acceder usuarios autorizados y deshabilitar todos los que no pertenezcan a este uno.

✅ Criterio cubierto: CE g

---

# 🚀 Tarea 8: Movilidad y Despliegue de Perfiles (CE h)

## Objetivo

Planificar perfiles móviles.

## Pasos
Previamente crearemos Usuarios Autentificados y Administradores.
<img width="706" height="376" alt="image" src="https://github.com/user-attachments/assets/ec1c4dc9-85a1-40e8-80fd-a9bd9a420718" />
<img width="708" height="505" alt="image" src="https://github.com/user-attachments/assets/ffa575cb-bfaf-4eb3-85a8-f3cc9d2dfba1" />
Y sus debidos grupos:
<img width="442" height="211" alt="image" src="https://github.com/user-attachments/assets/de4336b8-cdae-4992-b95c-b62d95e16d17" />
<img width="429" height="275" alt="image" src="https://github.com/user-attachments/assets/a436b1e2-c0bd-4dd9-b576-2f207728eab3" />


1. En el explorador de archivos vamos al disco (C:\) y creamos una carpeta:
  <img width="740" height="575" alt="image" src="https://github.com/user-attachments/assets/98d5ba18-b214-495c-a3b8-042371454fce" />
2. Vamos a propiedades, pestaña compartir y en uso compartido avanzado marcamos compartir esta carpeta.
<img width="368" height="486" alt="image" src="https://github.com/user-attachments/assets/d8f12ba9-aad1-4a93-8c54-4b7aa0c34678" />
3. En permisos eliminamos Everyone (Todos) y añadimos Usuarios Autentificados y Administradores.
   <img width="364" height="227" alt="image" src="https://github.com/user-attachments/assets/3cacdfdd-13a9-4ed4-ae0a-48aba1e06fb7" />

Se planificaron e implementaron perfiles móviles creando un recurso compartido en el servidor, configurando los permisos adecuados y asignando la ruta de perfil a los usuarios que requieren movilidad entre distintos equipos

✅ Criterio cubierto: CE h

---

# 🛠️ Tarea 9: Aplicación de Herramientas de Administración (CE i)

## Objetivo

Utilizar herramientas del sistema operativo.

## Herramientas usadas

* Usuarios y equipos de Active Directory
* Centro administrativo de AD
* GPO
* PowerShell (opcional):

```powershell
New-ADUser
Add-ADGroupMember
```

✅ Criterio cubierto: CE i

---

# 📊 Diagrama de Flujo General

```mermaid
flowchart TD
A[Crear usuarios] --> B[Crear grupos]
B --> C[Asignar usuarios a grupos]
C --> D[Configurar perfiles]
D --> E[Asignar permisos]
E --> F[Auditoría y seguridad]
```

