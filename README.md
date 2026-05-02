# NanoFiles - Redes de Comunicaciones

**NanoFiles** es un sistema descentralizado de compartición y transferencia de ficheros (P2P y Cliente-Servidor) desarrollado en Java. Este proyecto ha sido creado como práctica final para la asignatura de Redes de Comunicaciones.

## 🚀 Características Implementadas

El sistema se divide en dos componentes principales: el **Directorio** (Servidor centralizado UDP) y los **Peers** (Nodos P2P TCP). Se han implementado con éxito todas las funcionalidades requeridas y las mejoras opcionales de los boletines:

- **Comunicación UDP Confiable:** Implementación del protocolo de ventana deslizante (Stop & Wait) para garantizar la entrega de datagramas al directorio, tolerando pérdidas en la red.
- **Comunicación Binaria P2P:** Los peers se comunican de forma directa mediante Sockets TCP enviando mensajes binarios puros (sin conversiones a cadenas).
- **Descargas Fragmentadas (Chunks):** Soporte para la transmisión de archivos muy pesados dividiéndolos en pequeños fragmentos (chunks) de 4MB. Los fragmentos se escriben directamente en disco para prevenir el desbordamiento de memoria (OOM).
- **Descarga Multi-Peer (`peerdl *`):** Capacidad para buscar un archivo en el directorio y descargarlo de forma concurrente desde todos los pares que lo tengan alojado en la red, balanceando la carga.
- **Verificación de Integridad:** Todas las descargas implementan validación final mediante el algoritmo hash **SHA-256** para evitar la persistencia de archivos corruptos.
- **Servidor Concurrente:** Cada Peer puede levantar un servidor TCP en segundo plano capaz de atender peticiones de descargas de múltiples clientes de forma simultánea (multihilo).

## 🛠️ Estructura del Proyecto

El código fuente está estructurado en los siguientes paquetes principales (`src/es/um/redes/nanoFiles/`):
* `application/`: Punto de entrada de la aplicación P2P.
* `logic/`: Lógica de control para interactuar con el directorio y otros peers.
* `shell/`: Intérprete de comandos por consola.
* `tcp/`: Clases para la creación del servidor y cliente TCP, además de los mensajes binarios (`PeerMessage`).
* `udp/`: Clases para el servidor de directorio y conector UDP, además de mensajes (`DirMessage`).
* `util/`: Utilidades matemáticas, hashing (SHA-256) y manejo de archivos.

## 💻 Instrucciones de Uso

Para probar la red, necesitas ejecutar al menos un Directorio y uno o más clientes NanoFiles. Puedes utilizar los archivos `.jar` ya compilados con Java 21.

### 1. Iniciar el Servidor de Directorio
Abre una terminal y ejecuta el directorio (escuchará peticiones UDP):
```bash
java -jar Directory.jar
```

### 2. Iniciar un Nodo NanoFiles (Peer)
Abre otra terminal (o varias si quieres simular varios usuarios) y arranca el programa:
```bash
java -jar NanoFiles.jar
```
*(Opcional: puedes indicar el nombre de tu carpeta compartida al ejecutar: `java -jar NanoFiles.jar mi_carpeta`)*

## ⌨️ Comandos Disponibles en NanoFiles

Una vez iniciada la consola de NanoFiles, tienes a tu disposición los siguientes comandos:

**Interacción con el Directorio (UDP):**
* `ping`: Comprueba si el directorio está vivo y es compatible.
* `dirfiles`: Solicita la lista de todos los archivos públicos alojados en el servidor del directorio.
* `dirdl <hash>`: Descarga un archivo desde el directorio a tu disco duro.
* `peers`: Lista todos los usuarios (peers) registrados en el directorio como servidores.

**Funciones Locales y de Red P2P (TCP):**
* `serve`: Levanta un servidor TCP en segundo plano y te inscribe en el directorio para que otros puedan descargar tus archivos.
* `myfiles`: Muestra los archivos que tú estás compartiendo.
* `peerfiles <nickname>`: Pide directamente a un usuario la lista de los archivos que tiene.
* `peerdl <nickname> <hash>`: Descarga el archivo de un usuario concreto.
* `peerdl * <hash>`: ¡Descarga el archivo aprovechando todos los usuarios que lo tengan compartido a la vez!
* `quit`: Finaliza tu servidor, te da de baja en el directorio y cierra la aplicación.

---
*Desarrollado para el curso 2025/2026 de Redes de Comunicaciones.*
