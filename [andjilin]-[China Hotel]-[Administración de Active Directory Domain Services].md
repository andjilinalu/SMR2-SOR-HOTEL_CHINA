# Proyecto Guiado SOR - China Hotel - 2ºSMR

# Preparación

**Objetivo:**  
Configurar un entorno de laboratorio con **Hyper‑V**, un **controlador de dominio (DC)** y un **servidor miembro**, utilizando Windows Server 2022 Evaluation Edition. Incluye instalación, configuración de red y promoción del dominio.

---

## Índice

1. [Instalar Hyper‑V]
2. [Crear máquina virtual del controlador de dominio (TAILWIND-DC1)]
3. [Promocionar TAILWIND-DC1 a controlador de dominio]
4. [Crear servidor miembro (TAILWIND-MBR1)]
5. [Unir TAILWIND-MBR1 al dominio]
6. [Diagramas de flujo]

---

## Instalar Hyper‑V
<img width="418" height="366" alt="image" src="https://github.com/user-attachments/assets/64f5dff2-00cb-4717-9c25-073380fd14da" />


1. Iniciar sesión en Windows 10.
2. Abrimos **Configuración → Sistema → Características opcionales**.
3. Seleccionar **Más funciones de Windows → Hyper‑V**.
4. Aceptar y **reiniciar**.
5. Abrir **Hyper‑V Manager** y fijarlo en la barra de tareas.
6. Configurar rutas por defecto:

   * Máquinas virtuales: `C:\VirtualMachines`
   * VHDs: `C:\VirtualMachines\VHDs`
7. Crear red NAT desde PowerShell (admin):
<img width="1715" height="876" alt="image" src="https://github.com/user-attachments/assets/d48ed155-868e-4b78-a34c-600067144952" />


```
New-VMSwitch -SwitchName "NATSwitch" -SwitchType Internal
New-NetIPAddress -IPAddress 10.10.10.1 -PrefixLength 24 -InterfaceAlias "vEthernet (NATSwitch)"
New-NetNat -Name "NATNetwork" –InternalIPInterfaceAddressPrefix "10.10.10.0/24"
```

---

## Crear controlador de dominio

### 1. Crear máquina virtual

1. Hyper‑V → **Nuevo → Máquina virtual**.
2. Nombre: **TAILWIND-DC1**.
3. Generación 2.
4. RAM: **4096 MB**, con memoria dinámica.
5. Red: **NAT**.
6. Disco: usar valores por defecto.
7. Instalar SO desde ISO `SERVER_EVAL_x64FRE_en-us.iso`.
8. Finalizar.

## 2. Configuración inicial

1. Desactivar checkpoints automáticos.
2. Iniciar VM y arrancar desde ISO.
3. Instalar **Windows Server 2022 (Experiencia de escritorio)**.
4. Contraseña del administrador: `Pa55w.rdPa55w.rd`.
5. Configurar IP:

```
IP: 10.10.10.10
Máscara: 255.255.255.0
Puerta de enlace: 10.10.10.1
DNS Primario: 1.1.1.1
DNS Secundario: 8.8.8.8
```
<img width="397" height="452" alt="image" src="https://github.com/user-attachments/assets/a0407afd-509d-46bd-8996-24804d07dad2" />


6. Cambiar nombre del equipo → **TAILWIND-DC1** y reiniciar.
<img width="739" height="330" alt="image" src="https://github.com/user-attachments/assets/1c6bc375-6a4f-4965-a682-6c2b0cf576e4" />

---

## Promocionar TAILWIND-DC1 a Controlador de Dominio

1. Abrir **Administrador del servidor**.
2. **Agregar roles y características**.
3. Seleccionar rol: **Servicios de dominio de Active Directory**.
4. Instalar.
5. Notificación → **Promocionar este servidor a controlador de dominio**.
6. Crear **nuevo bosque** → Dominio raíz: `tailwindtrader.internal`.
7. Contraseña DSRM: `Pa55w.rdPa55w.rd`.
8. Finalizar e instalar (reinicio automático).

---

# Crear servidor miembro

## 1. Crear VM 
1. Hyper‑V → Nueva máquina virtual.
2. Nombre: **TAILWIND-MBR1**.
3. Generación 2.
4. RAM 4096 MB.
5. NATSwitch.
6. Instalar desde ISO.
7. Desactivar checkpoints.

