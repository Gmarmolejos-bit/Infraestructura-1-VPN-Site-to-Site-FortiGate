# Infraestructura 1 - VPN Site-to-Site con FortiGate

## 🎥 Video demostrativo

[Ver video demostrativo](ENLACE_DEL_VIDEO)

## 📌 Propósito del laboratorio

El objetivo de esta práctica fue configurar una VPN IPsec Site-to-Site entre dos equipos FortiGate dentro de GNS3.

La idea principal fue lograr que dos redes diferentes pudieran comunicarse de forma segura, utilizando un túnel VPN entre ambos FortiGate.

En este caso, una red representa el lado de los usuarios y la otra el lado donde se encuentra el servidor web.

---

## 🖥️ Topología

La infraestructura está formada por los siguientes equipos:

- 2 FortiGate
- 1 PC de usuario
- 1 servidor web
- Switches para representar la conexión entre ambos sitios
- Un nodo NAT para tener conectividad externa
- 2 Webterm utilizados para acceder a la interfaz gráfica de los FortiGate

### Diagrama de la topología

![Topología de Infraestructura 1](imagenes/topologia.png)

---

## 🌐 Direccionamiento IP

Para separar correctamente ambas redes se utilizó el siguiente direccionamiento:

| Dispositivo | Red / Interfaz | Dirección IP |
|---|---|---|
| FortiGate 1 | WAN | 203.0.113.2/29 |
| FortiGate 2 | WAN | 203.0.113.3/29 |
| FortiGate 1 | LAN de usuarios | 10.12.48.1/25 |
| PC1 | LAN de usuarios | 10.12.48.2/25 |
| FortiGate 2 | LAN de servidores | 10.12.48.129/28 |
| WEB-SERVER | LAN de servidores | 10.12.48.130/28 |

---

## 🔐 Configuración de la VPN Site-to-Site

Se configuró una VPN IPsec Site-to-Site entre los dos FortiGate utilizando IKEv2.

La función de esta VPN es permitir la comunicación entre estas dos redes:

- Red de usuarios: `10.12.48.0/25`
- Red de servidores: `10.12.48.128/28`

Una vez levantado el túnel, el tráfico entre ambas redes puede viajar de forma cifrada a través de la VPN.

---

## 🔄 Enrutamiento

También fue necesario configurar las rutas para que cada FortiGate supiera cómo llegar a la red que se encuentra del otro lado del túnel.

En el FortiGate 1 se configuró una ruta hacia:

`10.12.48.128/28`

Y en el FortiGate 2 se configuró una ruta hacia:

`10.12.48.0/25`

De esta forma, cada equipo puede enviar el tráfico correspondiente a través de la VPN.

---

## 🛡️ Políticas de Firewall

Se crearon las políticas necesarias para permitir el tráfico entre las redes locales y la VPN.

Las principales reglas permiten la comunicación:

- Desde la red local hacia la VPN
- Desde la VPN hacia la red local

Sin estas políticas, aunque el túnel estuviera activo, el tráfico no podría pasar correctamente entre ambas redes.

---

## ✅ Pruebas realizadas

Para comprobar que la configuración estaba funcionando correctamente se realizaron varias pruebas.

La prueba principal fue hacer ping desde PC1 hacia el servidor web que se encuentra detrás del segundo FortiGate.

**Origen:** `10.12.48.2`

**Destino:** `10.12.48.130`

**Resultado:** comunicación exitosa.

Con esta prueba se confirmó que el tráfico estaba pasando correctamente desde la red de usuarios hacia la red de servidores a través de la VPN.

---

## 🔎 Verificación del túnel

También se comprobó directamente desde el FortiGate que el túnel estuviera activo.

Uno de los comandos utilizados fue:

```bash
get vpn ipsec tunnel summary
```

El resultado mostró el selector activo:

```text
selectors(total,up): 1/1
```

Además, se utilizó:

```bash
diagnose vpn tunnel list
```

Con este comando se pudieron revisar los paquetes enviados y recibidos a través del túnel IPsec.

---

## 📸 Evidencias

En la carpeta `imagenes/` se encuentran las capturas utilizadas para demostrar el funcionamiento de la infraestructura.

Entre las evidencias se incluyen:

- Topología completa
- Estado de la VPN
- Configuración de las interfaces
- Rutas configuradas
- Políticas de firewall
- Ping exitoso entre PC1 y el servidor web
- Tráfico pasando por el túnel IPsec

---

## ⚙️ Running Configurations

Las configuraciones de ambos FortiGate se encuentran guardadas en la carpeta:

`running-configs/`

Archivos:

- `fortigate-1.txt`
- `fortigate-2.txt`

Esto permite revisar de manera más detallada la configuración realizada en cada equipo.

---

## 📂 Scripts y comandos

En la carpeta `scripts/` se encuentran los comandos utilizados durante la configuración y las pruebas realizadas en el laboratorio.

---

## 📝 Conclusión

Durante esta práctica fue posible configurar correctamente una VPN IPsec Site-to-Site entre dos FortiGate.

Al finalizar, se comprobó que PC1 podía comunicarse con el servidor web ubicado en la red remota, confirmando que el direccionamiento, las rutas, las políticas de firewall y el túnel VPN estaban funcionando correctamente.

Esta práctica permitió entender de una manera más clara cómo se puede establecer una comunicación segura entre dos redes diferentes utilizando una VPN Site-to-Site.
