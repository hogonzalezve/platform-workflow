# AppModelA Backstage Template

Esta plantilla permite a los desarrolladores crear requests de aplicaciones usando la Promise **AppModelA** de Kratix a través de Backstage.

## 🎯 **Funcionalidad:**

### **Generación Automática de Nombres Únicos:**
- **Número aleatorio de 2 dígitos** (00-99) se añade al nombre de la app
- **Timestamp único** para el nombre del archivo
- **Previene conflictos** cuando múltiples developers usan el mismo nombre

### **Ejemplo de nombres generados:**
- **App Name:** `mi-app`
- **Unique Name:** `mi-app-47` (con número aleatorio)
- **Filename:** `mi-app-47-20250925-221630.yaml` (con timestamp)

## 📋 **Parámetros del Formulario:**

### **Application Information (Requerido):**
- `appName`: Nombre de la aplicación (solo minúsculas, números, guiones)
- `targetNamespace`: Namespace donde se desplegará la app
- `image`: Imagen Docker a usar

### **Application Configuration:**
- `port`: Puerto de la aplicación (1-65535)
- `replicas`: Número de replicas (1-10)  
- `serviceType`: ClusterIP, NodePort, LoadBalancer
- `imagePullPolicy`: Always, IfNotPresent, Never

### **Gateway Configuration:**
- `gatewayEnabled`: Habilitar acceso externo
- `gatewayHostname`: Dominio para acceso externo
- `gatewayPath`: Ruta URL (default: "/")
- `gatewayTls`: Habilitar HTTPS

### **Storage Configuration:**
- `storageEnabled`: Habilitar almacenamiento persistente
- `storageSize`: Tamaño del volumen (ej: "1Gi")
- `storageMountPath`: Ruta de montaje (default: "/data")
- `storageClass`: Clase de almacenamiento (default: "gp3")

### **Database Configuration:**
- `databaseEnabled`: Solicitar base de datos
- `databaseType`: postgresql, mysql, mongodb
- `databaseStorageGB`: Tamaño en GB (10-1000)
- `databaseInstanceSize`: Tipo de instancia (db.t3.micro, etc.)

### **Deployment Configuration:**
- `requestNamespace`: Namespace para el request de Kratix
  - `default`: Development
  - `development`: Dev Environment  
  - `staging`: Staging Environment
  - `production`: Production Environment

## 🔄 **Flujo del Template:**

1. **Usuario completa** el formulario en Backstage
2. **Template genera** números aleatorios únicos
3. **Template procesa** el skeleton con los parámetros
4. **Template crea** Pull Request en `platform-workflow`
5. **ArgoCD detecta** el nuevo archivo en `kratix/requests/`
6. **Kratix procesa** el request y despliega la aplicación
7. **Recursos aparecen** en `platform-state` para sincronización

## 📁 **Estructura de archivos:**

```
platform-templates/templates/
├── app-model-a-request.yaml          # Template principal
└── skeletons/app-model-a-request/
    └── webapp-2fyq-default-request.yaml  # Template del request
```

## 🚀 **Uso:**

1. **Registrar template** en Backstage:
   - Añadir a `app-config.yaml` de Backstage
   - Apuntar a este directorio

2. **Developers acceden** desde Software Catalog
3. **Completan formulario** con parámetros de su app
4. **Template genera** Pull Request automáticamente
5. **Platform team revisa** y merge el PR
6. **ArgoCD procesa** automáticamente la aplicación

## ⚙️ **Configuración necesaria:**

- **GitHub token** en Backstage para crear PRs
- **ArgoCD** monitoreando `platform-workflow/kratix/requests/`
- **Kratix Promise** AppModelA instalada en el cluster
- **Destinations** configurados para recibir los Works

## 🔗 **Links útiles:**
- ArgoCD: Para ver el estado de despliegue
- Platform State: Para ver recursos generados  
- Pull Request: Para seguimiento del request