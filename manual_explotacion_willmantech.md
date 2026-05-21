# **MANUAL DE EXPLOTACIÓN WILLMANTECH**

## FASE 1º: GENERACIÓN DEL INFORME DINÁMICO (QWEB XML)

El objetivo de esta fase es diseñar y programar la estructura en XML de una plantilla de informe de factura. que la recogeremos en el archivo:
"*report_invoice_willmantech.xml*"

Esa estructura va a estar con una sintaxix bien estruturada, haciendo uso de directivas de iteración y usando campos como "doc.name", "doc.date" o "doc.amount_total"

**Gestión de Ventas (sale)**: Permite la parametrización de ofertas comerciales, gestión de presupuestos, flujos de aprobación y ordenación de pedidos de clientes.

**Facturación (account)**: Motor contable financiero que gestiona los diarios de ventas, el cálculo automático de devengos impositivos (IVA), la conciliación bancaria y la emisión de facturas.

**CRM (crm)**: Módulo de preventa para el seguimiento de iniciativas, embudos de conversión de clientes (leads) y asignación de oportunidades comerciales.


### ¿Como he realizado esta fase?

Para empezar, he usado la factura que ya habiamos creado previamente en Odoo y he utlizado su código para la realización de la Fase 1.
He extraido las partes importantes y así he comenzado a generar el archivo xml.

## FASE 2º: INTEROPERABILIDAD DE DATOS

Debido a que no hemos hecho JSON en clase, unicamente vamos a realizar el 
"*invoice_ubl.xml*" aquí incluiremos componentes agregados y basicos como el ID de personalización 


## GUIA DE INSTALACIÓN:

Para instalar este sistema desde cero en un servidor limpio, debes seguir los siguientes pasos utilizando la terminal de comandos.

hay que crear un archivo de texto llamado .env en la carpeta del proyecto. Este archivo guarda los nombres de usuario y las contraseñas secretas para que los contenedores puedan conectarse entre sí:

    *POSTGRES_DB=willmantech_prod
    POSTGRES_USER=willman_admin
    POSTGRES_PASSWORD=Mi_Contrasena_Segura_2026

    HOST=db_willmantech
    USER=willman_admin
    PASSWORD=Mi_Contrasena_Segura_2026*

### **Archivo de inicio (docker-compose.yml)**:
```yml
version: '3.8'

services:
  db_willmantech:
    image: postgres:15-alpine
    container_name: willmantech_sgbd
    environment:
      - POSTGRES_DB=${POSTGRES_DB}
      - POSTGRES_USER=${POSTGRES_USER}
      - POSTGRES_PASSWORD=${POSTGRES_PASSWORD}
    volumes:
      - willmantech_db_data:/var/lib/postgresql/data
    networks:
      - willman_net
    restart: always

  web_willmantech:
    image: odoo:16.0
    container_name: willmantech_app
    depends_on:
      - db_willmantech
    ports:
      - "8069:8069"
    environment:
      - HOST=${HOST}
      - USER=${USER}
      - PASSWORD=${PASSWORD}
    volumes:
      - willmantech_app_data:/var/lib/odoo
    networks:
      - willman_net
    restart: always

networks:
  willman_net:
    driver: bridge

volumes:
  willmantech_db_data:
  willmantech_app_data:
```

### **Seguridad y Permisos de Usuarios**

**Administrador:** Puede hacer de todo. Crea usuarios, borra registros, ve la facturación y cambia las configuraciones de todo el sistema.

**Contable:** Puede ver y modificar todo lo relacionado con facturas e impuestos, pero no puede cambiar la configuración general del programa.

**Comercial:** Puede crear presupuestos y ver los datos de los clientes, pero tiene prohibido entrar al módulo de facturación y ver las cuentas de la empresa.

Además habrá que revisar las contraseñas y las reglas para ambas.

### **Cómo se hacen las Facturas y cómo se crea el PDF**

1.Entra en el menú Facturación y pincha en Facturas de Clientes.

2.Pulsa el botón Nuevo.

3.Elige el nombre del cliente de la lista.

4.Abajo, en la zona de las líneas, añade los productos o servicios que le vas a vender y pon el precio. El programa calcula el 21% de IVA de forma automática.

5.Si quieres hacer un descuento, pon el número en la columna de descuento. Si no pones nada, esa columna directamente no aparecerá en el papel final.

6.Pulsa Guardar 

7.Cuando estés seguro de que todo está bien, pulsa Confirmar. El programa le asignará un número oficial que ya no se puede cambiar


