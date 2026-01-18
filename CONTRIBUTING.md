# Guía de Contribución

¡Gracias por tu interés en contribuir a Claude Agents Collection! Esta guía te ayudará a realizar contribuciones de calidad.

## 📋 Tabla de Contenidos

- [Código de Conducta](#código-de-conducta)
- [¿Cómo Puedo Contribuir?](#cómo-puedo-contribuir)
- [Proceso de Desarrollo](#proceso-de-desarrollo)
- [Estándares de Calidad](#estándares-de-calidad)
- [Convenciones de Commits](#convenciones-de-commits)

## 🤝 Código de Conducta

Este proyecto sigue un código de conducta profesional y respetuoso:

- Sé respetuoso con todos los contribuidores
- Acepta críticas constructivas
- Enfócate en lo mejor para la comunidad
- Muestra empatía hacia otros miembros

## 🎯 ¿Cómo Puedo Contribuir?

### Reportar Bugs

Si encuentras un bug, abre un issue con:

- **Descripción clara** del problema
- **Pasos para reproducir** el comportamiento
- **Comportamiento esperado** vs actual
- **Screenshots** si es aplicable
- **Entorno**: OS, versión de Claude CLI, etc.

### Sugerir Mejoras

Para sugerir mejoras o nuevas funcionalidades:

- Usa el template de Feature Request
- Explica el **caso de uso** y el **valor** que aporta
- Considera incluir **ejemplos** de implementación

### Contribuir con Código

1. **Fork** el proyecto
2. **Crea una rama** desde `main`:
   ```bash
   git checkout -b feature/nombre-descriptivo
   ```
3. **Realiza tus cambios** siguiendo los estándares
4. **Commit** tus cambios con mensajes descriptivos
5. **Push** a tu fork
6. **Abre un Pull Request** con descripción detallada

## 🔧 Proceso de Desarrollo

### Configuración del Entorno

```bash
# Clonar el repositorio
git clone https://github.com/tu-usuario/claude-agents-collection.git
cd claude-agents-collection

# Crear rama de trabajo
git checkout -b feature/mi-contribucion
```

### Estructura de un Agente

Todos los agentes deben seguir esta estructura:

```markdown
# [Nombre del Agente] Agent

Eres un [rol] experto en [área].

## Tu Rol
[Descripción clara del rol]

## Conocimiento Base
[Tecnologías y áreas de expertise]

## Instrucciones
[Guía paso a paso numerada]

## Mejores Prácticas
[Best practices con ejemplos ✅/❌]

## Ejemplos de Uso
[Mínimo 3 ejemplos concretos]

## Referencias
[Enlaces a documentación]

## Contexto del Proyecto
$ARGUMENTS
```

### Agregar un Nuevo Agente

1. **Copia el template**:
   ```bash
   cp templates/agent.md mi-nuevo-agente.md
   ```

2. **Completa todas las secciones**:
   - Usa placeholders `[nombre]` para identificar qué llenar
   - Incluye ejemplos específicos de tu dominio
   - Agrega referencias a documentación oficial

3. **Prueba tu agente**:
   ```bash
   cp mi-nuevo-agente.md ~/.claude/agents/
   # Prueba en Claude CLI
   ```

4. **Actualiza el README**:
   - Agrega tu agente a la sección correspondiente
   - Actualiza estadísticas si es necesario
   - Verifica los enlaces

### Mejorar Documentación Existente

**Para Design Patterns:**
- Sigue la estructura de 10 secciones
- Incluye diagramas Mermaid
- Agrega 3+ ejemplos de código
- Documenta ventajas/desventajas

**Para Good Practices:**
- Usa ejemplos antes/después
- Incluye métricas cuando sea posible
- Referencia principios SOLID
- Agrega casos de uso reales

## ✅ Estándares de Calidad

### Checklist Pre-Commit

Antes de hacer commit, verifica:

- [ ] **Formato**: Markdown correcto y consistente
- [ ] **Ortografía**: Sin errores de escritura
- [ ] **Completitud**: Todas las secciones requeridas
- [ ] **Ejemplos**: Mínimo 3 ejemplos funcionales
- [ ] **Referencias**: Enlaces válidos y actualizados
- [ ] **README**: Actualizado con tu contribución
- [ ] **Pruebas**: Agente probado en Claude CLI

### Estándares de Código

**Para ejemplos de código:**
- Usa sintaxis clara y legible
- Incluye comentarios cuando sea necesario
- Sigue las convenciones del lenguaje
- Código ejecutable y probado

**Para documentación:**
- Usa presente simple
- Sé específico y conciso
- Evita ambigüedades
- Usa listas y tablas para claridad

### Convenciones de Nombres

- **Archivos**: `kebab-case.md` (e.g., `backend-developer.md`)
- **Títulos**: Title Case para secciones principales
- **Variables**: Usa placeholders claros como `[nombre]`, `[descripción]`

## 📝 Convenciones de Commits

Usa [Conventional Commits](https://www.conventionalcommits.org/):

```
<tipo>(<alcance>): <descripción>

[cuerpo opcional]

[footer opcional]
```

### Tipos de Commits

- `feat`: Nueva funcionalidad o agente
- `fix`: Corrección de bugs
- `docs`: Cambios en documentación
- `style`: Formato, espacios (no afecta código)
- `refactor`: Refactorización de código
- `test`: Agregar o corregir tests
- `chore`: Tareas de mantenimiento

### Ejemplos

```bash
# Nuevo agente
git commit -m "feat(agents): add python-backend-expert agent"

# Mejora de documentación
git commit -m "docs(design-patterns): add Observer pattern examples"

# Corrección de bug
git commit -m "fix(qa-testing): correct SQL query validation"

# Actualización de README
git commit -m "docs(readme): update installation instructions"
```

## 🔍 Proceso de Review

### Para Revisores

Al revisar un PR, verifica:

1. **Calidad del contenido**:
   - ¿El agente tiene un propósito claro?
   - ¿La documentación es completa?
   - ¿Los ejemplos son útiles y funcionales?

2. **Consistencia**:
   - ¿Sigue la estructura estándar?
   - ¿Usa el mismo estilo que otros agentes?
   - ¿Las referencias son válidas?

3. **Valor agregado**:
   - ¿Aporta algo nuevo a la colección?
   - ¿No duplica funcionalidad existente?
   - ¿Es útil para la comunidad?

### Para Contribuidores

Después de abrir un PR:

- Responde a comentarios prontamente
- Realiza los cambios solicitados
- Mantén el PR actualizado con `main`
- Sé receptivo al feedback

## 🚀 Despliegue

Los maintainers se encargarán de:

1. Revisar y aprobar PRs
2. Hacer merge a `main`
3. Actualizar versiones
4. Publicar releases

## 💬 ¿Preguntas?

Si tienes preguntas:

- Abre un [Discussion](../../discussions)
- Pregunta en un [Issue](../../issues)
- Revisa la [documentación](README.md)

## 📄 Licencia

Al contribuir, aceptas que tu contribución se licenciará bajo la misma [MIT License](LICENSE) del proyecto.

---

**¡Gracias por contribuir a hacer esta colección más útil para todos! 🎉**
