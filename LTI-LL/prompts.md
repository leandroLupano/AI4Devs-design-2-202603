
## Prompt para generar el template a usar para las User Stories.

```markdown
# Rol:
Sos un experto Product Manager y Business Analyst con amplio conocimiento y trabajo con metodología Agile.

# Contexto:
En este respositorio se tienen que crear User Stories.
Deben estar basadas en el archivo: [LTI-LL.md](LTI-LL/LTI-LL.md)
Las instrucciones de la tarea a realizar están en: [ReadMe.md](ReadMe.md)
La información teoríca para realizar esta tarea se encuentra en: [supporting-theoretical-information.md](supporting-theoretical-information.md)

# Tarea:
Quiero que generes un fichero: "user-story-template.md" para usar como template para generar las User Stories.
Tiene que estar basado en la información teórica de [supporting-theoretical-information.md](supporting-theoretical-information.md)  y en las buenas prácticas recomendas para Agile.
El template debe estar en inglés.
```

---
## Meta prompt: Generar prompt para generar User Stories.

```markdown
# Rol:
Sos un experto Prompt engineer con amplio conocimiento en sistema Agile y con conocimiento en producto y negocio de software. 


# Contexto:
En este respositorio se tienen que crear User Stories.
Deben estar basadas en el archivo: [LTI-LL.md](LTI-LL/LTI-LL.md)
Las instrucciones de la tarea a realizar están en: [ReadMe.md](ReadMe.md)
La información teoríca para realizar esta tarea se encuentra en: [supporting-theoretical-information.md](supporting-theoretical-information.md)
El template para crear las User Stories es: [user-story-template.md](user-story-template.md) 


# Tarea:
Quiero que generes un prompt para ejecutar en un agente y cumplir la tarea descripta en [ReadMe.md](ReadMe.md) , usando el template [user-story-template.md](user-story-template.md) , según la info teórica de [supporting-theoretical-information.md](supporting-theoretical-information.md) , y debe estar basado en el proyecto [LTI-LL.md](LTI-LL/LTI-LL.md) .
```

---

## Prompt final recomendado para generar User Stories, Backlog y Tickets

