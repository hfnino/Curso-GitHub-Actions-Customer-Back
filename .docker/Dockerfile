# Use official OpenJDK 17 image as the base image "eclipse-temurin:17-alpine" esta disponible dentro del 
# repositorio de Docker Hub y es una imagen ligera basada en Alpine Linux.
FROM eclipse-temurin:17-alpine

# Create a new group with specific uid and non-root user called "admin"
#Be sure that group id are not present on host. if already exist change by arbitrary other uid
RUN addgroup -g 1028 devopsc \
    && adduser -D -G devopsc admin

#Create a new mount point at /tmp on native hostbecause a volume is mor efficient and speed that filesystem
# configuramos /tmp como directorio de trabajo como un volumen para almacenar archivos temporales
VOLUME /tmp

# El jar se copiará en el contenedor en la ruta /tmp/app.jar
# Usamos ARG para pasar el nombre del archivo JAR y la contraseña de la base de datos
# Estos valores se pueden proporcionar al construir la imagen Docker.
# Esto permite que el archivo JAR y la contraseña de la base de datos se pasen
# dinámicamente al construir la imagen, lo que es útil para diferentes entornos de despliegue.
# Esto es útil para que el contenedor pueda acceder al archivo JAR y a la contraseña
# de la base de datos sin necesidad de codificarlos directamente en la imagen.
# Esto es una buena práctica de seguridad para evitar exponer información sensible en la imagen.
# DB_PASSWORD es una variable de entorno que se puede usar para pasar la contraseña de la base de datos al contenedor.
# JAR_FILE es una variable de entorno que se puede usar para pasar el nombre del archivo
ARG JAR_FILE
ARG ARG DB_PASSWORD
COPY ${JAR_FILE} /tmp/app.jar

# Aqui definimos una variable de entorno llamada DB_PASSWORD y toma el valor del argumento ARG DB_PASSWORD que definimos
# anteriormente. # Esto permite que la contraseña de la base de datos se pase al contenedor
# como una variable de entorno, lo que es útil para que la aplicación pueda acceder a ella
# sin necesidad de codificarla directamente en el código fuente o en la imagen.
# Esto es una buena práctica de seguridad para evitar exponer información sensible en la imagen.
ENV DB_PASSWORD=$DB_PASSWORD

# Change ownership of the /app directory to the "admin" user
# Aqui estamos cambiando la propiedad del directorio /tmp al usuario "admin" y al grupo "devopsc"
# Esto es importante para que el usuario no root pueda acceder y ejecutar el archivo JAR
# y para evitar problemas de permisos al ejecutar la aplicación.
# Esto es una buena práctica de seguridad para evitar ejecutar aplicaciones como root.
RUN chown -R admin:devopsc /tmp

# Switch to non-root "admin" user for added security
# Cambiamos al usuario "admin" para ejecutar la aplicación a partir de aquí
# Esto es una buena práctica de seguridad para evitar ejecutar aplicaciones como root.
USER admin

# Ejecutamos el jar al iniciar el contenedor
ENTRYPOINT ["java","-jar","/tmp/app.jar"]














