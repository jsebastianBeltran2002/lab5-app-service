# Portal de notas · Laboratorio 5

Aplicación de ejemplo del laboratorio **Azure App Service, IaC y CI/CD** del curso
de Cloud Computing y DevOps (PEMU 2026, Escuela Colombiana de Ingeniería Julio
Garavito).

La guía completa del laboratorio está en el documento
`Laboratorio_AppService_CICD.docx`. Este README es solo el mapa del repositorio.

## Estructura

```
src/lib/notas.js        Lógica de negocio pura (sin HTTP) — pruebas unitarias
src/app.js              Aplicación Express — se exporta, no escucha
src/server.js           Arranque del proceso — escucha en process.env.PORT
public/index.html       Página del portal (personalícela con su nombre)
tests/notas.test.js     Pruebas unitarias con Jest
tests/api.test.js       Pruebas de integración con Supertest
scripts/empaquetar.sh   Construye deploy.zip
infra/                  Terraform: grupo de recursos, plan F1 y Web App
.github/workflows/      Pipeline de CI/CD
```

## Comandos

| Comando | Qué hace |
|---|---|
| `npm ci` | Instala dependencias exactas según `package-lock.json` |
| `npm start` | Levanta el servidor en `http://localhost:3000` |
| `npm test` | Ejecuta las pruebas unitarias y de integración |
| `npm run test:cov` | Igual, con reporte de cobertura |
| `bash scripts/empaquetar.sh` | Genera `deploy.zip` listo para App Service |

## Endpoints

| Método y ruta | Respuesta |
|---|---|
| `GET /` | Página del portal |
| `GET /health` | `{ estado, version, uptimeSegundos }` |
| `GET /api/version` | Versión, Node, sitio e instancia que respondió |
| `GET /api/estudiantes` | Lista registrada en memoria |
| `POST /api/estudiantes` | Crea un estudiante; `201` o `400` si los datos no son válidos |
| `GET /api/estudiantes/:id` | Un estudiante; `404` si no existe |

## Despliegue

1. **Infraestructura** (desde su máquina, con `az login`):
   `cd infra && terraform init && terraform apply`
2. **Aplicación**: automática en cada `push` a `main` mediante GitHub Actions,
   usando el secreto `AZURE_WEBAPP_PUBLISH_PROFILE` y la variable
   `AZURE_WEBAPP_NAME`.
