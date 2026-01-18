# Good Practices - Principios de Diseño de Software

Guías completas sobre principios fundamentales de diseño de software orientado a objetos, con ejemplos prácticos para proyectos empresariales.

## 📚 Contenido

Este directorio contiene documentación detallada sobre los principios y métricas que definen código de calidad:

### 1. [Principios SOLID](solid-principles.md) ⭐

Guía completa de los 5 principios fundamentales de diseño orientado a objetos.

**Contenido (1,600+ líneas)**:
- **S** - Single Responsibility Principle
- **O** - Open/Closed Principle
- **L** - Liskov Substitution Principle
- **I** - Interface Segregation Principle
- **D** - Dependency Inversion Principle

**Incluye**:
- ✅ Explicación detallada de cada principio
- ✅ Ejemplos ❌ violando vs ✅ cumpliendo
- ✅ Casos reales de proyectos empresariales
- ✅ Checklist de verificación por principio
- ✅ Relación con patrones de diseño

**Cuándo consultar**:
- Diseñando nuevas clases o módulos
- Refactorizando código existente
- Code reviews enfocados en calidad
- Entrenamiento de equipo en principios OOP

### 2. [Cohesión y Acoplamiento](cohesion-coupling.md) ⭐

Guía exhaustiva sobre las dos métricas fundamentales de calidad de diseño.

**Contenido (1,400+ líneas)**:
- **Cohesión**: 7 niveles (Coincidental → Functional)
- **Acoplamiento**: 6 tipos (Content → Message)
- Métricas LCOM, Ca, Ce, Instability
- Análisis a nivel de clase y paquete

**Incluye**:
- ✅ Definición clara de conceptos con diagramas
- ✅ Niveles de cohesión con ejemplos de cada uno
- ✅ Tipos de acoplamiento de peor a mejor
- ✅ Métricas cuantitativas y cómo calcularlas
- ✅ Ejemplos completos de refactoring
- ✅ Organización de paquetes por dominio vs técnica
- ✅ Casos de sistemas empresariales (Ventas, Reportes, Precios)

**Cuándo consultar**:
- Diseñando arquitectura de módulos/paquetes
- Evaluando si separar o unir clases
- Midiendo calidad de diseño de código
- Decidiendo estructura de microservicios

## 🎯 Objetivo: Alta Cohesión + Bajo Acoplamiento

```
┌─────────────────────────────────────┐
│   Módulo con DISEÑO IDEAL          │
├─────────────────────────────────────┤
│                                     │
│  ✅ ALTA COHESIÓN                  │
│  - Responsabilidad única y clara   │
│  - Métodos trabajan juntos         │
│  - Fácil de entender               │
│                                     │
│  ✅ BAJO ACOPLAMIENTO              │
│  - Pocas dependencias externas     │
│  - Depende de abstracciones        │
│  - Fácil de testear y reutilizar  │
│                                     │
└─────────────────────────────────────┘
```

## 📊 Comparación Rápida

| Aspecto | Alta Cohesión | Bajo Acoplamiento |
|---------|---------------|-------------------|
| **Qué mide** | Relación entre elementos internos | Dependencia entre módulos |
| **Objetivo** | Métodos/campos relacionados | Pocas dependencias externas |
| **Indicador** | Todos trabajan hacia un objetivo | Cambios no se propagan |
| **Ejemplo Bueno** | `EmailService` solo envía emails | Depende de `interface EmailSender` |
| **Ejemplo Malo** | `UtilService` hace 10 cosas distintas | Depende de 15 clases concretas |

## 🔍 Cómo Usar Esta Documentación

### Para Desarrolladores

```bash
# 1. Estás diseñando una clase nueva
→ Lee solid-principles.md sección "Single Responsibility"
→ Pregúntate: ¿Esta clase tiene una única razón para cambiar?

# 2. Tu clase tiene 500 líneas
→ Lee cohesion-coupling.md sección "Cohesión a Nivel de Clase"
→ Identifica si tiene múltiples responsabilidades (cohesión baja)
→ Aplica Extract Class

# 3. Tienes muchos if/else por tipo
→ Lee solid-principles.md sección "Open/Closed Principle"
→ Aplica Strategy Pattern o Factory Method

# 4. Tu clase depende de 10 clases concretas
→ Lee solid-principles.md sección "Dependency Inversion"
→ Introduce interfaces y usa Dependency Injection
```

### Para Arquitectos

```bash
# 1. Diseñando estructura de paquetes
→ Lee cohesion-coupling.md sección "Cohesión a Nivel de Paquete"
→ Organiza por dominio (ventas/, clientes/) no por técnica (controllers/, services/)

# 2. Hay dependencias circulares entre módulos
→ Lee cohesion-coupling.md sección "Acoplamiento a Nivel de Paquete"
→ Aplica inversión de dependencias o extrae interfaz compartida

# 3. Evaluando calidad de un módulo
→ Calcula métricas: LCOM, Ca, Ce, Instability
→ Compara con valores ideales documentados
```

