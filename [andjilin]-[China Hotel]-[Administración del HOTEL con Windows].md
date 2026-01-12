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

## Diseño

* **Grupos de seguridad** (acceso a recursos)
* Ámbito:

  * Global → Usuarios
  * Local de dominio → Permisos

Ejemplo:

```
GG_Recepción
DL_Acceso_PMS
```

✅ Criterio cubierto: CE d

---

# 👥 Tarea 5: Implementación de Grupos Funcionales (CE e)

## Objetivo

Crear y gestionar grupos de seguridad.

## Pasos

1. Crear grupos:

```
GR_Recepción_Diurna
GR_Administración_TI
GR_Mantenimiento
```

2. Tipo: Seguridad
3. Ámbito: Global

✅ Criterio cubierto: CE e

---

# 🔑 Tarea 6: Asignación de Permisos (CE f)

## Objetivo

Gestionar la pertenencia de usuarios a grupos.

## Pasos

1. Añadir usuarios a grupos según rol.
2. Ejemplo:

```
recep01_temp → GR_Recepción_Diurna
admin01 → GR_Administración_TI
```

3. Asignar permisos NTFS y recursos compartidos a grupos.

✅ Criterio cubierto: CE f

---

# 🔍 Tarea 7: Auditoría de Seguridad Inicial (CE g)

## Objetivo

Identificar usuarios y grupos especiales.

## Pasos

1. Revisar cuentas:

   * Administrator → Renombrar
   * Guest → Deshabilitar
2. Revisar grupos:

   * Domain Admins
   * Enterprise Admins
3. Documentar estado de seguridad.

✅ Criterio cubierto: CE g

---

# 🚀 Tarea 8: Movilidad y Despliegue de Perfiles (CE h)

## Objetivo

Planificar perfiles móviles.

## Pasos

1. Crear recurso compartido:

```
\\SERVER\PerfilesMoviles
```

2. Configurar perfiles móviles para:

   * Dirección
   * Mantenimiento
3. Definir rutas en propiedades del usuario.

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

---

# ✅ Síntesis Final

Este proyecto cumple íntegramente el **RA2**, demostrando:

* Gestión avanzada de usuarios
* Diseño correcto de grupos
* Seguridad y movilidad
* Uso competente de herramientas de administración

📁 Documento listo para **GitHub (.md)**
