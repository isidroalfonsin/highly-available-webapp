
#  Proyecto: Highly Available Web App en AWS

Este proyecto tiene como objetivo mejorar la confiabilidad de una aplicación web mediante la creación de una arquitectura altamente disponible que se extienda por múltiples Zonas de Disponibilidad (AZ), con balanceo de carga, monitoreo y escalado automático.

---

##  Objetivo

Incrementar la **disponibilidad y tolerancia a fallos** de una aplicación web distribuyéndola en varias Availability Zones, usando **autoscaling**, **load balancing** y **monitoreo de estado**.

---

##  Servicios utilizados

| Servicio    | Función                                                                |
|-------------|------------------------------------------------------------------------|
| EC2         | Ejecutar instancias de la aplicación                                   |
| ELB (ALB)   | Balancear carga entre múltiples instancias EC2                         |
| Auto Scaling| Escalar automáticamente las instancias según demanda                   |
| RDS         | Base de datos relacional de backend                                    |
| Route 53    | DNS de alta disponibilidad y baja latencia                             |
| S3          | Contenido estático                                                     |
| CloudFront  | Distribución global del contenido con baja latencia                    |
| CloudWatch  | Monitoreo y visualización de métricas                                  |

---

##  Pasos realizados

1. Despliegue de una instancia EC2 con la aplicación.
2. Creación de un Auto Scaling Group (ASG) vinculado a un nuevo Load Balancer (ALB).
3. Configuración de un Application Load Balancer expuesto a Internet.
4. Selección de 2 Zonas de Disponibilidad y creación de un Target Group para el listener del ALB.
5. Configuración de nuevos Security Groups:
   - Uno para el ALB, permitiendo tráfico HTTP (puerto 80) desde `0.0.0.0/0`.
   - Otro para las instancias EC2, permitiendo solo tráfico desde el Security Group del ALB (mejor práctica de seguridad).
6. Revisión y ajuste de reglas `inbound` y `outbound` en ambos Security Groups.
7. Simulación de fallo: eliminación manual de una instancia EC2.
8. Validación de que el Auto Scaling Group lanza automáticamente una nueva instancia (verificado en el Activity Log).
9. Adición de una tercera Availability Zone al Auto Scaling Group.
10. Aumento de la capacidad deseada del Load Balancer para admitir la nueva instancia.

---

##  Problemas encontrados y cómo se resolvieron

- **Problema:** Tráfico no permitido entre ALB y EC2.
  - **Solución:** Ajuste de reglas de SG para que el tráfico entrante a EC2 esté limitado al SG del ALB.

---

##  Notas adicionales

- IAM roles no detallados.
- Podría expandirse a infraestructura como código en Terraform o CloudFormation.

---

##  Resultado esperado

- Balanceo de carga automático en múltiples zonas de disponibilidad.
- Alta disponibilidad mediante escalado automático y detección de estado.
