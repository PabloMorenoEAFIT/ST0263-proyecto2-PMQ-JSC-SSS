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
  - 1 master
  - 2 Wordpress
  - 1 NFS
  - 1 MYSQL
 
Cada instancia está configurada para ser autoescalable según demanda del sistema.