## 2. Configuración inicial

1. Instalar Windows Server.
2. Contraseña admin: `Pa55w.rdPa55w.rd`.
3. Configurar IP:

```
IP: 10.10.10.20
Máscara: 255.255.255.0
Puerta de enlace: 10.10.10.1
DNS Primario: 10.10.10.10
DNS Secundario: 8.8.8.8
```
<img width="398" height="453" alt="image" src="https://github.com/user-attachments/assets/7dc3d0be-96e5-4b3b-aa32-8bd696996d27" />

4. Reiniciar.

---

# Unir TAILWIND-MBR1 al dominio

1. Abrir **Propiedades del sistema**.
2. Miembro de → **Dominio: TAILWINDTRADERS**.
3. Credenciales:

```
Usuario: TAILWINDTRADERS\Administrator
Contraseña: Pa55w.rdPa55w.rd
```
<img width="791" height="466" alt="image" src="https://github.com/user-attachments/assets/0bfbdf84-ce4d-4638-976c-0551d6be4f37" />

4. Mensaje de bienvenida.
<img width="369" height="149" alt="image" src="https://github.com/user-attachments/assets/fd70c9dd-437f-4849-8cdc-8761a156b4dc" />

5. Reiniciar.

---

# Diagramas de flujo

## Flujo: Instalación de Hyper‑V

```mermaid
flowchart TD
    A[Inicio] --> B[Activar Hyper-V]
    B --> C[Reiniciar]
    C --> D[Configurar rutas de VMs]
    D --> E[Crear red NAT]
    E --> F[Fin]
```

## Flujo: Promoción a Controlador de Dominio

```mermaid
flowchart TD
    A[Instalar rol AD DS] --> B[Abrir notificación]
    B --> C[Promocionar]
    C --> D[Crear nuevo bosque]
    D --> E[Configurar opciones]
    E --> F[Instalar y reiniciar]
```

## Flujo: Unión de servidor al dominio

```mermaid
flowchart TD
    A[Configurar IP] --> B[Cambiar nombre del equipo]
    B --> C[Unir al dominio]
    C --> D[Ingresar credenciales]
    D --> E[Reinicio]
```

---

# Preparación completada

# Configuración de operaciones de controlador de dominio

# Proyecto: Administración avanzada de Active Directory (AZ-1008)

**Objetivo:** 

Ampliar la infraestructura de **Active Directory** promoviendo un segundo controlador de dominio, transfiriendo roles FSMO y configurando **sitios y subredes** para una correcta replicación y administración del directorio.

---

# Índice