```markdown
# Rol
Actua como un Senior Product Manager y Business Analyst especializado en Agile/Scrum, discovery de producto B2B SaaS y escritura de User Stories listas para refinamiento con equipos de ingenieria.

# Contexto
Debes preparar la documentacion necesaria para iniciar la implementacion de **LTI - Modern ATS for Collaborative Hiring**, un ATS B2B SaaS para empresas medianas.

Lee y usa obligatoriamente estos archivos:
- `ReadMe.md`: instrucciones del ejercicio y entregables esperados.
- `LTI-LL/LTI-LL.md`: PRD base del producto LTI.
- `supporting-theoretical-information.md`: teoria sobre PRD, Agile, User Stories, backlog, tickets, priorizacion y estimacion.
- `user-story-template.md`: plantilla obligatoria para redactar cada User Story.

# Objetivo
Crea o actualiza `LTI-LL/UserStories-LL.md` con un unico documento Markdown que cumpla la tarea de `ReadMe.md`.

El documento debe incluir:
1. User Stories basadas en el PRD.
2. Product Backlog priorizado con una metodologia explicita.
3. Diferentes prompts usados o propuestos para generar el backlog, indicando cual dio mejores resultados y por que.
4. Una User Story elegida para planificacion tecnica.
5. Tickets de trabajo tecnicos para esa User Story.
6. Estimacion de esfuerzo de los tickets usando Fibonacci story points.

# Reglas
- Escribe el entregable en ingles.
- Usa exactamente la estructura de `user-story-template.md` para cada User Story.
- Genera entre 6 y 10 User Stories.
- Usa IDs consecutivos: `US-001`, `US-002`, etc.
- Mantén trazabilidad hacia secciones, features o use cases de `LTI-LL/LTI-LL.md`.
- Aplica INVEST, Definition of Ready, Definition of Done y criterios de aceptacion BDD `Given / When / Then`.
- No inventes alcance fuera del MVP. Si mencionas algo futuro, marcalo como out of scope.
- Respeta la regla de producto: la IA es asistiva y nunca toma decisiones finales de contratacion, rechazo, oferta, ranking automatico ni cambio de etapa sin accion humana autorizada.
- Incluye requisitos no funcionales de seguridad, permisos, privacidad, auditoria, performance y UX/accesibilidad.

# Areas MVP que deben quedar cubiertas
- Job vacancy management.
- Application intake.
- Candidate profile.
- Recruitment pipeline.
- Collaboration tools.
- Interview coordination.
- Candidate communication.
- Assistive AI.
- Human decision controls.
- Role-based access control.
- Basic reporting.

# Roles y entidades relevantes
Personas: Admin, Recruiter, Hiring Manager, Interviewer, Candidate.

Entidades: Organization, User, Role, JobOpening, JobTeamMember, Candidate, Application, PipelineStage, Interview, Evaluation, Comment, Document, AutomationRule, Notification, AIInsight.

# Estructura obligatoria del documento

## 1. Executive Summary
Resume en 5-8 bullets que contiene el documento, el enfoque de priorizacion y la cobertura del MVP.

## 2. Source Interpretation
Explica brevemente los objetivos del PRD, restricciones relevantes y principios Agile aplicados.

## 3. User Stories
Genera entre 6 y 10 User Stories usando el template completo. Deben cubrir como minimo:
- Crear y publicar una vacante.
- Recibir postulaciones con CV y consentimiento.
- Gestionar aplicaciones en pipeline.
- Revisar perfil de candidato con resumen IA asistivo.
- Coordinar entrevistas.
- Registrar feedback estructurado.
- Colaborar con comentarios o tareas.
- Comunicar estados al candidato.
- Mantener control humano sobre decisiones.
- Ver metricas basicas del pipeline.

## 4. Product Backlog
Crea una tabla con:
- Rank.
- User Story ID.
- Title.
- Epic / Feature.
- Persona.
- Priority.
- Estimated Story Points.
- Business Value.
- Urgency.
- Risk Reduction / Learning.
- Dependencies.
- Rationale.

Usa **WSJF simplificado** o **MoSCoW + valor/esfuerzo** y explica por que esa metodologia encaja con el MVP.

## 5. Prompt Experiments
Incluye al menos 3 prompts:
- Prompt 1: generacion directa desde PRD.
- Prompt 2: generacion usando template + INVEST + BDD.
- Prompt 3: generacion con priorizacion explicita y enfoque MVP.

Para cada prompt, incluye texto completo, resultado esperado, fortalezas y limitaciones. Luego selecciona el mejor y explica por que.

## 6. Selected User Story for Technical Planning
Elige una User Story prioritaria y justifica la eleccion. Prioriza una historia fundacional como vacancy creation, application intake o pipeline management.

## 7. Work Tickets
Descompone la historia elegida en 5-10 tickets tecnicos. Cada ticket debe incluir:
- Ticket ID.
- Usa IDs consecutivos: `TASK-001`, `TASK-002`, etc.
- Title.
- Description.
- Area: Frontend, Backend, Data, QA, UX, Infra, Security, AI, Notifications.
- Story Points.
- Priority.
- Assigned person.
- Tags: Security, Backend, Sprint 10.
- Technical notes.
- Acceptance criteria.
- Dependencies.
- Comments.

## 8. Effort Estimation
Incluye tabla de estimacion con Fibonacci story points, criterio usado, riesgos/incertidumbres y total aproximado.

## 9. Conclusions
Resume por que el backlog es adecuado para empezar el MVP, que decisiones validar con stakeholders y que riesgos quedan abiertos.

# Checklist final
Antes de finalizar, verifica:
- Todas las User Stories aportan valor claro.
- Todas siguen el template.
- Todas tienen criterios verificables.
- El backlog esta priorizado con razonamiento.
- Los tickets son tecnicamente accionables.
- La estimacion es consistente.
- La IA queda como asistiva, no decisoria.
- El documento no contradice el PRD.
```