### Para Code Reviewers

```bash
# Durante code review, verifica:
1. ✅ Cada clase cumple SRP (1 responsabilidad)
2. ✅ No hay código duplicado (DRY)
3. ✅ Dependencias son de abstracciones (DIP)
4. ✅ Métodos < 30 líneas (cohesión funcional)
5. ✅ Clase depende de < 5 clases externas (bajo acoplamiento)
6. ✅ No usa variables estáticas compartidas (acoplamiento common)

# Si encuentras violaciones:
→ Referencia sección específica del documento
→ Sugiere refactoring concreto con ejemplo
```

## 🛠️ Herramientas de Análisis

### Análisis Estático

```bash
# SonarQube - Análisis completo de calidad
docker run -d --name sonarqube -p 9000:9000 sonarqube

# JDepend - Métricas de acoplamiento/cohesión
mvn jdepend:generate

# Checkstyle - Validación de convenciones
mvn checkstyle:check

# PMD - Detección de code smells
mvn pmd:pmd
```

### IntelliJ IDEA Metrics

```
1. Analyze → Calculate Metrics
2. Seleccionar módulo/paquete
3. Ver:
   - Lines of Code
   - Number of Methods
   - Cyclomatic Complexity
   - Afferent/Efferent Coupling
```

### Métricas a Vigilar

| Métrica | Valor Ideal | Valor Alerta | Acción |
|---------|-------------|--------------|--------|
| **Líneas por clase** | < 150 | > 300 | Extract Class |
| **Métodos por clase** | < 10 | > 15 | Revisar cohesión |
| **Parámetros por método** | < 4 | > 6 | Introducir DTO |
| **Complejidad ciclomática** | < 10 | > 15 | Extract Method, Strategy |
| **Efferent Coupling (Ce)** | < 5 | > 10 | Dependency Inversion |
| **LCOM** | < 0.3 | > 0.7 | Extract Class |

## 📖 Ejemplos por Escenario

### Escenario 1: Clase Grande (300+ líneas)

**Problema**: `VentaService` tiene 350 líneas.

**Diagnóstico**:
1. Abre [cohesion-coupling.md](cohesion-coupling.md)
2. Busca sección "Cohesión a Nivel de Clase"
3. Lee ejemplo "Baja Cohesión: Clase con Múltiples Responsabilidades"

**Solución**:
- Aplicar **Extract Class** para separar responsabilidades
- Seguir ejemplo de refactoring mostrado

### Escenario 2: Muchos If/Else

**Problema**: Método con 10 `if/else` según tipo de objeto.

**Diagnóstico**:
1. Abre [solid-principles.md](solid-principles.md)
2. Busca sección "Open/Closed Principle"
3. Lee ejemplo "Problema: Modificar Código Existente"

**Solución**:
- Aplicar **Strategy Pattern**
- Consultar `design_patterns/strategy.md` para implementación

### Escenario 3: Dependencias Circulares

**Problema**: `VentaService` usa `ClienteService` y viceversa.

**Diagnóstico**:
1. Abre [cohesion-coupling.md](cohesion-coupling.md)
2. Busca sección "Alto Acoplamiento: Dependencias Cíclicas"
3. Lee las 3 soluciones propuestas

**Solución**:
- Invertir dependencia (unidireccional)
- Extraer interfaz compartida
- Usar eventos asíncronos

### Escenario 4: Difícil de Testear

**Problema**: Clase imposible de testear sin BD/email reales.

**Diagnóstico**:
1. Abre [solid-principles.md](solid-principles.md)
2. Busca sección "Dependency Inversion Principle"
3. Lee ejemplo "Problema: Dependencia de Implementaciones Concretas"

**Solución**:
- Introducir **interfaces** para dependencias
- Usar **Dependency Injection** con Spring
- Testear con **mocks** de interfaces

## 🎓 Plan de Aprendizaje

### Nivel 1: Fundamentos (1 semana)

**Día 1-2**: Single Responsibility Principle
- Leer sección SRP en `solid-principles.md`
- Identificar 3 clases en tu código que violen SRP
- Refactorizar una usando Extract Class

**Día 3-4**: Cohesión
- Leer niveles de cohesión en `cohesion-coupling.md`
- Evaluar cohesión de 5 clases tuyas
- Mejorar la de menor cohesión

**Día 5**: Acoplamiento
- Leer tipos de acoplamiento
- Calcular Ca, Ce, I de un módulo tuyo
- Reducir acoplamiento si I > 0.7

