# Gestión de Recursos Compartidos en GNU/Linux con Samba

[Reto 0x: Gestionar los recursos compartidos SAMBA](https://aules.edu.gva.es/fp/mod/assign/view.php?id=9934089)

---

## 🎯 Objetivo
El objetivo es gestionar los recursos compartidos del sistema, creando una estructura de carpetas de red lógica y segura, e interpretando especificaciones y determinando los niveles de seguridad  que rigen el acceso.

 ---

# 🧩 Relación con Resultados de Aprendizaje
**RA4.** Gestiona los recursos compartidos del sistema, interpretando especificaciones y determinando niveles de seguridad.

---

# 🏨 Contexto del Proyecto
El **ChinaHotel** está optimizando su infraestructura de red para asegurar que la información sensible (reservas, nóminas, informes de gestión) y los recursos de impresión sean accesibles solo por el personal  autorizado.

---

#### Tareas Operativas Críticas
Cada una de las siguientes tareas es fundamental para el despliegue seguro y funcional de los recursos del hotel:
 
> - Tarea 1: Clasificación de Roles de Seguridad (CE a)

> - Tarea 2: Inventario y Planificación de Recursos (CE b)

> - Tarea 3: Aplicación de Permisos de Carpeta (CE c)

> - Tarea 4: Despliegue del Servicio de Impresión (CE d)

> - Tarea 6: Determinación de la Seguridad Resultante (CE f)

> - Tarea 7: Validación de Acceso Funcional (CE g))


---

#🧑‍💼 Tarea 1: Clasificación de Roles de Seguridad (CE a)

#### diferencia entre permiso y derecho

-Permiso: es el acceso sobre un recurso específico.
Ejemplo:

 - permiso de lectura a un arcivo
 - permiso de escrtura y lectura a una carpeta
 - permiso de ejecucion a la impresora

```bash
chmod 770 carpeta
```

-Derecho: Se refiere a privilegios del sistema operativo.

Ejemplo:
 - Derecho a inicar sesion
 - Derecho a administrar Samba
 - Derecho a apagar el servidor

---

#🧑‍💼 Tarea 2:  Inventario y Planificación de Recursos (CE b)

##objetivo

identificar qué necesita ser compartido y bajo qué condiciones iniciales.

###Pasos

Antes de todo tenemos que empezar instalando samba con los siguientes comandos:

```powershell
sudo apt update
sudo apt upgrade -y
sudo apt install samba -y
```

comprobamos que este instalado:

```powershell
sudo systemctl status smbd
```


1. Datos_Huespedes
  Creamos la carpeta Datos huespedes con el comando mkdir
```powershell
/srv/samba/Datos_Huespedes
```

3. 
4. 














