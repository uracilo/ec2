# ec2

# Configuración de Auto Scaling y Elastic Load Balancer (ELB) en AWS para Instancias de Backend

Este proyecto configura una arquitectura escalable para instancias de backend usando **Auto Scaling** y un **Elastic Load Balancer (ALB)** en AWS. A continuación, se detallan los pasos para configurar, probar y optimizar esta infraestructura.

## Paso 1: Preparación de las Instancias Backend

### 1.1 Crear la AMI de la Instancia Backend
- Asegúrate de que la instancia backend esté configurada correctamente (puedes usar una AMI existente o crear una nueva si has personalizado la instancia).
- La AMI debe contener todo el software necesario para ejecutar tu aplicación en el backend.

### 1.2 Configuración de Seguridad
- **Grupos de seguridad:** Asegúrate de configurar el grupo de seguridad para que permita el tráfico entre las instancias frontend y backend (por ejemplo, abriendo el puerto 8080 en el backend).
- **Subredes:** Las instancias de backend deben estar en una subred privada para protegerlas de accesos no autorizados.

## Paso 2: Configuración de Auto Scaling

### 2.1 Crear un Auto Scaling Group (ASG)
1. **Accede a la consola de Auto Scaling:**
   - Dirígete a la consola de **Auto Scaling** en AWS.
   - Crea un nuevo **Auto Scaling Group** para la instancia de backend.

2. **Configuración de la Launch Configuration:**
   - Utiliza la **Launch Configuration** basada en la AMI de la instancia de backend que creaste en el paso anterior.
   - Define las configuraciones necesarias como tipo de instancia, clave SSH, y grupo de seguridad.

3. **Configuración de Mínimo y Máximo de Instancias:**
   - Establece el mínimo de instancias como **1** y el máximo como **3** para permitir el escalado según la carga.
   - Especifica que las nuevas instancias deben lanzarse en la **subred privada**.

### 2.2 Configurar el Escalado Automático
1. **Crear políticas de escalado:** Configura políticas de escalado para ajustar el número de instancias en función de métricas de uso, como el **uso de CPU** o **tráfico de red**.
   - Por ejemplo, puedes configurar el escalado para que aumente el número de instancias cuando el uso de CPU supere el 70%.
   - Igualmente, puedes reducir el número de instancias cuando la carga disminuya.

## Paso 3: Configuración del Elastic Load Balancer (ELB)

### 3.1 Crear un Application Load Balancer (ALB)
1. **Accede a la consola de EC2 > Load Balancers:**
   - Crea un nuevo **Application Load Balancer (ALB)**.
   - Configura el ALB para que escuche en el puerto **80 (HTTP)**.
   - Asegúrate de asociar el ALB a **subredes públicas** para que pueda recibir tráfico externo.

2. **Configuración del Target Group:**
   - Crea un **Target Group** que apunte a las instancias de backend en la subred privada.
   - Configura la **salud del target** para que monitoree el puerto de backend (por ejemplo, 8080) y determine si las instancias están en estado saludable.

### 3.2 Registrar las Instancias de Backend con el ALB
1. **Asociar Instancias al Target Group:**
   - Registra las instancias de backend en el Target Group creado en el paso anterior para que el ALB pueda distribuir el tráfico entre ellas.

## Paso 4: Probar la Configuración

### 4.1 Acceso a la Página de Frontend
- Visita la **IP pública** de la instancia frontend en tu navegador.
- La página web del frontend debería realizar solicitudes al backend, que serán redirigidas a través del ALB.

### 4.2 Verificación de Escalado Automático
1. **Generar Tráfico en el Frontend:**
   - Realiza múltiples peticiones al frontend para generar tráfico.
2. **Monitorear Auto Scaling:**
   - Accede a la consola de Auto Scaling para verificar si el número de instancias del backend aumenta automáticamente en respuesta a la carga.

## Paso 5: Optimización de Costos

### 5.1 Ajustar Tipos de Instancias
- **Revisar el rendimiento de las instancias EC2:** Evalúa si las instancias backend están utilizando más recursos de los necesarios.
- Si las instancias tienen una carga baja, prueba con tipos de instancias más pequeñas (por ejemplo, **t3.micro** a **t3.nano**) para reducir costos.

### 5.2 Uso de Spot Instances
- Para cargas de trabajo menos críticas, considera usar **Spot Instances** para las instancias de backend, lo que puede reducir significativamente los costos.
- Ajusta el precio máximo que estás dispuesto a pagar por las Spot Instances según las fluctuaciones del mercado.

## Consideraciones Finales

- **Seguridad:** Asegúrate de que las instancias de backend solo sean accesibles a través del ALB y que los grupos de seguridad estén configurados correctamente.
- **Monitoreo:** Usa Amazon CloudWatch para monitorear el rendimiento de tus instancias y los triggers de escalado automático.
- **Costo:** Utiliza **AWS Cost Explorer** para seguir de cerca los costos asociados con las instancias y los servicios involucrados.

## Recursos Adicionales

- [Documentación de Auto Scaling](https://docs.aws.amazon.com/autoscaling/latest/userguide/)
- [Guía de Elastic Load Balancer](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/)
- [Optimización de Costos en AWS](https://aws.amazon.com/es/aws-cost-management/)