1. [Promover servidor miembro a controlador de dominio]
2. [Transferir roles FSMO (RID Master)
3. [Crear sitio de Active Directory y subred]
4. [Diagramas de flujo]

---

# Promover servidor miembro a controlador de dominio

En esta sección se promueve **TAILWIND-MBR1** a controlador de dominio adicional dentro del dominio existente `tailwindtraders.internal`.

## Pasos

1. Iniciar sesión en **TAILWIND-MBR1** como:

```
TAILWINDTRADERS\Administrator
Contraseña: Pa55w.rdPa55w.rd
```

2. Abrir **Administrador del servidor → Administrar → Agregar roles y características**.
3. Seleccionar **Instalación basada en roles o características**.
4. Elegir el servidor **TAILWIND-MBR1**.
5. Marcar el rol **Servicios de dominio de Active Directory (AD DS)**.
6. Agregar las características requeridas y continuar.
7. Confirmar e iniciar la instalación.
8. Al finalizar, hacer clic en el **icono de notificación**.
9. Seleccionar **Promocionar este servidor a controlador de dominio**.
10. Elegir **Agregar un controlador de dominio a un dominio existente**.
11. Verificar el dominio: `tailwindtraders.internal`.
<img width="763" height="562" alt="image" src="https://github.com/user-attachments/assets/e042d208-da90-493b-a2f7-632c09cf93f6" />

12. Reautenticarse como Administrator.
<img width="454" height="339" alt="image" src="https://github.com/user-attachments/assets/e2e41b4b-043c-49fb-8f9c-db85f1c20d2d" />

13. Introducir la contraseña **DSRM**:

```
Pa55w.rdPa55w.rd
```
<img width="761" height="554" alt="image" src="https://github.com/user-attachments/assets/a04959ea-d634-4b46-99f8-05262d20dd8d" />

14. Aceptar opciones por defecto (DNS, rutas, revisión).
15. Instalar y permitir el **reinicio automático**.

---

# Transferir roles FSMO

Se transfiere el rol **RID Master** desde **TAILWIND-DC1** hacia **TAILWIND-MBR1**.

## Pasos

1. En **TAILWIND-MBR1**, abrir:
   **Herramientas → Usuarios y equipos de Active Directory**.
2. Clic derecho sobre el dominio → **Todas las tareas → Maestros de operaciones**.
3. En la pestaña **RID**, seleccionar **Cambiar**.
4. Confirmar con **Sí**.
5. Cerrar la ventana de Maestros de operaciones.
<img width="756" height="530" alt="image" src="https://github.com/user-attachments/assets/209e6483-84bf-4d9f-9f04-04ed6e9a7650" />


---

# Crear sitio de Active Directory y subred

Esta configuración optimiza la replicación y representa una ubicación física de red.

## Pasos

1. Iniciar sesión en **TAILWIND-DC1** como:

```
tailwindtraders\Administrator
```

2. Abrir **Herramientas → Sitios y servicios de Active Directory**.
3. Clic derecho en **Sitios → Nuevo sitio**.
4. Nombre del sitio: **Sydney**.
<img width="753" height="529" alt="image" src="https://github.com/user-attachments/assets/598d831a-4d9e-4d3f-8d60-a48f1bfba1ed" />

6. Seleccionar **DEFAULTIPSITELINK** y confirmar.
7. Expandir la carpeta **Sitios**.
8. Clic derecho en **Subredes → Nueva subred**.
9. Prefijo de subred:

```
172.16.1.0/24
```

9. Asociar la subred al sitio **Sydney**.
10. Aceptar y cerrar la consola.
<img width="434" height="538" alt="image" src="https://github.com/user-attachments/assets/eb2ea1ba-f5ee-48fd-8f2a-d84771660c41" />

---

# Diagramas de flujo

## Flujo: Promoción a controlador de dominio adicional

```mermaid
flowchart TD
    A[Servidor miembro] --> B[Instalar rol AD DS]
    B --> C[Promocionar a DC]
    C --> D[Unirse al dominio existente]
    D --> E[Configurar DSRM]
    E --> F[Reinicio]
```

## Flujo: Transferencia de rol FSMO

```mermaid
flowchart TD
    A[Abrir Usuarios y equipos AD] --> B[Maestros de operaciones]
    B --> C[Seleccionar rol RID]
    C --> D[Transferir rol]
    D --> E[Confirmar]
```

## Flujo: Creación de sitio y subred

```mermaid
flowchart TD
    A[Abrir Sitios y servicios AD] --> B[Crear nuevo sitio]
    B --> C[Asignar IP Site Link]
    C --> D[Crear subred]
    D --> E[Asociar subred al sitio]
```

---

# ✅ Resultado

La infraestructura de Active Directory ahora cuenta con:

* Dos controladores de dominio
* Rol FSMO transferido
* Sitio y subred configurados


#  Configuración de operaciones de administración de usuarios

**Objetivo:**

Gestionar **usuarios, grupos y unidades organizativas (OU)** en Active Directory, aplicando buenas prácticas de administración como delegación de permisos, usuarios protegidos, atributos de búsqueda y control de cuentas.

---

# Índice

1. [Crear Unidades Organizativas (OU)]
2. [Crear usuarios]
3. [Crear grupo Sydney Administrators]
4. [Configurar usuario como Protected User]
5. [Delegar permisos a un grupo sobre una OU]
6. [Configurar atributo City y búsqueda]
7. [Deshabilitar usuario Melbourne]
8. [Restablecer contraseña del usuario Brisbane]
9. [Diagramas de flujo]

---

# Crear Unidades Organizativas (OU)

Se crean tres OU para organizar los objetos del dominio por ubicación.

## Pasos

1. En **TAILWIND-DC1**, abrir **Usuarios y equipos de Active Directory** desde **Herramientas**.
2. Clic derecho sobre el dominio `tailwindtraders.internal`.
3. Seleccionar **Nuevo → Unidad organizativa**.
4. Crear las siguientes OU:

   * **Sydney**
   * **Melbourne**
   * **Brisbane**
<img width="753" height="526" alt="image" src="https://github.com/user-attachments/assets/f5919d7f-3cff-45b8-93b6-db79885e3622" />

---

# Crear usuarios

Se crean usuarios contratistas y se configura la expiración de la cuenta.

## Pasos

1. En **TAILWIND-DC1**, abrir **Usuarios y equipos de Active Directory**.
2. Clic derecho sobre la OU **Sydney → Nuevo → Usuario**.
3. Completar los campos:

```
Nombre completo: SydneyContractor
Nombre de inicio de sesión: SydneyContractor
```
<img width="432" height="376" alt="image" src="https://github.com/user-attachments/assets/4d53c6e9-f6ef-4436-b048-c854dde18337" />

4. Establecer contraseña:

```
Pa55w.rdPa55w.rd
```
<img width="429" height="371" alt="image" src="https://github.com/user-attachments/assets/a74aa596-0a27-4628-972f-0bcb45015296" />

5. Finalizar el asistente.
6. Abrir las propiedades del usuario **SydneyContractor**.
7. En la pestaña **Cuenta**, configurar **Expiración de la cuenta**:

```
Fecha: 1 de enero de 2030
```
<img width="452" height="538" alt="image" src="https://github.com/user-attachments/assets/dde376e0-48dd-41b3-816d-b64473154161" />

8. Clic derecho sobre **SydneyContractor → Copiar**.
9. Crear los siguientes usuarios:

   * **MelbourneContractor**
   * **BrisbaneContractor**
<img width="753" height="529" alt="image" src="https://github.com/user-attachments/assets/e823f794-0419-4e3f-81b6-acea421d0673" />
10. Mover:

* **MelbourneContractor** a la OU **Melbourne**
* **BrisbaneContractor** a la OU **Brisbane**

11. Confirmar los avisos de movimiento.


---

# 👥 Crear grupo Sydney Administrators

Se crea un grupo de seguridad para administración delegada.

## Pasos

1. En **Usuarios y equipos de Active Directory**, clic derecho sobre la OU **Sydney**.
2. Seleccionar **Nuevo → Grupo**.
3. Configurar:

```
Nombre del grupo: Sydney Administrators
Ámbito: Universal
```

4. Abrir el usuario **SydneyContractor**.
5. Pestaña **Miembro de → Agregar**.
6. Añadir:

```
Sydney Administrators
```
<img width="761" height="529" alt="image" src="https://github.com/user-attachments/assets/736909d8-a92e-4a77-a6d4-104ebd456b8d" />

7. Confirmar cambios.

---

# Configurar usuario como Protected User

Esta opción aumenta la seguridad del usuario frente a ataques de credenciales.

## Pasos

1. Abrir **SydneyContractor**.
2. Pestaña **Miembro de → Agregar**.
3. Añadir el grupo:

```
Protected Users
```
<img width="753" height="527" alt="image" src="https://github.com/user-attachments/assets/c05e5133-7184-445f-9137-ac09a41bdf9f" />

4. Confirmar.

---

# Delegar permisos a un grupo sobre una OU

Se delega la capacidad de restablecer contraseñas en la OU Sydney.

## Pasos

1. Clic derecho sobre la OU **Sydney → Delegar control**.
2. Siguiente en el asistente.
3. Agregar el grupo:

```
Sydney Administrators
```
<img width="471" height="241" alt="image" src="https://github.com/user-attachments/assets/41a60054-3ee4-4a01-8e51-f810bf0e1489" />

4. Seleccionar la tarea:

```
Restablecer contraseñas de usuario y forzar el cambio en el próximo inicio de sesión
```

<img width="496" height="392" alt="image" src="https://github.com/user-attachments/assets/ab9dd837-ad5a-4c0f-8a69-189d30b567ac" />

5. Finalizar.

---

# Configurar atributo City y búsqueda

Se asigna el atributo **City** a un usuario y se valida mediante búsqueda avanzada.

## Pasos

1. Abrir las propiedades de **SydneyContractor**.
2. Pestaña **Dirección**.
3. Configurar:

```
Ciudad: Sydney
```
<img width="448" height="537" alt="image" src="https://github.com/user-attachments/assets/65d617f2-c8a5-4a25-9dfb-2c406d2976f4" />

4. Clic derecho sobre el dominio → **Buscar**.
5. Pestaña **Avanzado**:

```
Campo: Usuario → City
Condición: Es exactamente
Valor: Sydney
```
<img width="512" height="309" alt="image" src="https://github.com/user-attachments/assets/4ffa7b12-b1c3-438a-a2d2-a0dc705131a3" />

6. Ejecutar búsqueda y verificar resultados.
<img width="517" height="507" alt="image" src="https://github.com/user-attachments/assets/15061d3b-7425-427c-a095-a499b0fa5f58" />

---

# Deshabilitar usuario Melbourne

## Pasos

1. Abrir la OU **Melbourne**.
2. Clic derecho sobre **MelbourneContractor**.
3. Seleccionar **Deshabilitar cuenta**.
4. Confirmar.
<img width="756" height="524" alt="image" src="https://github.com/user-attachments/assets/b85f1dca-cd24-4926-a669-90bf83c6ff32" />

---

# Restablecer contraseña del usuario Brisbane

## Pasos

1. Abrir la OU **Brisbane**.
2. Clic derecho sobre **BrisbaneContractor → Restablecer contraseña**.
3. Establecer la nueva contraseña:

```
Pa66w.rdPa66w.rd
```
<img width="749" height="527" alt="image" src="https://github.com/user-attachments/assets/59b385f6-3df5-4291-a48d-aa201355c42b" />

4. Confirmar los mensajes.

---

# Diagramas de flujo

## Flujo: Gestión de usuarios y OU

```mermaid
flowchart TD
    A[Crear OU] --> B[Crear usuarios]
    B --> C[Asignar grupos]
    C --> D[Configurar atributos]
    D --> E[Delegar permisos]
```

## Flujo: Administración de cuentas

```mermaid
flowchart TD
    A[Usuario existente] --> B[Configurar Protected User]
    B --> C[Delegar control]
    C --> D[Deshabilitar o resetear contraseña]
```

---

# ✅ Resultado

El dominio ahora cuenta con:

* OU organizadas por ciudad
* Usuarios y grupos configurados
* Seguridad reforzada
* Permisos delegados correctamente

# Administrar directivas de contraseña

**Objetivo:**

Configurar **políticas de contraseñas en Active Directory** para reforzar la seguridad del dominio, incluyendo:

* Política de contraseñas a nivel de dominio
* Política de contraseñas de grano fino para administradores
* Habilitación de la **Papelera de reciclaje de Active Directory**

---

# Índice

1. [Configurar la política de contraseñas del dominio]
2. [Configurar una política de contraseñas de grano fino]
3. [Habilitar la Papelera de reciclaje de Active Directory]
4. [Diagramas de flujo]

---

# Configurar la política de contraseñas del dominio

Esta política afecta a **todos los usuarios del dominio**, salvo aquellos que tengan políticas de grano fino aplicadas.

## Pasos

1. En **TAILWIND-DC1**, abrir **Administración de directivas de grupo** desde **Herramientas**.
2. Expandir:

```
Bosque: tailwindtraders.internal
 → Dominios
   → tailwindtraders.internal
```

3. Clic derecho sobre **Directiva de dominio predeterminada** → **Editar**.
4. Navegar a:
(En esta parte el manual de Microsoft esta mal, en vez de salirte Politicas en la maquina lo que salen son directivas, lo he corregido para que este bien)

```
Configuración del equipo
 → Directivas
   → Configuración de Windows
     → Configuración de seguridad
       → Directivas de cuenta
         → Directiva de contraseñas
```

5. Abrir **Longitud mínima de la contraseña**.
6. Establecer el valor en:

```
14 caracteres
```
<img width="814" height="559" alt="image" src="https://github.com/user-attachments/assets/829832cd-15a3-43ea-b1cc-783152960a68" />

7. Aplicar cambios y cerrar el editor.
8. Cerrar la consola de Administración de directivas de grupo.

---

# Configurar una política de contraseñas de grano fino

Las políticas de grano fino permiten aplicar **requisitos distintos** a usuarios o grupos específicos.

## Pasos

1. En **TAILWIND-DC1**, abrir el **Centro administrativo de Active Directory**.
2. En **Descripción general**, seleccionar **Tailwindtraders (local)**.
3. Abrir el contenedor:

```
Sistema
 → Contenedor de configuración de contraseña
```

4. Clic derecho → **Nuevo → Configuración de contraseña**.
5. Configurar los siguientes valores:

```
Nombre: Domain Admin Password Policy
Precedencia: 1
Longitud mínima de contraseña: 16
```
<img width="896" height="543" alt="image" src="https://github.com/user-attachments/assets/50a20d3a-b616-4b07-b7ee-67bd60f4a4ad" />


6. Guardar la política.
7. Abrir la política creada.
8. En **Se aplica directamente a**, hacer clic en **Agregar**.
9. Añadir el grupo:

```
Admins. del dominio
```

10. Comprobar nombres y confirmar.
(El manual esta mal, me pone nombre no encontrado ya que no hemos creado ningun grupo llamado "Domain Admins", esta mal traducido y es Admins. del dominio)

<img width="478" height="362" alt="image" src="https://github.com/user-attachments/assets/4ef7a2b8-00d9-4802-b61c-56d0d5b8a5ec" />


---

# Habilitar la Papelera de reciclaje de Active Directory

## Pasos

1. En **TAILWIND-DC1**, abrir el **Centro administrativo de Active Directory**.
2. Seleccionar **Tailwindtraders (local)**.
3. En el panel derecho, hacer clic en:

```
Habilitar papelera de reciclaje
```

4. Confirmar la advertencia inicial.
5. Confirmar la advertencia sobre latencia de replicación.

(No me salia nada de bin / papelera)
<img width="683" height="631" alt="image" src="https://github.com/user-attachments/assets/17ee72f3-d42f-4d9e-81d9-4d5cfd92b26e" />

---

# Diagramas de flujo

## Flujo: Política de contraseñas del dominio

```mermaid
flowchart TD
    A[Abrir GPMC] --> B[Editar Directiva de dominio predeterminada]
    B --> C[Política de contraseñas]
    C --> D[Establecer longitud mínima]
    D --> E[Aplicar cambios]
```

## Flujo: Política de grano fino

```mermaid
flowchart TD
    A[Abrir AD Administrative Center] --> B[Configuración de contraseña]
    B --> C[Crear nueva política]
    C --> D[Asignar precedencia]
    D --> E[Aplicar a Domain Admins]
```

## Flujo: Papelera de reciclaje de AD

```mermaid
flowchart TD
    A[Abrir AD Administrative Center] --> B[Seleccionar dominio]
    B --> C[Habilitar papelera de reciclaje]
    C --> D[Confirmar advertencias]
    D --> E[Función activa]
```

---

# ✅ Resultado

El dominio ahora cuenta con:

* Política de contraseñas reforzada a nivel global
* Política más estricta para administradores
* Capacidad de recuperación de objetos eliminados

# Configuración de las opciones de seguridad

**Objetivo**

Fortalecer la **seguridad del dominio Active Directory** mediante:

* Restricción de la autenticación **NTLM**
* Habilitación de **auditoría de administración de cuentas**
* Denegación del **inicio de sesión como servicio** a grupos específicos

Estas configuraciones ayudan a reducir vectores de ataque y a mejorar la trazabilidad de acciones administrativas.

---

# Índice

1. [Restringir la autenticación NTLM]
2. [Auditoría de la gestión de cuentas de usuario en la OU Sydney]
3. [Denegar el inicio de sesión como servicio]
4. [Diagramas de flujo]

---

# Restringir la autenticación NTLM

La desactivación de NTLM reduce el riesgo de ataques como **Pass-the-Hash** y fuerza el uso de métodos de autenticación más seguros.

## Pasos

1. En **TAILWIND-DC1**, abrir **Administración de directivas de grupo** desde **Herramientas**.
2. Expandir:

```
Bosque: tailwindtraders.internal
 → Dominios
   → tailwindtraders.internal
     → Objetos de directiva de grupo
```

3. Clic derecho en **Política de controlador de dominio predeterminada** → **Editar**.
4. Navegar a:

```
Configuración del equipo
 → Directivas
   → Configuración de Windows
     → Configuración de seguridad
       → Políticas locales
         → Opciones de seguridad
```

5. Abrir la directiva:

```
Seguridad de red: Restringir NTLM: Autenticación NTLM en este dominio
```

6. Marcar **Definir esta configuración de directiva**.
7. Seleccionar el valor:

```
Denegar todo
```
<img width="945" height="585" alt="image" src="https://github.com/user-attachments/assets/cb1c46ea-f691-4aab-bea0-6d5d9b7c13e1" />

8. Confirmar el aviso de seguridad.
<img width="431" height="201" alt="image" src="https://github.com/user-attachments/assets/c24227cb-7b40-41cf-a090-a1366ecae943" />

10. Cerrar el editor de directivas.

---

# Auditoría de la gestión de cuentas de usuario en la OU Sydney

La auditoría permite registrar eventos de **creación, modificación y eliminación de cuentas**, tanto exitosos como fallidos.

## Pasos

1. Abrir **Administración de directivas de grupo**.
2. Expandir el dominio **tailwindtraders.internal**.
3. Clic derecho sobre la **OU Sydney** → **Crear un GPO en este dominio y vincularlo aquí**.
4. Nombre del GPO:

```
SydneyOUPolicy
```
<img width="383" height="174" alt="image" src="https://github.com/user-attachments/assets/4e975e22-71c6-474a-83d9-398e997a1aeb" />

5. Clic derecho sobre **SydneyOUPolicy** → **Editar**.
6. Navegar a:

```
Configuración del equipo
 → Directivas
   → Configuración de Windows
     → Configuración de seguridad
       → Configuración avanzada de auditoría
         → Políticas de auditoría
           → Administración de cuentas
```

7. Abrir la subcategoría:

```
Auditorar administración de cuentas de usuario
```

8. Marcar **Configurar los siguientes eventos de auditoría**.
9. Seleccionar:

```
Correcto y Error
```
<img width="898" height="576" alt="image" src="https://github.com/user-attachments/assets/e9f15a59-c929-4e57-9170-9f4f9027370c" />

10. Aplicar y cerrar el editor.

---

# Denegar el inicio de sesión como servicio

Esta configuración evita que determinados grupos puedan ejecutar servicios de Windows, reduciendo riesgos de abuso de privilegios.

## Pasos

1. En **Administración de directivas de grupo**, expandir el dominio **tailwindtraders.internal**.
2. Ir a la **OU Sydney**.
3. Clic derecho sobre **SydneyOUPolicy** → **Editar**.
4. Navegar a:

```
Configuración del equipo
 → Políticas
   → Configuración de Windows
     → Configuración de seguridad
       → Directivas locales
         → Asignación de derechos de usuario
```
<img width="791" height="562" alt="image" src="https://github.com/user-attachments/assets/5ea0080d-18c3-47a4-97e8-d318494d1d7e" />

5. Abrir la directiva:

```
Denegar inicio de sesión como servicio
```
<img width="411" height="503" alt="image" src="https://github.com/user-attachments/assets/43dec6a1-4697-40cc-bcbe-a8e6eafd7aa5" />

6. Marcar **Definir esta directiva**.
7. Seleccionar **Agregar usuario o grupo**.
8. Buscar y seleccionar el grupo:

```
Administradores de Sydney
```
<img width="414" height="504" alt="image" src="https://github.com/user-attachments/assets/16a33a0d-eecd-489b-a39b-2723917a2186" />

9. Confirmar todos los cuadros de diálogo.

---

# Diagramas de flujo

## Flujo: Restricción de NTLM

```mermaid
flowchart TD
    A[Abrir GPMC] --> B[Editar política de DC]
    B --> C[Opciones de seguridad]
    C --> D[Restringir NTLM]
    D --> E[Denegar todo]
```

## Flujo: Auditoría de cuentas

```mermaid
flowchart TD
    A[Crear GPO en OU] --> B[Editar GPO]
    B --> C[Auditoría de cuentas]
    C --> D[Éxito y Fracaso]
```

## Flujo: Denegar inicio de sesión como servicio

```mermaid
flowchart TD
    A[Editar GPO de OU] --> B[Asignación de derechos]
    B --> C[Denegar inicio de sesión como servicio]
    C --> D[Agregar grupo]
```

---

# ✅ Resultado

El dominio ahora dispone de:

* Autenticación NTLM deshabilitada
* Auditoría activa sobre cuentas de usuario
* Restricciones de inicio de sesión por servicio

