# cursor-rules

Repositorio de reglas compartidas para el asistente de IA de Cursor (capa 2 del sistema de reglas). Contiene directrices por dominio o tecnología que podés reutilizar en varios proyectos sin duplicar contenido.

## Sistema de 3 capas

| Capa | Dónde | Uso |
|------|--------|-----|
| **1 – Global** | Cursor Settings → Rules | Workflow, automejora, estilo de respuesta, seguridad base. Aplica a todos los proyectos. |
| **2 – Compartidas** | Este repo (`cursor-rules`) | Reglas por stack/dominio. Cada proyecto referencia o copia solo las que necesita. |
| **3 – Por proyecto** | `.cursor/rules/` en cada repo | Reglas específicas de ese proyecto (ej. "Terraform AWS us-east-1 con estos módulos"). |

Detalle y justificación de esta estructura: [docs/decisions.md](docs/decisions.md).

## Reglas disponibles

| Archivo | Contenido |
|---------|-----------|
| `rule-general.mdc` | Resumen/plantilla de reglas globales (referencia; lo que aplica a todos va en Cursor Settings). |
| `ansible.mdc` | Reglas para proyectos con Ansible. |
| `bash.mdc` | Reglas para scripts y entornos Bash. |
| `cicd.mdc` | Reglas para pipelines y CI/CD. |
| `containers.mdc` | Reglas para Docker/containers. |
| `kubernetes.mdc` | Reglas para proyectos con Kubernetes. |
| `observability.mdc` | Reglas para monitoreo, logs, métricas. |
| `proxmox-netbox.mdc` | Reglas para entornos Proxmox + NetBox. |
| `terraform.mdc` | Reglas para proyectos con Terraform. |

## Cómo usar en un proyecto

Cursor no incluye reglas de otro repo de forma nativa. Opciones:

1. **Copiar a `.cursor/rules/`**  
   Copiá solo los `.mdc` que apliquen al proyecto (ej. `terraform.mdc`, `kubernetes.mdc`) dentro de `.cursor/rules/` del repo. Podés versionarlos ahí y actualizarlos cuando quieras.

2. **Sincronizar con script o CI**  
   Un script o job que clone este repo (o un subconjunto) y vuelque los archivos en `.cursor/rules/`. Así centralizás cambios en este repo y los proyectos reciben actualizaciones al correr el script.

3. **Herramientas tipo Cursor Workbench**  
   Si usás una extensión/registry que importe reglas desde un repo remoto, podés apuntar a este repositorio (público o privado) y elegir qué reglas aplicar por proyecto.

Recomendación: definir **un** mecanismo por proyecto o por equipo y documentarlo (ej. en el README del proyecto o en su `docs/`).

## Mantenimiento

- Cambios en este repo afectan a todos los proyectos que consuman estas reglas (por copia o por sincronización). Pensar en el impacto antes de modificar.
- Las reglas se versionan como código: commits claros, PRs si trabajás en equipo, y opcionalmente tags para "versiones estables" si necesitás inmutabilidad.
