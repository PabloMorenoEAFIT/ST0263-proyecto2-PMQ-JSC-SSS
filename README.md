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

- Cluster de kubernetes implementando microk8s
- Dominio propio: 
- Configuracion de certificado SSL
- 6 nodos creados
  - Master
  - Wordpress 1
  - Wordpress 2
  - NFS
  - MYSQL 1
  - MYSQL 2

  
---
### 1.2 Aspectos no cumplidos
- Aspectos funcionales de la entrega
---
### 2. Arquitectura implementada
![image](https://github.com/user-attachments/assets/e4c27ff4-6605-4eb6-958c-bfe0da16c24f)

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
sudo usermod -a -G microk8s <user>
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
Descargar el archvio .yaml ubicado en el repositorio 
El archivo debe estar en el formato de kubernetes y no de docker
(para nuestro caso es wordpress-deployment.yaml)
```bash
wget https://raw.githubusercontent.com/PabloMorenoEAFIT/ST0263-proyecto2-PMQ-JSC-SSS/refs/heads/main/wordpress-deployment.yaml
```

Aplicar el archivo en el cluster
```bash
kubectl apply -f wordpress-deployment.yaml
```

Verificar que los pods se esten ejecutando en los nodos dedicados
```bash
kubectl get pods -o wide
```

### 3.2.3 MYSQL 1 y 2
Descargar el archvio .yaml ubicado en el repositorio 
El archivo debe estar en el formato de kubernetes y no de docker
(para nuestro caso es mysql-deployment.yaml)
```bash
wget https://raw.githubusercontent.com/PabloMorenoEAFIT/ST0263-proyecto2-PMQ-JSC-SSS/refs/heads/main/mysql-deployment.yaml
```

Aplicar el archivo en el cluster
```bash
kubectl apply -f mysql-deployment.yaml
```

Verificar que los pods se esten ejecutando en los nodos dedicados
```bash
kubectl get pods -o wide
```

Verificar los Persistent Volumes (PVs) y Persistent Volume Claims (PVCs)
```bash
wget https://raw.githubusercontent.com/PabloMorenoEAFIT/ST0263-proyecto2-PMQ-JSC-SSS/refs/heads/main/persistent-volume.yaml
```

Aplicar el archivo en el cluster
```bash
kubectl apply -f persistent-volume.yaml
```

### 3.2.4 NFS
Instalar el servidor NFS
```bash
sudo apt-get install nfs-kernel-server
```

Configurar el directorio
```bash
sudo vim /etc/exports

# Modificar el archivo agregado la sigueinte linea:
# /srv/nfs/wordpress <IP-Wordpress-1>(rw,sync,no_subtree_check) <IP-Wordpress-2>(rw,sync,no_subtree_check)
```

Exportar los directorios
```bash
sudo exportfs -ra
```

Iniciar y habilitar el servicio
```bash
sudo systemctl start nfs-server
```

En caso de tener restricciones de Firewall por la VPC, para permitir el acceso del NFS (usando ufw)
```bash
sudo ufw allow from <IP-wordpress> to any port nfs
```

Agregar el servicio al cluster
```bash
wget https://raw.githubusercontent.com/PabloMorenoEAFIT/ST0263-proyecto2-PMQ-JSC-SSS/refs/heads/main/nfs-pod.yaml

kubectl apply -f nfs-pvc.yaml
```
