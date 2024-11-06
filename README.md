# Proyecto 2 
---

### **Curso:** ST0263 - Tópicos Especiales en Telemática
### **Título:** Proyecto 2 
### **Objetivo:**

Desplegar una aplicación en un clúster Kubernetes de alta disponibilidad utilizando la distribución MicroK8s.

### **Estudiantes:**
- Pablo Moreno Quintero
- Samuel Salazar Salazar
- Juan Sebastian Camacho

### Profesor: 
Edwin Montoya - emontoya@eafit.edu.co
---
### LINK DE LA SUSTENTACIÓN

---
### 1. Descripción de la actividad

Se hizo un cluster de Kubernetes de alta disponibilidad utilizando kubernetes en la plataforma GCP con una infraestructura de Infraestructura Como Servicio (IaaS). 

El proyecto se basa en un CMS desplegado en un cluster de kubernetes creados bajo microk8s, donde para nuestro caso se dispone de un mínimo de 3 máquinas virtuales autoescalables.
---
### 1.1 Aspectos cumplidos del proyecto

-  Cluster de kubernetes implementando microk8s
- Dominio propio
- Configuracion de certificado SSL
-  6 nodos creados
  - Master
  - Wordpress 1
  - Wordpress 2
  - NFS
  - MYSQL 1
  - MYSQL 2

  
---
### 1.2 Aspectos no cumplidos

---
### 2. Arquitectura implementada

---
### 3. Ambiente de desarrollo

- Cluster configurado con microk8s
- 5 instancias creadas
  - Instancias tipo e2-large (40gb de memoria)
  - 1 master
  - 2 Wordpress
  - 1 NFS
  - 1 MYSQL
 
Cada instancia está configurada para ser autoescalable según demanda del sistema.

---
### 3.1 Ejecución
Configuraciones del cluster (Aplicable para cada nodo)

```bash 
sudo snap install microk8s --classic
sudo usermod -a -G microk8s pmorenoq
mkdir -p ~/.kube
sudo chown -R $(whoami) ~/.kube
newgrp microk8s
sudo microk8s config > ~/.kube/config
```
---
### 3.2 Desarrollo de los nodos
### 3.2.1 Master
creación del cluster
```bash
newgrp microk8s
```

Iniciar el cluster
```bash
microk8s status --wait-ready
```

Se debe esperar a que muestre información del cluster (cuales servicios están habilitados y el estado general)
```bash
microk8s enable <service>
```


Reiniciar el servicio
```bash
microk8s stop
microk8s start
```

Revisar que no hayan nodos en el cluster
```bash
microk8s kubectl get no

# En caso de encontrar una dirección no deseada
microk8s remove-node <Ip-a-borrar>
```

Crear 'Tokens' de acceso al cluster 
Cada nodo adicional debe ingresar con un toke único al cluster
```bash
microk8s add-node

# Debe retornar algo como lo siguiente:
# 'From the node you wish to join to this cluster, run the following:'
# 'microk8s join <IP-Master-Node>:25000/<KEY>'
```



### 3.2.2 Slave 1 y 2 (Wordpress)
### 3.2.3 MYSQL 1 y 2
### 3.2.4 NFS

