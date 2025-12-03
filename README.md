# 📡Chat-Bidireccional-Python

Este proyecto implementa un **chat cliente-servidor en tiempo real** usando:
- **Sockets** para comunicación bidireccional.
- **Programación orientada a objetos (POO)** en Python.
- **Interfaz gráfica con Tkinter** (simple y funcional).
- Hilos (`threading`) para evitar bloqueos en la UI.

Ideal para entender los fundamentos de redes y programación concurrente.

---

## 📁 Estructura del Proyecto
chat_app/

├── server.py # Clase ChatServer: gestiona conexiones y mensajes entrantes/salientes

├── client.py # Clase ChatClient: se conecta al servidor y envía/recibe mensajes

└── README.md # Este archivo

---

## ▶️ Cómo Ejecutar

1. **Inicia el servidor** (espera conexiones):
   ```bash
   python server.py
   ```
2. **Inicia el cliente** (se conecta automáticamente):
   ```bash
   python client.py
   ```
3. Escribe mensajes y presiona **Enter** o haz clic en **Enviar**.
   
   → Los mensajes aparecen en tiempo real en ambas ventanas.

### 🔌 Métodos de Sockets Usados (Explicados)

A continuación se describen los métodos clave del módulo `socket` utilizados en este proyecto, con su propósito y contexto de uso:

- **`socket.socket(family, type)`**  
  *Uso*: Creación inicial del socket (tanto en servidor como cliente).  
  *Ejemplo*: `socket.socket(socket.AF_INET, socket.SOCK_STREAM)`  
  *Detalles*:  
  - `AF_INET`: indica IPv4.  
  - `SOCK_STREAM`: protocolo TCP (conexión orientada, fiable).

- **`.bind((host, port))`**  
  *Uso*: Solo en `server.py`.  
  *Detalles*: Asocia el socket del servidor a una dirección IP y número de puerto local. Necesario antes de escuchar.

- **`.listen(backlog)`**  
  *Uso*: Solo en `server.py`, tras `bind()`.  
  *Detalles*: Configura el socket para aceptar conexiones entrantes. El parámetro `backlog` define cuántas conexiones pueden esperar en cola (aquí: `1`).

- **`.accept()`**  
  *Uso*: Solo en `server.py`, dentro de un hilo.  
  *Detalles*: Método **bloqueante** que espera hasta que un cliente se conecte. Retorna una tupla:  
  ```python
  (client_socket, client_address)
  ```

- **`.connect((host, port))`**  
  *Uso*: Solo en `client.py`, inmediatamente después de crear el socket.  
  *Detalles*: Inicia la conexión TCP con el servidor especificado. Realiza el *handshake* de 3 vías (SYN → SYN-ACK → ACK). Lanza una excepción (ej. `ConnectionRefusedError`) si el servidor no está escuchando.

- **`.send(data)`**  
  *Uso*: En `server.py` y `client.py`.  
  *Detalles*: Envía datos en **bytes**. Requiere codificación previa (ej. `"mensaje".encode('utf-8')`). No garantiza entrega inmediata, pero TCP asegura que llegue intacto. Lanza excepción si la conexión está rota.

- **`.recv(bufsize)`**  
  *Uso*: En `server.py` y `client.py`, dentro de un hilo dedicado.  
  *Detalles*: Método **bloqueante** que espera hasta recibir datos (máximo `bufsize` bytes). Devuelve `b''` si la conexión fue cerrada por el otro extremo. Siempre se usa con `.decode('utf-8')` para obtener texto legible.

- **`.close()`**  
  *Uso*: En `server.py` y `client.py`, al finalizar la conexión o detectar fallos.  
  *Detalles*: Cierra el socket de forma ordenada, liberando el puerto y recursos del sistema. En TCP, desencadena el *handshake* de cierre (FIN/ACK). Es una buena práctica invocarlo siempre, incluso tras excepciones.
