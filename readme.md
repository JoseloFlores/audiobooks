📚 Audiobook-Stack-RPi: "A Prueba de Dummies" 🔊
Este proyecto permite desplegar un ecosistema completo para la descarga, gestión y reproducción de audiolibros en una Raspberry Pi (probado en Debian 13). Está diseñado para que cualquier miembro de la familia pueda descargar libros y escucharlos directamente en una Smart TV con el control remoto.

🚀 Características
Descarga Simplificada: Instancia dedicada de Transmission (Puerto 9091) configurada para descargar directamente a la biblioteca.

Reproducción en TV: Jellyfin configurado para servir audiolibros a la app oficial de de tu Smart TV.

Gestión Avanzada: Audiobookshelf para organizar metadatos, series y unir (merge) libros de múltiples archivos.

Persistencia de lectura: El sistema recuerda exactamente dónde te quedaste en cada libro.

---

### 🔒 Advertencia de Seguridad
Este proyecto está diseñado para ser simple y usarse en una red local y privada. **No exponga los servicios directamente a Internet sin tomar precauciones.**

- **Contraseñas:** Ninguno de los servicios viene con una contraseña por defecto. Accede a la configuración de cada uno (Transmission, Jellyfin, Audiobookshelf) y establece contraseñas seguras.
- **Acceso Remoto:** Si necesitas acceder desde fuera de tu casa, utiliza una VPN (como WireGuard o Tailscale) en lugar de abrir los puertos en tu router.

---

🛠️ Estructura de Carpetas
El proyecto utiliza una ruta base que puedes configurar para separar la configuración de los archivos multimedia. La estructura es la siguiente:

```text
/ruta/que/elijas/
├── audiolibros/              # Los archivos MP3/M4B finales
├── jellyfin/                 # Configuración de Jellyfin
├── audiobookshelf/           # Configuración de Audiobookshelf
└── transmission_libros/      # Configuración del descargador
``` 

📦 Instalación
1. Configurar el entorno:
Crea un archivo llamado `.env` a partir del ejemplo proporcionado:
```bash
vim .env
```
Abre el archivo `.env` y edita la variable `BOOK_STACK_PATH` para que apunte a la ruta donde quieres guardar todo. Por ejemplo:
```
BOOK_STACK_PATH=/home/tu-usuario/audiobooks
```

2. Preparar el sistema:
Crea las carpetas necesarias usando la ruta que definiste en el paso anterior.
```bash
# Lee la variable del archivo .env para no tener que escribirla a mano
export $(grep -v '^#' .env | xargs)

mkdir -p $BOOK_STACK_PATH/audiolibros $BOOK_STACK_PATH/jellyfin/config $BOOK_STACK_PATH/audiobookshelf/config $BOOK_STACK_PATH/transmission_libros/config
sudo chown -R 1000:1000 $BOOK_STACK_PATH
```

3. Desplegar con Docker Compose:
Usa el archivo `docker-compose.yml` para levantar los servicios. Docker leerá automáticamente las variables del archivo `.env`.
```bash
cd /home/tu-usuario/audiobook
docker-compose.yml up -d
```

Puertos configurados:

- Transmission (Descargas): http://tu-ip-raspberry:9091
- Jellyfin (TV/Media): http://tu-ip-rpi:8096
- Audiobookshelf (Gestión): http://tu-ip-rpi:13378

📖 Guía de Uso (Flujo de trabajo)
1. Descarga
Entrar a la interfaz de Transmission (puerto 9091), pegar el enlace magnet o subir el archivo .torrent. Los archivos se guardarán automáticamente en la carpeta de la biblioteca.

2. Organización (Opcional pero recomendado)
Si un libro viene en muchas partes (múltiples MP3), entrar a Audiobookshelf http://tu-ip-raspberry:13378 y usar la función "Merge" para convertirlos en un solo archivo .m4b. Esto facilita la navegación en la TV.

3. Reproducción
Abrir la app de Jellyfin en la Smart TV . Los libros aparecerán con su carátula y descripción. Seleccionar y dar a Play.

⚠️ Notas Técnicas
- Permisos: Si los contenedores no pueden escribir archivos, verifica los permisos de la carpeta definida en `BOOK_STACK_PATH`.
- Firewall: Asegurarse de tener abiertos los puertos 8096, 9092 y 13378 en el sistema (ufw allow).
- Jellyfin en Smart TV: Configurar la biblioteca como tipo "Música" y activar la opción de "Reproducción continua" para no perder el progreso.

Hecho con ❤️ para que tu no deje de leer (ni de escuchar).
¡Gracias totales!
