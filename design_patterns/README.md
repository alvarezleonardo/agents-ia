# Patrones de Diseño - Documentación Completa

Esta carpeta contiene documentación detallada de los patrones de diseño GoF (Gang of Four) con ejemplos prácticos y casos de uso para proyectos empresariales.

## 📚 Índice de Patrones

### Patrones Creacionales
Patrones que se enfocan en la creación de objetos de manera que se maximice la flexibilidad y reutilización del código.

| Patrón | Archivo | Descripción | Ejemplo de Uso |
|--------|---------|-------------|----------------|
| **Factory Method** | [factory-method.md](factory-method.md) | Define interfaz para crear objetos, pero deja que las subclases decidan qué clase instanciar | Generadores de reportes por formato |
| **Singleton** | [singleton.md](singleton.md) | Garantiza una única instancia de una clase con acceso global | Connection Pool Manager |

### Patrones Estructurales
Patrones que se ocupan de cómo se componen las clases y objetos para formar estructuras más grandes.

| Patrón | Archivo | Descripción | Ejemplo de Uso |
|--------|---------|-------------|----------------|
| **Adapter** | [adapter.md](adapter.md) | Convierte la interfaz de una clase en otra que los clientes esperan | Adaptador para BD legacy |
| **Composite** | [composite.md](composite.md) | Compone objetos en estructuras de árbol para representar jerarquías | Jerarquías de organizaciones |
| **Decorator** | [decorator.md](decorator.md) | Añade responsabilidades a objetos dinámicamente | Descuentos y promociones en cascada |
| **Proxy** | [proxy.md](proxy.md) | Proporciona un sustituto o marcador para controlar el acceso a un objeto | Services con cache y logging |

### Patrones Comportamentales
Patrones que se ocupan de algoritmos y la asignación de responsabilidades entre objetos.

| Patrón | Archivo | Descripción | Ejemplo de Uso |
|--------|---------|-------------|----------------|
| **Memento** | [memento.md](memento.md) | Captura y externaliza el estado interno de un objeto sin violar encapsulación | Snapshots de transacciones |
| **Observer** | [observer.md](observer.md) | Define dependencia uno-a-muchos entre objetos | Sistema de eventos y notificaciones |
| **State** | [state.md](state.md) | Permite que un objeto altere su comportamiento cuando su estado interno cambia | Estados de pedido/orden |
| **Strategy** | [strategy.md](strategy.md) | Define familia de algoritmos intercambiables | Calculadores de precio dinámicos |


## 📖 Estructura de Cada Documento

Cada archivo de patrón incluye:

1. **📋 Propósito**: Qué problema resuelve el patrón
2. **🎯 Problema**: Contexto y motivación
3. **💡 Solución**: Cómo el patrón resuelve el problema
4. **🏗️ Estructura**: Diagrama de clases (Mermaid)
5. **👥 Participantes**: Roles de cada componente
6. **💻 Ejemplos**: 3+ ejemplos completos con código Java
   - Ejemplo genérico/clásico
   - Ejemplo de dominio común
   - **Ejemplo de aplicación empresarial**
7. **⚖️ Ventajas y Desventajas**: Trade-offs del patrón
8. **✅ Aplicabilidad**: Cuándo usarlo
9. **🔗 Patrones Relacionados**: Similitudes y diferencias
10. **📚 Referencias**: Fuentes y lecturas adicionales

## 🎯 Ejemplos de Aplicaciones Empresariales

Cada patrón incluye ejemplos prácticos de aplicaciones empresariales:

- **Composite**: Jerarquía de organizaciones, departamentos y equipos
- **Adapter**: Conversión de APIs legacy a interfaces modernas
- **Decorator**: Sistema de descuentos y promociones encadenados
- **Memento**: Snapshots de transacciones para rollback
- **Observer**: Sistema de notificaciones y eventos
- **Factory Method**: Generadores de reportes (PDF, Excel, CSV)
- **Strategy**: Calculadores de precios y tarifas dinámicos
- **Singleton**: Pool de conexiones a base de datos
- **State**: Flujo de estados de órdenes/pedidos (Pendiente → Confirmado → En Proceso → Completado)
- **Proxy**: Services con cache, logging y validaciones

## 🚀 Cómo Usar Esta Documentación

### Para Desarrolladores Nuevos
1. Comienza con los patrones más comunes: **Singleton**, **Factory Method**, **Strategy**
2. Estudia los diagramas de estructura antes de ver el código
3. Ejecuta los ejemplos genéricos para entender el concepto
4. Revisa los ejemplos empresariales para ver aplicación real

### Para Code Reviews
- Identifica patrones usados en el código
- Verifica que se implementen correctamente según la documentación
- Sugiere patrones apropiados cuando encuentres problemas recurrentes

