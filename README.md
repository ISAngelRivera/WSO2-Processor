# WSO2-Processor

Receptor de webhooks desde WSO2 API Manager. Traduce eventos del Publisher a un formato estándar y los envía al GIT-Helix-Processor.

## Responsabilidades

- Recibir webhooks desde botones del Publisher de WSO2
- Exportar API usando `apictl`
- Transformar a payload estándar (vendor-agnostic)
- Invocar al GIT-Helix-Processor

## Flujo

```
WSO2 Publisher (botón)
       │
       ▼
┌─────────────────────┐
│   receive-webhook   │  ◄── Este repo
│      workflow       │
└─────────────────────┘
       │
       ▼
   GIT-Helix-Processor
```

## Payload Estándar de Salida

```json
{
  "action": "register-uat",
  "api": {
    "name": "PizzaAPI",
    "version": "1.0.0",
    "domain": "Informatica",
    "subdomain": "DevOps"
  },
  "artifact": {
    "zip_base64": "...",
    "source": "wso2"
  },
  "user": {
    "id": "usuario",
    "email": "usuario@example.com"
  },
  "timestamp": "2025-01-15T10:30:00Z"
}
```

## Configuración

### Secrets requeridos

| Secret | Descripción |
|--------|-------------|
| `WSO2_APIM_BASE_URL` | URL base del APIM (ej: https://localhost:9443) |
| `WSO2_ADMIN_USERNAME` | Usuario admin de WSO2 |
| `WSO2_ADMIN_PASSWORD` | Contraseña admin de WSO2 |
| `GIT_HELIX_PROCESSOR_TOKEN` | Token para invocar al GIT-Helix-Processor |

## Notas de Diseño

Este repo es **específico de WSO2**. Si en el futuro se migra a otro API Manager (Apigee, Kong, etc.), se creará un nuevo `[Vendor]-Processor` que genere el mismo payload estándar. El GIT-Helix-Processor permanece sin cambios.
