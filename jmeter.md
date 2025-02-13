# Carga Masiva de Peticiones GET con Apache JMeter

Este documento describe los pasos para configurar y ejecutar una carga masiva de peticiones GET a la IP `http://204.236.218.67/` utilizando **Apache JMeter** en Ubuntu.

---

## **1. Instalación de JMeter en Ubuntu**

1. **Actualizar el sistema**
   ```bash
   sudo apt update && sudo apt upgrade -y
   ```
2. **Instalar Java** (requerido para JMeter)
   ```bash
   sudo apt install openjdk-11-jdk -y
   ```
   Verificar la instalación:
   ```bash
   java -version
   ```
3. **Descargar Apache JMeter**
   ```bash
   wget https://dlcdn.apache.org//jmeter/binaries/apache-jmeter-5.6.2.tgz
   ```
4. **Extraer el archivo**
   ```bash
   tar -xvzf apache-jmeter-5.6.2.tgz
   ```
5. **Mover JMeter a /opt/jmeter (opcional)**
   ```bash
   sudo mv apache-jmeter-5.6.2 /opt/jmeter
   ```
6. **Configurar la variable de entorno**
   ```bash
   echo 'export JMETER_HOME=/opt/jmeter' >> ~/.bashrc
   echo 'export PATH=$JMETER_HOME/bin:$PATH' >> ~/.bashrc
   source ~/.bashrc
   ```
7. **Verificar instalación**
   ```bash
   jmeter -v
   ```

---

## **2. Configurar Prueba de Carga en JMeter**

### **Crear un Plan de Pruebas**
1. Abrir JMeter en modo gráfico:
   ```bash
   jmeter
   ```
2. Agregar un **Thread Group**:
   - `Test Plan` → `Añadir` → `Threads (Users)` → `Thread Group`
   - Configurar:
     - **Number of Threads (Usuarios simulados):** `100`
     - **Ramp-Up Period (Tiempo de escalado):** `60`
     - **Loop Count (Repeticiones):** `10`

3. Agregar un **HTTP Request Sampler**:
   - `Thread Group` → `Añadir` → `Sampler` → `HTTP Request`
   - Configurar:
     - **Server Name or IP:** `204.236.218.67`
     - **Port Number:** (vacío si es puerto 80)
     - **Protocol:** `http`
     - **Method:** `GET`
     - **Path:** `/`
     - **Number of Retries:** `0`

4. Agregar un **Listener** para ver los resultados:
   - `Añadir` → `Listener` → `View Results in Table` o `Summary Report`

---

## **3. Ejecutar la Prueba**

1. Presionar el botón **Start (Play)** en JMeter.
2. Monitorear los resultados en los listeners agregados.

---

## **4. Ejecutar en Modo CLI (headless)**
Si deseas ejecutar la prueba sin interfaz gráfica:
1. Guardar el plan como `carga-get.jmx`.
2. Ejecutar en línea de comandos:
   ```bash
   jmeter -n -t carga-get.jmx -l resultados.jtl
   ```

---

## **5. Consideraciones**
- Asegúrate de que la IP **permite pruebas de carga**.
- **No realices ataques sin autorización**, ya que un tráfico excesivo puede ser considerado un **DDoS**.
- Usa "Constant Throughput Timer" si necesitas controlar la tasa de peticiones.

---

¡Listo! Ahora puedes realizar pruebas de carga con **JMeter** en Ubuntu.

