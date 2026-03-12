# China Hotel - Gestión de Recursos Compartidos con Samba
## Ubuntu Server 24.04 en VirtualBox

---

# RA4 - Gestión de Recursos Compartidos

## Resultado de Aprendizaje
Gestionar los recursos compartidos del sistema, interpretando especificaciones y determinando niveles de seguridad.

## Criterios de Evaluación
- a) Diferenciar permiso y derecho
- b) Identificar recursos a compartir
- c) Asignar permisos correctamente
- d) Compartir impresoras en red
- e) Utilizar entorno gráfico
- f) Establecer niveles de seguridad efectivos
- g) Validar el acceso en grupo

---

# Contexto: China Hotel

El **China Hotel** ha decidido implementar un servidor de archivos para organizar y proteger la información interna.

Se almacenarán:
- Datos de huéspedes
- Nóminas del personal
- Informes de dirección
- Documentación técnica
- Recursos de impresión

El servidor se implementa en **Ubuntu Server 24.04 dentro de VirtualBox** usando **Samba**.

---

# Tarea 1 - Diferencia entre Permiso y Derecho (CE a)

## Permiso
Un permiso define el acceso a un recurso.

Ejemplos:
- Leer archivos
- Escribir en una carpeta
- Modificar documentos

Ejemplo en China Hotel:
El grupo Recepción puede leer la carpeta de huéspedes.

## Derecho
Un derecho define acciones dentro del sistema.

Ejemplos:
- Iniciar sesión en el servidor
- Apagar el sistema
- Administrar servicios

Conclusión:
- Permiso → acceso a archivos
- Derecho → acciones del usuario en el sistema

---

# Tarea 2 - Recursos a Compartir (CE b)

| Recurso | Departamento | Acceso |
|---|---|---|
| registro_huespedes | Recepción | Solo lectura |
| gestion_nominas | RRHH y Gerencia | Lectura y escritura |
| informes_direccion | Gerencia | Lectura y escritura |
| soporte_tecnico | Mantenimiento | Lectura y escritura |

---

# Instalación de Samba

## 1. Actualizar el sistema

```bash
sudo apt update && sudo apt upgrade -y
```
## 2. Instalar Samba
```
sudo apt install samba smbclient cifs-utils -y
```
# Tarea 3 - Crear Estructura del China Hotel (CE c)
## Crear carpetas
```
sudo mkdir -p /srv/chinahotel/registro_huespedes
sudo mkdir -p /srv/chinahotel/gestion_nominas
sudo mkdir -p /srv/chinahotel/informes_direccion
sudo mkdir -p /srv/chinahotel/soporte_tecnico
```

<img width="717" height="70" alt="image" src="https://github.com/user-attachments/assets/efc62ab4-7a2f-4bbe-bb29-168eb740f41d" />



## Crear grupos
```
sudo groupadd Recepcion
sudo groupadd RRHH
sudo groupadd Gerencia
sudo groupadd Mantenimiento
```

<img width="532" height="63" alt="image" src="https://github.com/user-attachments/assets/e3b23350-ed0a-4934-8246-6efc9013c8ef" />


## Asignar grupos a carpetas
```
sudo chown :Recepcion /srv/chinahotel/registro_huespedes
sudo chown :RRHH /srv/chinahotel/gestion_nominas
sudo chown :Gerencia /srv/chinahotel/informes_direccion
sudo chown :Mantenimiento /srv/chinahotel/soporte_tecnico
```

<img width="782" height="67" alt="image" src="https://github.com/user-attachments/assets/6ae370d8-a69a-4bbe-b1f3-f248aae67ce8" />


## Permisos del sistema
```
sudo chmod 750 /srv/chinahotel/registro_huespedes
sudo chmod 770 /srv/chinahotel/gestion_nominas
sudo chmod 770 /srv/chinahotel/informes_direccion
sudo chmod 770 /srv/chinahotel/soporte_tecnico
```

<img width="765" height="68" alt="image" src="https://github.com/user-attachments/assets/a900a610-9bb8-4426-b02d-4ee9c7319bf6" />


