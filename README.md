# VPN Site-to-Site con FortiGate - Proyecto Final P1
**Estudiante:** Cristopher de los Santos  
**Matrícula:** 2024-1414  


---

## 1. Objetivo de la Red
El objetivo de esta asignación es implementar una conexión segura mediante un túnel **IPsec Site-to-Site** entre dos infraestructuras de red (Site A y Site B). Se busca garantizar la integridad y confidencialidad de los datos que viajan a través de un proveedor de servicios de internet (ISP) simulado, permitiendo que dos redes LAN privadas se comuniquen de forma transparente.

## 2. Topología de Red
Se ha diseñado una topología en GNS3 que consta de:
* **FortiGate-A y FortiGate-B:** Firewalls perimetrales encargados del cifrado.
* **Router IOU:** Actúa como el ISP principal.
* **Nodos Finales:** VPCS y WebTerm para pruebas de conectividad.

> ![Insertar aquí captura de tu topología de GNS3]

### Tabla de Direccionamiento (Basada en Matrícula 2024-1414)
| Dispositivo | Interfaz | Dirección IP | Descripción |
| :--- | :--- | :--- | :--- |
| **FG-A** | Port2 (WAN) | 200.24.14.2/30 | Salida al ISP |
| **FG-A** | Port3 (LAN) | 10.24.14.1/24 | Gateway LAN A |
| **FG-B** | Port2 (WAN) | 200.24.14.6/30 | Salida al ISP |
| **FG-B** | Port3 (LAN) | 10.14.14.1/24 | Gateway LAN B |
| **ISP** | e0/0 - e0/1 | 200.24.14.1 - 5 | Enlaces WAN |

---

## 3. Configuraciones Realizadas

### A. Configuración de Interfaces y Rutas
Se configuraron las IPs estáticas en los puertos WAN y se estableció una **Static Route** apuntando al ISP para permitir la salida de los paquetes hacia el túnel.

<img width="777" height="642" alt="image" src="https://github.com/user-attachments/assets/9b0ef90a-a43b-4adc-9d8e-8d2baec07b7f" />


### B. Fase 1 y Fase 2 de IPsec
Se definieron los parámetros de encriptación (AES-256) y autenticación (SHA-256) utilizando una **Pre-shared Key**. Se configuraron los selectores de fase 2 para permitir el tráfico entre las subredes `10.24.14.0/24` y `10.14.14.0/24`.

<img width="563" height="580" alt="image" src="https://github.com/user-attachments/assets/14bbe5e0-3421-4be1-b135-b96c0b0af5cf" />


### C. Políticas de Firewall
Se crearon políticas bidireccionales para permitir el tráfico desde la interfaz LAN hacia la interfaz de la VPN y viceversa, asegurando que el tráfico no sufra NAT para mantener las IPs originales.

<img width="1061" height="88" alt="image" src="https://github.com/user-attachments/assets/84a96fd3-19d3-4bb0-b664-b41f8e273ca2" />


---

## 4. Demostración de Funcionamiento
Se realizaron pruebas de conectividad desde los hosts finales para validar el túnel.
<img width="1101" height="482" alt="image" src="https://github.com/user-attachments/assets/3a6dd667-df71-4846-9cf7-b7fd34c651ff" />


### Trace-route
En la traza se observa que el tráfico salta directamente de la red local a la remota, confirmando la encapsulación de los datos sobre el ISP.

<img width="494" height="102" alt="image" src="https://github.com/user-attachments/assets/4b34c50b-4ca2-42ef-8924-7ecaf0d34878" />


---

## 5. Archivos del Proyecto
* [Documentación en PDF](./CristopherDelosSantos_2024-1414_Informe_P1.pdf)
* [Video de Demostración](Enlace_a_Youtube_o_GoogleDrive)