### Para Nuevas Features
1. Identifica el problema (creacional, estructural, comportamental)
2. Busca patrones aplicables en esta documentación
3. Adapta el ejemplo a tu caso específico
4. Valida con el equipo antes de implementar

## 📊 Clasificación Rápida

### Por Categoría

**Creacionales** (Cómo se crean objetos):
- Factory Method
- Singleton

**Estructurales** (Cómo se componen objetos):
- Adapter
- Composite
- Decorator
- Proxy

**Comportamentales** (Cómo interactúan objetos):
- Memento
- Observer
- State
- Strategy

### Por Complejidad

**Básicos** (Fáciles de entender e implementar):
- Singleton
- Strategy
- Observer

**Intermedios** (Requieren algo más de planificación):
- Factory Method
- Adapter
- Decorator
- State

**Avanzados** (Requieren diseño cuidadoso):
- Composite
- Memento
- Proxy

### Por Frecuencia de Uso

**Muy Frecuente** (Usado en múltiples módulos):
- Strategy (calculadores, validadores, procesadores)
- Singleton (servicios, configuración, pools de conexión)
- Observer (eventos, notificaciones, listeners)

**Frecuente** (Usado en módulos específicos):
- Decorator (funcionalidades adicionales, permisos)
- Adapter (integraciones legacy, APIs externas)
- Proxy (caching, logging, seguridad)

**Ocasional** (Casos específicos):
- Factory Method (reportes, exportadores, builders)
- State (flujos de negocio, máquinas de estado)
- Composite (jerarquías organizacionales, árboles)
- Memento (undo/redo, snapshots, historial)

## 🛠️ Mejores Prácticas

### Al Implementar Patrones

1. **No sobre-diseñes**: Usa patrones solo cuando resuelvan un problema real
2. **Mantén simplicidad**: El patrón debe simplificar, no complicar
3. **Documenta el uso**: Indica qué patrón usas y por qué
4. **Sigue convenciones**: Usa nombres estándar del patrón

### Nombres Estándar

- **Strategy**: `XXXStrategy`, `XXXCalculator`
- **Factory**: `XXXFactory`, `XXXCreator`
- **Decorator**: `XXXDecorator`, `BaseXXX + EnhancedXXX`
- **Observer**: `XXXListener`, `XXXObserver`, `XXXEvent`
- **Adapter**: `XXXAdapter`, `XXXWrapper`
- **Proxy**: `XXXProxy`, `CachedXXX`, `LazyXXX`

### Code Smells que Indican Necesidad de Patrones

| Code Smell | Patrón Sugerido |
|------------|-----------------|
| Muchos `if/switch` según tipo de objeto | Factory Method |
| Muchos `if/switch` según estado | State |
| Muchos `if/switch` según algoritmo | Strategy |
| Clases que necesitan notificar a muchas otras | Observer |
| Necesidad de añadir funcionalidad sin modificar clase | Decorator |
| Múltiples instancias de objeto que debería ser único | Singleton |
| Interfaces incompatibles entre componentes | Adapter |

## 📝 Convenciones de Código

### Ejemplos Java
- Todos los ejemplos usan Java 17+
- Incluyen imports necesarios
- Tienen salida de consola comentada
- Usan nombres descriptivos y significativos

### Diagramas Mermaid
- Todos los patrones tienen diagrama de clases
- Usan notación UML estándar
- Incluyen relaciones (herencia, composición, agregación)

## 🔗 Recursos Adicionales

### Libros Recomendados
- **Design Patterns: Elements of Reusable Object-Oriented Software** (Gang of Four)
- **Head First Design Patterns** (Freeman & Freeman)
- **Refactoring: Improving the Design of Existing Code** (Martin Fowler)
- **Effective Java** (Joshua Bloch)

### Referencias Online
- [Libro - Design Patterns](https://drive.google.com/file/d/1lJj0-tWCS_Ou1im76XgwOYmLcunVJtX7/view)
- [Spring Framework - Design Patterns](https://spring.io/guides)

## 🤝 Contribuir

Al agregar o mejorar documentación de patrones:

1. Mantén la estructura estándar de 10 secciones
2. Incluye al menos 3 ejemplos con código completo y ejecutable
3. Agrega diagrama Mermaid de la estructura
4. Proporciona ejemplos prácticos aplicables a proyectos reales
5. Lista ventajas, desventajas y aplicabilidad
6. Actualiza este README si agregas nuevos patrones

## 📅 Historial

- **2026-01-18**: Generalización de documentación
  - Eliminación de referencias específicas de proyecto
  - Ejemplos generalizados para uso universal

- **2026-01-07**: Refactorización completa de documentación
  - Separación de patrones combinados en archivos individuales
  - Adición de ejemplos completos con código ejecutable
  - Inclusión de casos de uso empresariales
  - Estandarización de estructura en todos los documentos
  - Migración de formato slides a documentación profesional

---
*Documentación de patrones de diseño para desarrollo de software*