## Configuración de Samba
Aquí editamos el archivo:
/etc/samba/smb.conf
Para ello ejecutaremos:
```
sudo nano /etc/samba/smb.conf
```
Añadimos al final de este uno:
```
[Registro_Huespedes]
   path = /srv/chinahotel/registro_huespedes
   browsable = yes
   writable = no
   valid users = @Recepcion
   create mask = 0640
   directory mask = 0750

[Gestion_Nominas]
   path = /srv/chinahotel/gestion_nominas
   browsable = no
   writable = yes
   valid users = @RRHH @Gerencia
   create mask = 0660
   directory mask = 0770

[Informes_Direccion]
   path = /srv/chinahotel/informes_direccion
   browsable = yes
   writable = yes
   valid users = @Gerencia
   create mask = 0660
   directory mask = 0770

[Soporte_Tecnico]
   path = /srv/chinahotel/soporte_tecnico
   browsable = yes
   writable = yes
   valid users = @Mantenimiento
   create mask = 0660
   directory mask = 0770
```

<img width="956" height="555" alt="image" src="https://github.com/user-attachments/assets/3cf5b8ca-fc8e-4757-8d97-735d001e00ad" />

# Creamos Usuarios del hotel
## Crear usuarios Linux
```
sudo useradd -M -s /sbin/nologin liwei
sudo useradd -M -s /sbin/nologin chen
sudo useradd -M -s /sbin/nologin zhang
```
<img width="643" height="69" alt="image" src="https://github.com/user-attachments/assets/a6815c5f-2415-4011-96c5-11b988abd6b9" />


## Asignar a grupos

```
sudo usermod -aG Recepcion liwei
sudo usermod -aG RRHH chen
sudo usermod -aG Gerencia zhang
```
<img width="678" height="54" alt="image" src="https://github.com/user-attachments/assets/c0a5da80-65d5-4f36-9872-66fba7c8a9f5" />

## Contraseñas
```
sudo passwd liwei
sudo passwd chen
sudo passwd zhang
```
<img width="576" height="204" alt="image" src="https://github.com/user-attachments/assets/405ad789-05a6-4b35-a10b-6dc77cb9d927" />


## Reiniciarmos los servicios
```
sudo systemctl restart smbd nmbd
sudo systemctl status smbd
```
<img width="956" height="362" alt="image" src="https://github.com/user-attachments/assets/0c71aca7-88f7-4109-ad91-f15257cc4a30" />

# Tarea 4 - Compartir Impresora (CE d)
Instalar CUPS:
```
sudo apt install cups -y
```
Editar configuración:
```
sudo nano /etc/cups/cupsd.conf
```
Permitir red local:
```
Listen 631
<Location />
  Allow @LOCAL
</Location>
```
Reiniciar:
```
sudo systemctl restart cups
```
Acceso desde navegador:
```
http://IP_SERVIDOR:631
```
Crear impresora:
Impresora_BackOffice_ChinaHotel

# Tarea 5 - Compartición desde Entorno Gráfico (CE e)
Si se instala GUI:
```
sudo dpkg --configure -a
sudo apt -f install
sudo apt update
sudo apt install ubuntu-desktop -y
```
despues reinciamo el sistema:

```
sudo reboot
```
Pasos:

1. Click derecho en carpeta
2. Propiedades
3. Compartir en red
4. Activar compartir

Ejemplo:
Carpeta: marketing_eventos

# Tarea 6 - Seguridad Efectiva (CE f)
La seguridad final es el permiso más restrictivo entre:

Permisos Linux

Permisos Samba

Ejemplo:
Si Samba permite escribir pero Linux solo lectura → el usuario solo podrá leer.

# Tarea 7 - Validación de Acceso (CE g)
| Usuario | Departamento | Acceso esperado    |
| ------- | ------------ | ------------------ |
| liwei   | Recepción    | Registro_Huespedes |
| chen    | RRHH         | Gestion_Nominas    |
| zhang   | Gerencia     | Informes_Direccion |

Desde Windows o Linux:
```
\\IP_SERVIDOR\Registro_Huespedes
```
Verificar:

-Accesos permitidos
-Accesos denegados correctamente
