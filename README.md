<div align="center">

# 🤖 Claude Agents Collection

### Colección de Agentes Especializados para Claude Code CLI / Copilot
 
[![Agents](https://img.shields.io/badge/agents-10-blue?style=flat-square)]()
[![Design Patterns](https://img.shields.io/badge/patterns-10-green?style=flat-square)]()
[![Documentation](https://img.shields.io/badge/docs-complete-orange?style=flat-square)]()
[![Language](https://img.shields.io/badge/lang-ES-red?style=flat-square)]()

[Instalación](#-instalación) • [Agentes](#-agentes-disponibles) • [Uso](#-uso) • [Recursos](#-recursos-adicionales) • [Contribuir](#-contribuir)

</div>

---

## 📖 ¿Qué son los Agentes?

Los agentes son asistentes especializados con conocimientos profundos en áreas específicas del desarrollo de software. A diferencia de las skills (herramientas/workflows), los agentes aportan **expertise técnico especializado** en dominios como arquitectura, testing, DevOps, calidad de código y más.

### ✨ Características

- 🎯 **Especialización**: Cada agente es experto en su dominio
- 📚 **Conocimiento Base**: Incluye documentación de referencia (patrones, principios SOLID)
- 🔄 **Integrados**: Los agentes trabajan juntos compartiendo conocimiento
- 🚀 **Listos para usar**: Solo copia y comienza a trabajar

## 📦 Instalación

### Instalación Completa

```bash
# Copiar todos los agentes
cp *.md ~/.claude/agents/

# Copiar recursos de referencia
cp -r design_patterns ~/.claude/agents/
cp -r good_practices ~/.claude/agents/
```

### Instalación Individual

```bash
# Solo un agente específico
cp devops-deployment-expert.md ~/.claude/agents/
```

### Verificación

```bash
# Listar agentes instalados
ls ~/.claude/agents/
```

---

## 🤖 Agentes Disponibles

<div align="center">

| Categoría | Agentes | Descripción |
|-----------|---------|-------------|
| 🚀 **DevOps** | 1 agente | Deployment, K8s, CI/CD, Cloud |
| 💻 **Backend** | 1 agente | Spring Boot, Java, Microservicios |
| 🎨 **Frontend** | 1 agente | Angular, TypeScript, SPA |
| 🏗️ **Arquitectura** | 3 agentes | Diseño, Patrones, Calidad de Código |
| 📋 **Product** | 2 agentes | Casos de Uso, User Stories |
| 🧪 **QA** | 1 agente | API Testing, Validación Automatizada |

</div>

### 🚀 DevOps & Infrastructure

#### devops-deployment-expert
Experto en DevOps, containerización, Kubernetes, Terraform, GCP, CI/CD y deployment strategies.

**Especialidades:**
- Docker y containerización
- Kubernetes (GKE, Helm)
- Infrastructure as Code (Terraform)
- Google Cloud Platform (GCP)
- CI/CD pipelines
- Deployment strategies (Blue/Green, Canary)
- Networking y seguridad

**Cuándo usar:**
- Diseño de infraestructura cloud
- Configuración de pipelines CI/CD
- Troubleshooting de deployments
- Optimización de contenedores
- Estrategias de deployment

### 💻 Desarrollo Backend

#### backend-developer
Desarrollador backend senior experto en Spring Boot, Java y arquitectura de microservicios.

**Especialidades:**
- Spring Boot, Spring Data, Spring Security
- Java 17+ y JVM
- Arquitectura de microservicios
- APIs RESTful con OpenAPI/Swagger
- Multi-tenancy y bases de datos
- Principios SOLID y patrones de diseño
- Testing (JUnit 5, Mockito)

**Cuándo usar:**
- Implementación de backend con Spring Boot
- Diseño de APIs REST
- Configuración de microservicios
- Arquitectura multi-tenant
- Integración de servicios Java
- Refactoring aplicando SOLID

**Conocimiento base:**
- Patrones de diseño en `design_patterns/`
- Principios SOLID en `good_practices/`

### 🎨 Desarrollo Frontend

#### angular-frontend-expert
Desarrollador frontend senior experto en Angular, TypeScript y arquitectura de aplicaciones SPA.

**Especialidades:**
- Angular 17+ (Standalone Components)
- TypeScript
- RxJS y programación reactiva
- Estado y arquitectura de SPAs
- Material Design / UI frameworks

**Cuándo usar:**
- Implementación de aplicaciones Angular
- Diseño de componentes y módulos
- Gestión de estado en frontend
- Integración con APIs backend

### 🏗️ Arquitectura & Diseño

#### software-architect-expert
Arquitecto de Software senior experto en diseño de sistemas distribuidos y arquitecturas cloud-native.

**Especialidades:**
- Diseño de arquitectura de sistemas
- Patrones arquitectónicos (MVC, Microservicios, etc.)
- ADRs (Architecture Decision Records)
- Diagramas de componentes
- Decisiones técnicas estratégicas

**Cuándo usar:**
- Diseño de arquitectura de proyectos
- Toma de decisiones técnicas importantes
- Documentación de arquitectura
- Evaluación de trade-offs tecnológicos

#### design-patterns-expert
Arquitecto de software experto en patrones de diseño GoF (Gang of Four) con especialización en Java/Spring Boot.

**Especialidades:**
- Patrones Creacionales (Factory Method, Singleton)
- Patrones Estructurales (Adapter, Composite, Decorator, Proxy)
- Patrones Comportamentales (Memento, Observer, State, Strategy)
- Implementación de patrones en Spring Boot
- Refactorización de código hacia patrones
- Testing de patrones

**Cuándo usar:**
- Identificar qué patrón usar para un problema específico
- Implementar patrones de diseño correctamente
- Refactorizar código existente aplicando patrones
- Code review enfocado en patrones
- Resolver problemas de diseño (muchos if/else, código duplicado, acoplamiento)
- Integrar patrones con Spring Boot (Events, AOP, Beans)

**Conocimiento base:**
- Documentación completa en `design_patterns/`
- 10 patrones documentados con ejemplos prácticos
- Diagramas Mermaid de estructura
- Casos de uso reales de proyectos

#### code-quality-analyzer
Arquitecto de software senior experto en análisis de calidad de código, principios SOLID, cohesión, acoplamiento y detección de code smells.

**Especialidades:**
- Evaluación de principios SOLID (SRP, OCP, LSP, ISP, DIP)
- Análisis de cohesión (7 niveles) y acoplamiento (6 tipos)
- Detección de code smells (God Class, Long Method, Feature Envy, etc.)
- Métricas de calidad (LCOM, Ca, Ce, Instability, Complejidad Ciclomática)
- Recomendaciones de refactoring con código antes/después
- Priorización de problemas por severidad
- Mapeo de violaciones → patrones de diseño

**Cuándo usar:**
- Code reviews enfocados en calidad
- Análisis de código legacy para mejoras
- Identificar violaciones de principios SOLID
- Calcular métricas de cohesión/acoplamiento
- Detectar code smells y anti-patrones
- Obtener recomendaciones de refactoring concretas
- Priorizar deuda técnica por impacto
- Evaluar testabilidad del código

**Conocimiento base:**
- Guías completas en `good_practices/solid-principles.md` (1,600 líneas)
- Métricas en `good_practices/cohesion-coupling.md` (1,400 líneas)
- 10 patrones de diseño en `design_patterns/`
- Integración con design-patterns-expert para recomendaciones

### 📋 Product Management & Documentation

#### use-case-expert
Analista de sistemas senior experto en documentación de casos de uso con metodología estándar y experiencia en sistemas empresariales.

**Especialidades:**
- Metodología de Casos de Uso (UML, análisis funcional)
- Estructura estándar con 11 secciones obligatorias
- Dominio de negocio (ERP, BackOffice, POS, sistemas empresariales)
- Flujos principal, alternativos y de excepción
- Identificación de actores y precondiciones
- Validaciones y reglas de negocio
- Queries SQL y modelo de datos
- Convenciones de documentación estándar

**Cuándo usar:**
- Crear casos de uso desde requerimientos
- Mejorar casos de uso existentes (agregar flujos de excepción)
- Validar completitud y calidad de casos de uso
- Generar múltiples casos de uso relacionados (CRUD)
- Analizar requerimientos ambiguos y clarificar
- Documentar flujos complejos (multi-actor, multi-flujo)
- Aplicar convenciones de proyecto (dominios, naming, YAML)

**Conocimiento base:**
- Template estandarizado en `templates/caso-uso.md`
- Casos de uso de ejemplo en `docs/casos-uso/`
- Bases de datos de sistemas empresariales
- Dominios: seguridad, ventas, inventario, rrhh, gestión empresarial
- Actores comunes: administrador, gerente, operador, sistema

#### product-owner-expert
Product Owner experto con experiencia en metodologías Lean Startup y Agile.

**Especialidades:**
- User stories y criterios de aceptación
- Priorización de funcionalidades
- Metodologías Agile
- Lean Startup
- Definición de MVPs

**Cuándo usar:**
- Definición de requisitos de proyecto
- Creación de user stories
- Priorización de features
- Análisis de funcionalidades

### 🧪 Quality Assurance

#### qa-api-testing-expert
Experto en QA de APIs, testing automatizado y validación contra casos de uso.

**Especialidades:**
- Testing de APIs REST (localhost, dev, qa)
- **Gestión automática del ciclo de vida de la API** - Detener, compilar, levantar antes de tests
- **Autenticación automática** - Flujos de autenticación con JWT
- Validación en bases de datos MySQL
- **Análisis de Swagger/OpenAPI** - Schema validation y contract testing
- Generación de test cases desde CU + Swagger
- Postman/Newman, RestAssured, JUnit 5
- Reportes para humanos y agentes
- Detección inteligente de tablas desde contexto
- **Análisis de consistencia** CU ↔ Swagger ↔ DB
- **Gestión de tokens JWT** - Caching y renovación automática
- **Build automation** - Maven/Gradle integration

**Cuándo usar:**
- Validar funcionamiento de APIs
- Verificar persistencia en MySQL
- Generar test suites automatizados
- Validar cumplimiento de casos de uso
- Testing de integración API + DB
- Validar conformidad con Swagger schemas
- Detectar discrepancias entre CU, Swagger y DB
- Testing de endpoints protegidos con autenticación
- Ejecutar tests contra versión más reciente del código (auto-recompila)

## 🚀 Quick Start

### 1. Instala los agentes
```bash
cp *.md ~/.claude/agents/
cp -r design_patterns good_practices ~/.claude/agents/
```

### 2. Usa los agentes en tu conversación

Los agentes se activan automáticamente cuando Claude detecta que necesitas expertise especializado:

**Ejemplo - DevOps:**
```
Usuario: "Necesito configurar un pipeline CI/CD para mi app Spring Boot en GCP"
→ Claude activará automáticamente devops-deployment-expert
```

**Ejemplo - Backend:**
```
Usuario: "Implementa un endpoint REST con Spring Boot para gestionar usuarios"
→ Claude activará backend-developer + design-patterns-expert
```

**Ejemplo - Testing:**
```
Usuario: "Valida que la API cumple con el caso de uso CU-001"
→ Claude activará qa-api-testing-expert
```

### 3. O invócalos explícitamente

```
"Usando design-patterns-expert, refactoriza esta clase aplicando el patrón Strategy"
```

---

## 💡 Uso

## Crear Nuevos Agentes

Para crear un agente personalizado, usa el template estandarizado:

### Opción 1: Usar Template (Recomendado)

```bash
# Copiar template base
cp templates/agent.md agents/mi-nuevo-agente.md

# Editar y completar las secciones
code agents/mi-nuevo-agente.md
```

El template incluye:
- ✅ Estructura completa con todas las secciones necesarias
- ✅ Placeholders `[nombre]` fáciles de identificar
- ✅ Ejemplos de código y tablas
- ✅ Secciones opcionales marcadas
- ✅ Checklist de calidad

Consulta `templates/README.md` para guía detallada de llenado.

### Opción 2: Formato Básico

```markdown
# [Nombre del Agente] Agent

Eres un [rol] experto en [área de especialización].

## Tu Rol
[Descripción del rol y responsabilidad]

## Conocimiento Base
[Lista de tecnologías y áreas de expertise]

## Instrucciones
[Guía paso a paso de cómo abordar las tareas]

## Mejores Prácticas
[Best practices y anti-patrones]

## Ejemplos de Uso
[Ejemplos de interacciones Usuario-Agente]

## Referencias
[Links a documentación relevante]

## Contexto del Proyecto
$ARGUMENTS
```

### Checklist para Nuevos Agentes

Antes de publicar un agente, verifica:

- [ ] Nombre descriptivo en kebab-case (`nombre-agente.md`)
- [ ] Rol y responsabilidades claramente definidos
- [ ] Conocimiento base específico y actualizado
- [ ] Instrucciones paso a paso numeradas
- [ ] Al menos 3 ejemplos de uso
- [ ] Mejores prácticas con ejemplos ✅/❌
- [ ] Referencias a documentación interna/externa
- [ ] Añadido al índice de este README
- [ ] Revisado por al menos un miembro del equipo

## 📚 Recursos Adicionales

Además de los agentes, esta colección incluye documentación completa de referencia:

### 🎨 Design Patterns (`design_patterns/`)

Documentación completa de 10 patrones de diseño GoF (Gang of Four):

- **Creacionales**: Factory Method, Singleton
- **Estructurales**: Adapter, Composite, Decorator, Proxy
- **Comportamentales**: Memento, Observer, State, Strategy

Cada patrón incluye:
- ✅ Explicación detallada del problema y solución
- ✅ Diagramas UML (Mermaid)
- ✅ 3+ ejemplos completos con código Java
- ✅ Casos de uso empresariales
- ✅ Ventajas, desventajas y aplicabilidad

**Consultar**: [`design_patterns/README.md`](design_patterns/README.md)

### ✨ Good Practices (`good_practices/`)

Guías completas sobre principios fundamentales de diseño:

**1. Principios SOLID** (`solid-principles.md` - 1,600+ líneas)
- Single Responsibility Principle
- Open/Closed Principle
- Liskov Substitution Principle
- Interface Segregation Principle
- Dependency Inversion Principle

**2. Cohesión y Acoplamiento** (`cohesion-coupling.md` - 1,400+ líneas)
- 7 niveles de cohesión
- 6 tipos de acoplamiento
- Métricas: LCOM, Ca, Ce, Instability
- Análisis a nivel de clase y paquete

**Consultar**: [`good_practices/README.md`](good_practices/README.md)

### 🔗 Integración

Los recursos están integrados con los agentes:

| Agente | Usa Recursos |
|--------|--------------|
| **design-patterns-expert** | Acceso completo a `design_patterns/` |
| **code-quality-analyzer** | Referencias a `good_practices/` |
| **backend-developer** | Aplica patrones y principios SOLID |

---

## 📊 Estadísticas del Proyecto

<div align="center">

| Métrica | Valor |
|---------|-------|
| **Total de Agentes** | 10 |
| **Design Patterns Documentados** | 10 |
| **Líneas de Documentación** | 9,000+ |
| **Ejemplos de Código** | 50+ |
| **Principios SOLID** | 5 |
| **Niveles de Cohesión** | 7 |
| **Tipos de Acoplamiento** | 6 |

</div>

---

## 🤝 Contribuir

¡Las contribuciones son bienvenidas! Este proyecto crece con la comunidad.

### 📝 Agregar un Nuevo Agente

1. **Fork el repositorio**
2. **Crea una rama**: `git checkout -b feature/agent-nombre`
3. **Usa el template**: Copia `templates/agent.md` como base
4. **Completa todas las secciones**:
   - Rol y responsabilidades
   - Conocimiento base específico
   - Instrucciones paso a paso
   - Ejemplos de uso (mínimo 3)
   - Mejores prácticas
   - Referencias
5. **Actualiza este README**: Añade tu agente a la sección correspondiente
6. **Commit**: `git commit -m "feat: add [nombre-agente] expert"`
7. **Push**: `git push origin feature/agent-nombre`
8. **Pull Request**: Incluye descripción completa y ejemplos

### 📚 Mejorar Documentación

**Design Patterns:**
- Agregar nuevos patrones GoF
- Mejorar ejemplos existentes
- Añadir diagramas UML
- Incluir casos de uso reales

**Good Practices:**
- Expandir guías SOLID
- Agregar métricas de calidad
- Documentar code smells
- Incluir ejemplos antes/después

### ✅ Checklist de Calidad

Antes de enviar tu PR, verifica:

- [ ] Nombre en kebab-case (`nombre-agente.md`)
- [ ] Todas las secciones completas
- [ ] Mínimo 3 ejemplos de uso
- [ ] Código de ejemplo funcional
- [ ] Mejores prácticas con ✅/❌
- [ ] Referencias actualizadas
- [ ] README actualizado
- [ ] Sin errores de ortografía
- [ ] Formato Markdown correcto

---

## 📄 Licencia

Este proyecto está bajo la Licencia MIT. Ver el archivo `LICENSE` para más detalles.

```
MIT License - Libre para uso comercial y personal
```

---

## 🗺️ Roadmap

### En Desarrollo
- [ ] Angular Microfrontend Expert
- [ ] Python Backend Expert
- [ ] Database Expert (PostgreSQL, MongoDB)
- [ ] Security Expert (OWASP, penetration testing)

### Planeado
- [ ] React Frontend Expert
- [ ] Cloud Expert (AWS, Azure, GCP)
- [ ] Machine Learning Expert
- [ ] GraphQL Expert
- [ ] Documentación en inglés

---

## 🔗 Enlaces Útiles

- 📖 [Documentación de Claude Code CLI](https://docs.anthropic.com/claude/docs)
- 🎯 [Guía de Design Patterns](design_patterns/README.md)
- ✨ [Principios SOLID](good_practices/README.md)
- 💬 [Discusiones y Preguntas](../../discussions)
- 🐛 [Reportar Issues](../../issues)

---

<div align="center">

**Hecho con ❤️ para la comunidad de desarrolladores**

⭐ **Si te resulta útil, considera darle una estrella al proyecto**

[⬆ Volver arriba](#-claude-agents-collection)

</div>