### Nivel 2: Intermedio (2 semanas)

**Semana 1**: Principios SOLID completos
- Leer todos los principios en orden
- Hacer checklist de cada uno
- Aplicar al menos 1 refactoring de cada tipo

**Semana 2**: Cohesión y Acoplamiento avanzado
- Leer secciones de paquetes
- Reorganizar un módulo por dominio
- Eliminar dependencias circulares si existen

### Nivel 3: Experto (1 mes)

**Semana 1-2**: Integración con Patrones
- Relacionar cada principio SOLID con patrones de diseño
- Implementar 3 patrones en código real
- Medir mejora en métricas

**Semana 3-4**: Code Reviews
- Revisar código de 10+ PRs aplicando principios
- Proporcionar feedback concreto con referencias
- Entrenar a 2 junior developers

## 🔗 Integración con Otros Recursos

### Agentes Especializados

| Agente | Cuándo Usar | Relación con Good Practices |
|--------|-------------|----------------------------|
| [code-quality-analyzer](../code-quality-analyzer.md) | Analizar código existente | Evalúa SOLID + cohesión/acoplamiento |
| [design-patterns-expert](../design-patterns-expert.md) | Necesitas aplicar un patrón | Patrones resuelven violaciones de OCP, SRP |
| [use-case-expert](../use-case-expert.md) | Documentar casos de uso | SRP aplicado a documentación |

### Patrones de Diseño

Los patrones de diseño en `design_patterns/` son soluciones que emergen de aplicar estos principios:

| Principio Violado | Patrón que Ayuda | Ejemplo |
|-------------------|------------------|---------|
| **SRP** | Facade, Proxy | Separar responsabilidades |
| **OCP** | Strategy, Template Method | Extensión sin modificación |
| **LSP** | Composition over Inheritance | Evitar jerarquías incorrectas |
| **ISP** | Interface Segregation | Interfaces pequeñas |
| **DIP** | Dependency Injection, Abstract Factory | Depender de abstracciones |

### Templates

Los templates en `templates/` aplican estos principios a documentación:

- `agent.md`: SRP aplicado (un agente, un dominio)
- `caso-uso.md`: Alta cohesión (todo sobre un caso de uso)
- `adr.md`: SRP (una decisión arquitectónica)

## ❓ FAQ

**P: ¿Debo aplicar SOLID a TODO el código?**  
R: Usa sentido común. Código simple (DTOs, modelos de 20 líneas) no necesita sobre-ingeniería. Aplica SOLID a lógica de negocio compleja.

**P: ¿Cómo sé si mi cohesión es buena?**  
R: Si todos los métodos de tu clase usan los mismos campos y trabajan hacia el mismo objetivo, tienes cohesión funcional (nivel 7 - el mejor).

**P: ¿Cuántas dependencias son demasiadas?**  
R: Si tu clase depende de > 7 clases externas, probablemente hace demasiado. Busca oportunidades de Extract Class.

**P: ¿Está bien tener acoplamiento?**  
R: Sí, el acoplamiento es inevitable. El objetivo es tener **bajo acoplamiento** (dependencias de interfaces, pocos módulos) y del tipo correcto (Message o Data, no Content o Common).

**P: ¿SOLID se aplica a JavaScript/Python/otros lenguajes?**  
R: Sí. Los principios SOLID son sobre diseño OOP, no sobre Java específicamente. Se aplican a cualquier lenguaje orientado a objetos.

**P: ¿Cómo convenzo a mi equipo de aplicar esto?**  
R: Empieza con métricas. Muestra que código con baja cohesión tiene más bugs y es más difícil de mantener. Haz demos de refactorings con mejoras medibles.

## 📚 Referencias Adicionales

### Libros
- **Clean Code** - Robert C. Martin (Uncle Bob)
- **Clean Architecture** - Robert C. Martin
- **Refactoring** - Martin Fowler
- **Design Patterns** - Gang of Four
- **Object-Oriented Software Construction** - Bertrand Meyer


### Documentación Interna
- [Design Patterns](../design_patterns/README.md)
- [Code Quality Analyzer Agent](../code-quality-analyzer.md)

---

## 📈 Métricas de Esta Documentación

```
📝 Archivos:                    2
📏 Líneas totales:              3,000+
🎯 Principios cubiertos:        7 (SOLID + Cohesión + Acoplamiento)
💡 Ejemplos de código:          40+
📊 Tablas comparativas:         15+
✅ Casos de uso empresariales:  10+
🔗 Referencias externas:        20+
```

---

**Última actualización**: 2026-01-18
**Versión**: 1.1.0

---

<div align="center">

**⭐ Código de calidad = Principios sólidos + Práctica constante**

</div>
