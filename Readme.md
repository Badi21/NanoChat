# nanoChat - Sistema de Chat Distribuido

## Descripción del Proyecto

Este proyecto, conocido como nanoChat, se enfoca en el diseño e implementación de un sistema de chat distribuido. El sistema incluye un servidor de chat y múltiples clientes que se conectan al chat. Una característica única es que la dirección de los servidores de chat se obtiene consultando a un servidor de directorio. Los servidores de chat se registran en el directorio para que los clientes puedan descubrirlos. Además, el servidor de chat permite a los usuarios participar en salas de chat para conversar entre ellos.

![Elementos del Nanochat](img/Elementos%20del%20Nanochat.png)

## Objetivo de la Práctica

El objetivo principal de esta práctica es que los alumnos adquieran habilidades para diseñar protocolos de comunicación y programar los elementos del sistema nanoChat. Se centra en dos protocolos de comunicación de nivel de aplicación: uno para la interacción con el directorio y otro para la comunicación cliente-servidor de chat.

## Diseño de Protocolos

### Comunicación con el Directorio
- Protocolo confiable semi-dúplex basado en parada y espera.
- Operación sobre un protocolo de transporte no confiable como UDP.

### Comunicación Cliente-Servidor de Chat
- Protocolo asumiendo que el transporte es confiable (como TCP).

### Autómatas y Especificaciones de Mensajes
- Autómatas relacionados con el servicio de directorio.
- Autómata para el cliente al comunicarse con el servidor de chat.
- Autómata para el servidor de chat al comunicarse con el cliente.
- Uso de lenguajes binario y ASCII (Field:Value o Lenguaje de Marcas) para los mensajes.

## Implementación del Software

En paralelo al diseño, los alumnos implementarán los elementos del software. Se proporciona un esqueleto de código para el servidor de directorio y las implementaciones parciales de los programas NanoChat (cliente) y NanoChatServer (servidor de chat).

### Lógica del Programa Directory
- Escucha mensajes y registra servidores de chat disponibles.
- Utiliza el puerto 6868/udp para recibir mensajes entrantes.
- Posibilidad de simular pérdida de paquetes (-loss <probability>).

### Lógica del Programa NanoChatServer
- Genera hilos para atender a clientes conectados.
- Utiliza el puerto 6969/tcp

### Lógica del Programa NanoChat (Cliente)
- Interacción mediante línea de comandos.
- Reconoce órdenes como nick, roomlist, enter, send, exit, info, quit, help.
- Requiere la dirección del servidor de directorio como parámetro.

## Funcionalidades Basicas Implementadas
- Formato de mensajes para segmentos UDP y TCP según el diseño.
- Registro de dirección del servidor de chat en el directorio.
- Obtención y muestra de la lista de salas disponibles.
- Registro de nick del usuario en el servidor de chat con verificación de duplicados.
- Solicitar acceso a una sala y conocer el resultado.
- Obtener información de una sala.
- Envío de mensajes de texto a todos los clientes en una sala.
- Salir de la sala actual y entrar en otra.
- Desconexión del usuario del servidor.

## Funcionalidades Adicioanles 
Cualquier persona puede añadir las siguientes funcionalidades al proyecto:

- Notificaciones de entradas/salidas de usuarios en salas.
- Creación de nuevas salas en el servidor.
- Mensajes privados a usuarios de la sala.
- Renombrar una sala.
- Ver el histórico de mensajes en una sala.
- Definir administradores de sala con capacidades de expulsión y ascenso de usuarios.
- Envío de archivos binarios a través de una sala.

## Instrucciones de Ejecución

### Directorio
```bash
java Directory -loss <probability>

```

### NanoChatServer
```bash
java NanoChatServer <DirectoryServer>

```

### NanoChat(Client)
```bash
java Directory -loss <probability>
```

## Contribuciones
Este proyecto es un trabajo en desarrollo. Se anima a los contribuyentes a implementar nuevas funcionalidades y mejorar la robustez del sistema.

¡Diviértete chateando con nanoChat!

<br>

---
---

<br>
<br>

