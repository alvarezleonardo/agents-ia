# Política de Seguridad

## Versiones Soportadas

Este proyecto actualmente soporta las siguientes versiones con actualizaciones de seguridad:

| Versión | Soportada          |
| ------- | ------------------ |
| 1.0.x   | :white_check_mark: |
| < 1.0   | :x:                |

## Reportar una Vulnerabilidad

La seguridad de nuestros usuarios es nuestra prioridad principal. Si descubres una vulnerabilidad de seguridad, por favor ayúdanos reportándola de manera responsable.

### Proceso de Reporte

**NO** abras un issue público si descubres una vulnerabilidad de seguridad.

En su lugar, por favor:

1. **Envía un reporte privado** a través de GitHub Security Advisories:
   - Ve a la pestaña "Security" del repositorio
   - Haz clic en "Report a vulnerability"
   - Completa el formulario con los detalles

2. **O envía un email** a [tu-email-de-seguridad@ejemplo.com] con:
   - Descripción detallada de la vulnerabilidad
   - Pasos para reproducir el problema
   - Posible impacto
   - Cualquier sugerencia de remediación

### Lo que Puedes Esperar

Después de reportar una vulnerabilidad:

- **Acuse de recibo**: Dentro de 48 horas
- **Evaluación inicial**: Dentro de 5 días hábiles
- **Actualizaciones regulares**: Al menos cada 7 días
- **Resolución**: Depende de la severidad
  - Crítica: 1-7 días
  - Alta: 7-14 días
  - Media: 14-30 días
  - Baja: 30-90 días

### Divulgación Coordinada

Trabajaremos contigo para:

1. Confirmar la vulnerabilidad
2. Desarrollar una solución
3. Preparar un anuncio
4. Publicar el fix y el anuncio simultáneamente

Pedimos que mantengas la vulnerabilidad confidencial hasta que publiquemos el fix.

## Alcance de Seguridad

### En Alcance

- Vulnerabilidades en agentes que puedan exponer información sensible
- Código malicioso en ejemplos o documentación
- Inyección de código en prompts de agentes
- Escalación de privilegios
- Exposición de información sensible

### Fuera de Alcance

- Vulnerabilidades en dependencias de terceros (reportar directamente al proveedor)
- Ataques que requieren acceso físico al dispositivo
- Vulnerabilidades en Claude CLI (reportar a Anthropic)
- Social engineering

## Mejores Prácticas de Seguridad

### Para Usuarios

1. **Mantén actualizados los agentes**:
   ```bash
   git pull origin main
   cp *.md ~/.claude/agents/
   ```

2. **Revisa el código antes de usar**:
   - Examina los agentes antes de copiarlos
   - Verifica que los ejemplos no contengan código malicioso

3. **No compartas información sensible**:
   - No incluyas credenciales en prompts
   - No expongas secrets en ejemplos

4. **Usa entornos seguros**:
   - Ejecuta agentes en entornos de desarrollo aislados
   - No uses en producción sin revisión completa

### Para Contribuidores

1. **Revisa tu código**:
   - No incluyas secrets o credenciales
   - Sanitiza ejemplos de código
   - Valida inputs en scripts

2. **Documenta riesgos**:
   - Menciona precauciones necesarias
   - Documenta permisos requeridos
   - Advierte sobre operaciones peligrosas

3. **Sigue principios de seguridad**:
   - Principio de menor privilegio
   - Defense in depth
   - Secure by default

## Vulnerabilidades Conocidas

Actualmente no hay vulnerabilidades conocidas en este proyecto.

### Historial de Vulnerabilidades

No se han reportado vulnerabilidades hasta la fecha.

## Reconocimientos

Agradecemos a los siguientes investigadores de seguridad por reportar vulnerabilidades de manera responsable:

*Ninguno hasta la fecha*

Si reportas una vulnerabilidad, con tu permiso, te añadiremos aquí con un agradecimiento.

## Política de Actualización

- **Parches de seguridad críticos**: Publicados inmediatamente
- **Parches de alta prioridad**: Publicados en próxima versión menor
- **Parches de media/baja prioridad**: Incluidos en próxima versión mayor

## Recursos Adicionales

- [OWASP Top 10](https://owasp.org/www-project-top-ten/)
- [CWE Top 25](https://cwe.mitre.org/top25/)
- [GitHub Security Best Practices](https://docs.github.com/en/code-security)

## Contacto

Para preguntas sobre esta política de seguridad:

- Abre un [Discussion](../../discussions)
- Envía un email a [tu-email@ejemplo.com]

---

**Última actualización**: 18 de enero de 2026

**Versión de la política**: 1.0
