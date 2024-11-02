# Workshop Distributed - Simple Chat Application

Este proyecto es una aplicación de chat distribuido creada en Java usando JGroups. Permite que múltiples instancias se comuniquen entre sí en un entorno distribuido, formando un clúster de chat. Cada usuario puede enviar y recibir mensajes en tiempo real desde el clúster, con funcionalidades como el almacenamiento de mensajes previos y la detección de la conexión de nuevos miembros.

# Tabla de contenidos
- # Requisitos
- # Instalación
- # Configuración
- # Ejecución
- # Uso
- # Detalles Técnicos
- # Test en netbeans
- # Test en EC2

# Requisitos
- **Java 21**: Asegúrate de tener JDK 21 instalado y configurado en tu sistema.
- **Apache Maven**: Herramienta de construcción para manejar dependencias y crear el proyecto.

# Instalación
1. Clona este repositorio en tu máquina local:
   ```
   git clone https://github.com/edwardfranciaescuelaing/workshop_distributed
   cd workshop_distributed
   ```

2. Asegúrate de tener el archivo `pom.xml` configurado correctamente. Este archivo ya incluye las dependencias necesarias, como JGroups, y define la clase principal del proyecto.

3. Compila el proyecto usando Maven:
   ```
   mvn clean install
   ```

# Configuración
El proyecto usa JGroups para gestionar la conectividad entre los nodos. JGroups se encarga de las configuraciones de red automáticamente, pero es posible que necesites ajustes adicionales en un entorno de red complejo o si estás ejecutando el proyecto en diferentes instancias de EC2 en AWS.

1. **Red y Seguridad**: Asegúrate de que los puertos de comunicación necesarios estén abiertos en los firewalls y configuraciones de seguridad, ya que JGroups se comunica a través de la red para conectar los nodos.
2. **Nombre de Usuario**: El nombre de usuario utilizado en el chat se toma del sistema mediante `System.getProperty("user.name")`. Si deseas personalizarlo, puedes establecer esta propiedad al iniciar la aplicación.

# Ejecución
Para iniciar el chat distribuido, ejecuta el comando:

```
mvn exec:java -Dexec.mainClass="com.mycompany.workshop_distributed.SimpleChat"
```

Cada instancia de este programa se conectará al clúster de chat llamado `ChatCluster`, que es el nombre configurado en el código. Una vez conectada, cada instancia recibirá y enviará mensajes a todas las demás.

# Uso
1. **Inicio**: Al iniciar, la aplicación se conecta automáticamente al clúster `ChatCluster` e intenta recuperar el historial de mensajes previos del grupo.
2. **Envío de Mensajes**: Escribe cualquier mensaje en la consola y presiona Enter para enviarlo a los demás miembros del clúster.
3. **Salir del Chat**: Para salir del chat, escribe `quit` o `exit` en la consola.

# Comandos Básicos
- **quit** o **exit**: Cierra la aplicación.

# Detalles Técnicos

El código está basado en la biblioteca **JGroups** y tiene las siguientes partes clave:

1. **Clase Principal (`SimpleChat`)**:
   - Implementa la interfaz `Receiver` para manejar eventos de la red.
   - Contiene métodos para conectar el chat, gestionar la recepción de mensajes y sincronizar el estado de mensajes entre nodos.

2. **Métodos Importantes**:
   - `start()`: Inicializa el canal de comunicación y conecta el nodo al clúster.
   - `eventLoop()`: Proporciona un bucle de entrada para recibir comandos del usuario.
   - `viewAccepted(View new_view)`: Notifica cuando hay un cambio en el grupo, como cuando un nuevo miembro se une o sale.
   - `receive(Message msg)`: Recibe mensajes de otros nodos y los muestra en la consola.
   - `getState(OutputStream output)` y `setState(InputStream input)`: Manejan la recuperación y sincronización del estado del chat entre los nodos, asegurando que todos los participantes tengan el mismo historial de mensajes.

3. **Gestión del Estado**:
   - Los mensajes se almacenan en una lista `state` y se sincronizan con otros nodos cuando uno nuevo se une. Esto permite que cada nuevo miembro vea el historial del chat.

# Test en netbeans
Para una primera prueba en local lo que se hace es abrir el proyecto en el IDE esccogido en mi caso es netbeans.
  - Ir a la parte izquierda del proyecto y dar click derecho sobre la clase SimpleChat y seguidamente en Run File.
    
  ![image](https://github.com/user-attachments/assets/6caea8fa-060c-4f97-92dd-c95e7f83633c)
  ![image](https://github.com/user-attachments/assets/52033671-830b-417c-a94f-a17650159cbb)

  - El proyecto correra en una terminal de netbeans de salida y cargara el chat para el user 1.
    
  ![image](https://github.com/user-attachments/assets/af35cf13-b135-4c03-ad8d-d2104871dacf)

  - Se repite el proceso para el user 2 y se debera cargar el historial previamente indexado por el chat 1 del user 1; y adicional todo lo que se escriba debera verse en el user 1.
    
  ![image](https://github.com/user-attachments/assets/647f8694-75d2-4a8b-bde4-a68b855029f0)
  ![image](https://github.com/user-attachments/assets/dc9d63e6-7912-4ac3-9568-cce04078aa13)

  - Finalmente si ejecutamos una tercera vez se debera ver el historial de la coversación en el chat 3.
    
  ![image](https://github.com/user-attachments/assets/151b0953-4b58-400a-98f2-64d1fca2a4dd)

# Test en EC2
  1. Inicia una instancia EC2 con Amazon Linux 2 y conéctate a ella usando SSH.
2. Instala las herramientas necesarias (YUM, Java, Git y Maven):
   ```
   sudo yum update -y
   sudo amazon-linux-extras install java-openjdk21 -y
   sudo yum install git maven -y
   ```
3. Clona el repositorio desde el link:
   ```
   git clone https://github.com/edwardfranciaescuelaing/workshop_distributed
   cd workshop_distributed
   ```
4. Configura la variable `JAVA_HOME` y construye el proyecto:
   ```
   export JAVA_HOME=/usr/lib/jvm/java-21-amazon-corretto
   mvn clean install
   ```
5. Ejecuta la aplicación:
   ```
   mvn exec:java -Dexec.mainClass="com.mycompany.workshop_distributed.SimpleChat"
   ```
6. Finalmente se puede repetir el proceso en diferentes accesos de usuarios por ssh a la instacia EC2 y podran interactuar entre si
![image](https://github.com/user-attachments/assets/6fb60035-8a7f-41c3-9a89-05fbc6a2f21a)