# Estructura del Proyecto nanoChat

El código proporcionado para el proyecto nanoChat está organizado en diferentes paquetes, cada uno encargado de una parte específica del sistema. A continuación se describe la estructura del proyecto:

![Organización del Proyecto y Entidades](img/Organizacion%20del%20Proyecto%20y%20Entidades.png)


## Paquete `es.um.redes.nanoChat.client.application`
- **NanoChat.java:** Clase principal del cliente que contiene la secuencia de llamadas al controlador. Esta clase ya está completamente implementada.
- **NCController.java:** Implementación del controlador encargado de coordinar la comunicación con el directorio y el servidor de chat. Gestiona las interacciones según la fase en la que se encuentre el programa o las acciones del usuario.

## Paquete `es.um.redes.nanoChat.client.shell`
- **NCShell.java:** Implementación del shell que interactúa con el usuario mediante la línea de comandos. Aunque está completa, puede ser modificada para incorporar mejoras.
- **NCCommands.java:** Clase de apoyo que define los tipos de comandos y parámetros aceptados por el shell. Puede ser modificada para mejorar la implementación del shell.

## Paquete `es.um.redes.nanoChat.client.comm`
- **NCConnector.java:** Contiene la funcionalidad necesaria para la comunicación entre el cliente y el servidor de chat. Aquí se deben programar los intercambios de mensajes iniciados principalmente por el cliente.

## Paquete `es.um.redes.nanoChat.directory.connector`
- **DirectoryConnector.java:** Implementación incompleta de la clase que proporcionará los métodos necesarios para la comunicación con el Directorio utilizando sockets UDP. Será utilizada tanto por el cliente como por el servidor de chat.

## Paquete `es.um.redes.nanoChat.directory.server`
- **Directory.java:** Clase principal que procesa los parámetros de llamada al servidor de directorio y se encarga de lanzar el proceso en segundo plano que atenderá las solicitudes. Este código ya ha sido completado por cada grupo.
- **DirectoryThread.java:** Implementación incompleta de la funcionalidad relacionada con el directorio. Se presupone ya completada en semanas anteriores.

## Paquete `es.um.redes.nanoChat.messageFV` / `es.um.redes.nanochat.messageML`
- **NCMessage.java:** Superclase que abstrae un mensaje del protocolo entre el cliente y el servidor de chat. Contiene métodos de utilidad para generar o interpretar mensajes. Se deben derivar los diferentes tipos de mensajes necesarios.
- **NCRoomMessage.java:** Subclase de ejemplo que proporciona la implementación concreta de un tipo de mensaje específico.

## Paquete `es.um.redes.nanoChat.server`
- **nanoChatServer.java:** Clase principal del servidor de chat. Se encarga de crear el socket del servidor, aceptar conexiones y generar los hilos correspondientes para atender las solicitudes entrantes. Está implementada salvo algunos detalles.
- **NCServerThread.java:** Clase que implementa el hilo que debe atender cada conexión entrante procedente de un cliente. Se encuentra esbozada.
- **NCServerManager.java:** Clase utilizada para gestionar las distintas salas y usuarios conectados al servidor. Un objeto de esta clase será compartido entre todos los hilos. Su implementación está incompleta.

## Paquete `es.um.redes.nanoChat.server.roomManager`
- **NCRoomManager.java:** Clase abstracta (superclase) que se utilizará como base para implementar la lógica concreta de cada sala de chat. Se heredará de ella implementando los métodos y atributos necesarios.
- **NCRoomDescription.java:** Clase que representa la descripción del estado actual de una sala, como su nombre, lista de miembros y fecha del último mensaje enviado. Contiene un método para transformar esta información a un formato imprimible por pantalla.

# Agradecimientos

Agradecemos a todos los contribuyentes que han participado en el desarrollo y mejora continua de nanoChat. ¡Gracias por hacer de este proyecto un esfuerzo colaborativo!

Si encuentras útil este proyecto, ¡no dudes en dejar una ⭐ en nuestro repositorio en [GitHub](https://github.com/badi21/nanoChat)! 
