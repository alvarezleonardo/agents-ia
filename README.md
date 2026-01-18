# Claude Agents Collection

Colección de agentes personalizados para Claude Code CLI / Copilot.

## ¿Qué son los Agentes?

Los agentes son asistentes especializados con conocimientos profundos en áreas específicas. A diferencia de las skills (que son herramientas/workflows), los agentes tienen expertise en dominios técnicos.

## Instalación

Para usar estos agentes, copia los archivos `.md` a tu directorio local de agentes:

```bash
# Copiar todos los agentes
cp agents/*.md ~/.claude/agents/

# O copiar un agente específico
cp agents/devops-deployment-expert.md ~/.claude/agents/
```

## Agentes Disponibles

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

## Uso

Los agentes se invocan automáticamente cuando Claude / Copilot detecta que tu consulta requiere expertise especializado, o puedes mencionarlos explícitamente en tu prompt.

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

## Contribuir

Para agregar o modificar agentes:

1. **Crear rama**: `git checkout -b feature/agent-nombre`
2. **Completar contenido**: Seguir estructuras existentes
3. **Actualizar este README**: Añadir a la sección correspondiente
4. **Pull Request**: Incluir descripción y ejemplos de uso

Para contribuir a la documentación de recursos:

1. **Design Patterns**: Seguir estructura de 10 secciones
2. **Good Practices**: Incluir ejemplos antes/después
3. **Actualizar READMEs**: Mantener índices actualizados
