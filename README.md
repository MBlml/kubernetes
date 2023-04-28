# Kubernetes
#### BARAJAS GOMEZ JUAN MANUEL | 216557005 | COMPUTACION TOLERANTE A FALLAS | 24/04/2023

### OBJETIVO:
Crea un ejemplo con el uso de las herramientas solicitadas por el profesor.

### DESARROLLO:
¿Qué es Kubernetes?
Kubernetes es un sistema de orquestación de contenedores de código abierto que facilita el despliegue, la gestión y la escalabilidad de aplicaciones en contenedores. Fue desarrollado por Google y actualmente es mantenido por la Cloud Native Computing Foundation (CNCF). Con Kubernetes, los desarrolladores pueden desplegar y gestionar aplicaciones en contenedores en múltiples máquinas y clusters, con alta disponibilidad y escalabilidad.

¿Qué es Ingress?
Ingress es un recurso en Kubernetes que proporciona una capa de abstracción para exponer servicios HTTP y HTTPS dentro del clúster de Kubernetes. Permite a los usuarios definir reglas de enrutamiento basadas en el host y la URL del cliente y redirigir el tráfico a diferentes servicios dentro del clúster. Ingress también admite la configuración de SSL para conexiones seguras.

¿Qué es un LoadBalancer?
Un LoadBalancer es un recurso en Kubernetes que permite distribuir el tráfico de red entrante entre múltiples réplicas de un conjunto de pods. Un LoadBalancer expone un servicio a Internet o a una red externa, y distribuye automáticamente el tráfico entre las diferentes réplicas del pod que ejecuta la aplicación. Esto mejora la escalabilidad, la disponibilidad y el rendimiento de las aplicaciones, ya que distribuye la carga de trabajo entre múltiples nodos del clúster de Kubernetes. Los servicios de LoadBalancer también pueden proporcionar balanceo de carga de capa 4 y capa 7.

### EJEMPLO:
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-node-app
spec:
  selector:
    matchLabels:
      app: my-node-app
  replicas: 3
  template:
    metadata:
      labels:
        app: my-node-app
    spec:
      containers:
        - name: my-node-app
          image: <docker-image-name>
          ports:
            - containerPort: 3000
---
apiVersion: v1
kind: Service
metadata:
  name: my-node-app-service
spec:
  selector:
    app: my-node-app
  ports:
    - name: http
      protocol: TCP
      port: 80
      targetPort: 3000
  type: LoadBalancer
---
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: my-node-app-ingress
spec:
  rules:
    - host: my-node-app.example.com
      http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: my-node-app-service
                port:
                  name: http
```

Aqui se debe reemplazar <docker-image-name> con el nombre de la imagen de Docker de nuestra aplicación.

Guardamos el archivo con el nombre correspondiente o deseado.
  
Finalmente aplicamos la configuracion anterior en Kubernetes:
  
```perl
kubectl apply -f my-node-app.yaml
```

Debemos esperar a que la aplicación se despliegue en el clúster de Kubernetes.

Accedemos a la aplicación a través de la dirección IP del balanceador de carga (que se puede encontrar en la salida de kubectl get services) o a través de my-node-app.example.com (Nombre del archivo utilizado) al configurar un registro DNS y una entrada de hosts.

En este ejemplo creamos un despliegue de tres réplicas de la aplicación Node.js, un servicio para exponer la aplicación a través de un balanceador de carga y un Ingress para enrutar el tráfico a la aplicación. Debemos tener en cuenta que este es solo un ejemplo básico y que hay muchas formas diferentes de configurar una aplicación Node.js en Kubernetes dependiendo de las necesidades requeridas anteriormente.

### CONCLUSIÓN:
En resumen, Kubernetes, Ingress y LoadBalancer son herramientas valiosas para la implementación y gestión de aplicaciones en contenedores. Estas herramientas permiten una gestión eficiente de los recursos de la aplicación, una escalabilidad fácil y una alta disponibilidad de la misma. Además, son altamente personalizables y permiten a los desarrolladores automatizar el despliegue de aplicaciones y simplificar la gestión de la infraestructura. En general, el uso de estas herramientas puede ayudar a mejorar la eficiencia y la confiabilidad del desarrollo y despliegue de aplicaciones en entornos de contenedores y nubes públicas o privadas.


### REFERENCIAS:
_ _
