# Decisiones técnicas

Registro de decisiones importantes sobre la estructura y el uso de este repositorio, con su justificación.

---

## 1. Estructura de 3 capas para reglas de Cursor

**Fecha:** 2025-03  
**Estado:** Aceptada

### Contexto

Cursor permite definir reglas para el asistente de IA en dos sitios nativos:

- **Reglas globales** (Cursor Settings → General → Rules for AI): aplican a todos los proyectos del usuario.
- **Reglas de proyecto**: archivos en `.cursor/rules/` dentro de cada repositorio, bajo control de versiones.

Queríamos un esquema que diera consistencia entre proyectos, evitara duplicar reglas en cada repo y permitiera tanto directrices universales como específicas por dominio o por proyecto.

### Decisión

Adoptar un **sistema de 3 capas**:

1. **Capa 1 – Global (Cursor Settings)**  
   Todo lo que debe aplicarse en *todos* los proyectos: rol, flujo de trabajo, automejora, estilo de respuesta, seguridad base, convenciones de commits. Se edita en un solo lugar y se propaga automáticamente.

2. **Capa 2 – Repo de reglas compartidas (este repo)**  
   Un repositorio dedicado solo a reglas, por dominio o tecnología (Ansible, Terraform, Kubernetes, CI/CD, etc.). Cada proyecto puede referenciar este repo o copiar solo las reglas que necesite. Las actualizaciones se hacen aquí y se propagan según el mecanismo elegido (copia manual, script, extensión).

3. **Capa 3 – `.cursor/rules/` por proyecto**  
   Reglas que solo aplican a *ese* proyecto: stack concreto, módulos, cuentas, convenciones locales. Viven en el propio repo y se versionan con el código del proyecto.

### Alternativas consideradas

- **Todo en global:** Contexto único y enorme; mezcla lo universal con lo específico de cada proyecto; difícil de mantener y de auditar por repo.
- **Solo reglas por proyecto:** Máxima flexibilidad pero mucha duplicación; cambios de criterio (ej. estándar Terraform) habría que repetirlos en N repos.
- **Un solo repo de reglas sin capa global:** Posible, pero las preferencias de workflow y estilo son transversales; tenerlas en global evita repetirlas en cada conjunto de reglas compartidas.

### Consecuencias

- **Positivas:** Base coherente en todos los proyectos, menos duplicación, cambios de criterio centralizados en el repo de reglas, separación clara entre “cómo trabajo siempre” (global), “cómo trabajo con X” (compartido) y “cómo es este proyecto” (local).
- **A asumir:** Hace falta definir *cómo* cada proyecto consume la capa 2 (copia, script, extensión); Cursor no referencia otro repo por sí solo. Conviene documentar el mecanismo elegido por proyecto o por equipo.
- **Riesgo:** Cambios en el repo compartido pueden afectar a muchos proyectos; por eso se trata las reglas como código (revisión, versionado, opcionalmente tags).

### Referencias

- Reglas globales actuales: Cursor Settings → Rules (y en este repo, `rule-general.mdc` como referencia/plantilla).
- Listado de reglas compartidas y uso: [README.md](../README.md).
