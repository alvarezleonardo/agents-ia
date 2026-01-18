# Cohesión y Acoplamiento

La cohesión y el acoplamiento son dos métricas fundamentales que miden la calidad del diseño de software a nivel de clases, módulos y paquetes.

## Tabla de Contenidos

- [Conceptos Fundamentales](#conceptos-fundamentales)
- [Cohesión](#cohesión)
  - [Niveles de Cohesión](#niveles-de-cohesión)
  - [Cohesión a Nivel de Clase](#cohesión-a-nivel-de-clase)
  - [Cohesión a Nivel de Paquete](#cohesión-a-nivel-de-paquete)
- [Acoplamiento](#acoplamiento)
  - [Tipos de Acoplamiento](#tipos-de-acoplamiento)
  - [Acoplamiento a Nivel de Clase](#acoplamiento-a-nivel-de-clase)
  - [Acoplamiento a Nivel de Paquete](#acoplamiento-a-nivel-de-paquete)
- [Métricas y Medición](#métricas-y-medición)
- [Principios Relacionados](#principios-relacionados)
- [Ejemplos de Proyecto](#ejemplos-de-proyecto)
- [Referencias](#referencias)

---

## Conceptos Fundamentales

### Cohesión

> "La cohesión mide qué tan relacionados están los elementos dentro de un módulo"

**Alta cohesión** = Los elementos de un módulo están fuertemente relacionados y trabajan juntos hacia un objetivo común.

```
📦 Módulo con Alta Cohesión
┌─────────────────────────┐
│  VentaService           │
│  ├─ calcularTotal()     │ ← Todas las operaciones
│  ├─ aplicarDescuento()  │   relacionadas con
│  └─ validarStock()      │   procesamiento de ventas
└─────────────────────────┘
```

**Baja cohesión** = Los elementos de un módulo hacen cosas no relacionadas entre sí.

```
❌ Módulo con Baja Cohesión
┌─────────────────────────┐
│  UtilService            │
│  ├─ calcularTotal()     │ ← Sin relación clara
│  ├─ enviarEmail()       │   entre métodos
│  ├─ formatearFecha()    │
│  └─ generarPDF()        │
└─────────────────────────┘
```

### Acoplamiento

> "El acoplamiento mide el grado de dependencia entre módulos"

**Bajo acoplamiento** = Los módulos son independientes y tienen pocas dependencias.

```
✅ Bajo Acoplamiento
┌──────────────┐     Interface     ┌──────────────┐
│ VentaService │────────────────────▶│   Repository │
└──────────────┘                     └──────────────┘
     ↓ Depende de abstracción            ↑
     ↓                                   │
┌──────────────────────────────────────┘
│ Implementación puede cambiar sin
│ afectar VentaService
```

**Alto acoplamiento** = Los módulos están fuertemente interdependientes.

```
❌ Alto Acoplamiento
┌──────────────┐  conoce detalles  ┌────────────────────┐
│ VentaService │────────────────────▶│ MySQLRepository    │
└──────────────┘                     │  + connection      │
     ↓                               │  + executeQuery()  │
     ↓                               └────────────────────┘
     └─ Cambio en BD rompe VentaService
```

### Objetivo del Buen Diseño

```
🎯 OBJETIVO: Alta Cohesión + Bajo Acoplamiento

✅ Alta Cohesión:
   - Módulos enfocados en una tarea
   - Fácil de entender y mantener
   - Cambios localizados

✅ Bajo Acoplamiento:
   - Módulos independientes
   - Fácil de reutilizar y testear
   - Cambios no se propagan
```

---

## Cohesión

### Niveles de Cohesión

De **peor** (baja cohesión) a **mejor** (alta cohesión):

| Nivel | Nombre | Descripción | Ejemplo |
|-------|--------|-------------|---------|
| 1️⃣ | **Coincidental** | Elementos sin relación, agrupados arbitrariamente | `Utils`, `Helper` con métodos random |
| 2️⃣ | **Logical** | Elementos relacionados lógicamente pero no funcionalmente | `IOManager` que maneja archivos, BD, red |
| 3️⃣ | **Temporal** | Elementos ejecutados al mismo tiempo | `Startup` que inicializa BD, config, logs |
| 4️⃣ | **Procedural** | Elementos ejecutados en secuencia | `OrderProcessor` que valida, cobra, envía |
| 5️⃣ | **Communicational** | Elementos que operan sobre los mismos datos | `ReporteVentas` con queries sobre tabla ventas |
| 6️⃣ | **Sequential** | Salida de uno es entrada de otro | Pipeline de procesamiento de pedidos |
| 7️⃣ | **Functional** | Todos los elementos contribuyen a una única tarea bien definida | `EmailService` solo envía emails |

**Objetivo**: Alcanzar cohesión **Functional** o **Sequential**.

---

### Cohesión a Nivel de Clase

#### ❌ Baja Cohesión: Clase con Múltiples Responsabilidades

```java
// COHESIÓN COINCIDENTAL: Métodos sin relación
public class UtilService {
    
    // Formateo de fechas
    public String formatearFecha(LocalDate fecha) {
        return fecha.format(DateTimeFormatter.ISO_DATE);
    }
    
    // Envío de emails
    public void enviarEmail(String destinatario, String mensaje) {
        // Código de email
    }
    
    // Generación de PDFs
    public byte[] generarPDF(String contenido) {
        // Código de PDF
    }
    
    // Cálculo de impuestos
    public BigDecimal calcularImpuesto(BigDecimal monto) {
        return monto.multiply(new BigDecimal("0.21"));
    }
    
    // Validación de CUIT
    public boolean validarCUIT(String cuit) {
        // Código de validación
    }
    
    // Compresión de archivos
    public byte[] comprimirArchivo(byte[] contenido) {
        // Código de compresión
    }
}
```

**Problemas**:
- 🔴 **Cohesión Coincidental** (nivel 1)
- 🔴 Clase de 500+ líneas
- 🔴 Cambios en cualquier funcionalidad afectan la clase completa
- 🔴 Nombre genérico sin significado (`Util`, `Helper`, `Manager`)
- 🔴 Difícil de testear (requiere muchos mocks)
- 🔴 Múltiples motivos para cambiar (viola SRP)

#### ✅ Alta Cohesión: Clases Especializadas

```java
// COHESIÓN FUNCIONAL: Cada clase hace UNA cosa bien definida

// 1. Servicio de formateo de fechas
@Service
public class DateFormatterService {
    
    private static final DateTimeFormatter ISO_FORMATTER = 
        DateTimeFormatter.ISO_DATE;
    
    private static final DateTimeFormatter DISPLAY_FORMATTER = 
        DateTimeFormatter.ofPattern("dd/MM/yyyy");
    
    public String formatearISO(LocalDate fecha) {
        return fecha.format(ISO_FORMATTER);
    }
    
    public String formatearDisplay(LocalDate fecha) {
        return fecha.format(DISPLAY_FORMATTER);
    }
    
    public LocalDate parsearISO(String fechaStr) {
        return LocalDate.parse(fechaStr, ISO_FORMATTER);
    }
}

// 2. Servicio de envío de emails
@Service
public class EmailService {
    
    private final JavaMailSender mailSender;
    private final EmailTemplateRenderer templateRenderer;
    
    public void enviarBienvenida(Usuario usuario) {
        String contenido = templateRenderer.render("bienvenida", usuario);
        enviar(usuario.getEmail(), "Bienvenido", contenido);
    }
    
    public void enviarComprobante(Venta venta) {
        String contenido = templateRenderer.render("comprobante", venta);
        enviar(venta.getCliente().getEmail(), "Comprobante de venta", contenido);
    }
    
    private void enviar(String destinatario, String asunto, String contenido) {
        SimpleMailMessage message = new SimpleMailMessage();
        message.setTo(destinatario);
        message.setSubject(asunto);
        message.setText(contenido);
        mailSender.send(message);
    }
}

// 3. Servicio de generación de PDFs
@Service
public class PDFGeneratorService {
    
    private final PDFRenderer pdfRenderer;
    
    public byte[] generarComprobante(Venta venta) {
        Document doc = pdfRenderer.createDocument();
        agregarEncabezado(doc, venta);
        agregarItems(doc, venta.getItems());
        agregarTotales(doc, venta);
        return doc.toByteArray();
    }
    
    public byte[] generarReporte(List<Venta> ventas) {
        Document doc = pdfRenderer.createDocument();
        agregarTitulo(doc, "Reporte de Ventas");
        agregarTablaVentas(doc, ventas);
        return doc.toByteArray();
    }
    
    private void agregarEncabezado(Document doc, Venta venta) {
        // Todos los métodos privados trabajan hacia el mismo objetivo
    }
    
    private void agregarItems(Document doc, List<VentaItem> items) {
        // Cohesión alta: métodos relacionados
    }
    
    private void agregarTotales(Document doc, Venta venta) {
        // Operan sobre los mismos datos
    }
}

// 4. Servicio de cálculo de impuestos
@Service
public class ImpuestoCalculator {
    
    private static final BigDecimal IVA_GENERAL = new BigDecimal("0.21");
    private static final BigDecimal IVA_REDUCIDO = new BigDecimal("0.105");
    
    public BigDecimal calcularIVA(BigDecimal monto, CategoriaIVA categoria) {
        BigDecimal tasa = obtenerTasa(categoria);
        return monto.multiply(tasa);
    }
    
    public BigDecimal calcularBaseImponible(BigDecimal montoConIVA, CategoriaIVA categoria) {
        BigDecimal tasa = obtenerTasa(categoria);
        return montoConIVA.divide(BigDecimal.ONE.add(tasa), 2, RoundingMode.HALF_UP);
    }
    
    private BigDecimal obtenerTasa(CategoriaIVA categoria) {
        return switch (categoria) {
            case GENERAL -> IVA_GENERAL;
            case REDUCIDO -> IVA_REDUCIDO;
            case EXENTO -> BigDecimal.ZERO;
        };
    }
}

// 5. Validador de documentos argentinos
@Service
public class DocumentoValidator {
    
    public boolean validarCUIT(String cuit) {
        if (!cuit.matches("\\d{11}")) {
            return false;
        }
        return validarDigitoVerificadorCUIT(cuit);
    }
    
    public boolean validarDNI(String dni) {
        return dni.matches("\\d{7,8}");
    }
    
    public String formatearCUIT(String cuit) {
        // XX-XXXXXXXX-X
        return cuit.substring(0, 2) + "-" + 
               cuit.substring(2, 10) + "-" + 
               cuit.substring(10);
    }
    
    private boolean validarDigitoVerificadorCUIT(String cuit) {
        // Algoritmo de validación CUIT
        int[] multiplicadores = {5, 4, 3, 2, 7, 6, 5, 4, 3, 2};
        int suma = 0;
        
        for (int i = 0; i < 10; i++) {
            suma += Character.getNumericValue(cuit.charAt(i)) * multiplicadores[i];
        }
        
        int resto = suma % 11;
        int digitoCalculado = 11 - resto;
        
        if (digitoCalculado == 11) digitoCalculado = 0;
        if (digitoCalculado == 10) digitoCalculado = 9;
        
        return digitoCalculado == Character.getNumericValue(cuit.charAt(10));
    }
}
```

**Beneficios de alta cohesión**:
- ✅ Cada clase tiene un propósito claro
- ✅ Nombre descriptivo que indica responsabilidad
- ✅ Métodos privados trabajan hacia el mismo objetivo
- ✅ Cambios localizados (cambio en PDF no afecta Email)
- ✅ Fácil de testear (una responsabilidad por test)
- ✅ Clases pequeñas (< 150 líneas)

---

### Cohesión a Nivel de Paquete

#### ❌ Baja Cohesión: Paquetes por Tipo Técnico

```
// ANTI-PATRÓN: Organización técnica (baja cohesión funcional)
com.empresa.backoffice
├── controllers/          ← Todos los controllers juntos
│   ├── VentaController
│   ├── ProductoController
│   ├── ClienteController
│   ├── EmpleadoController
│   └── ReporteController
│
├── services/             ← Todos los services juntos
│   ├── VentaService
│   ├── ProductoService
│   ├── ClienteService
│   ├── EmpleadoService
│   └── ReporteService
│
├── repositories/         ← Todos los repositories juntos
│   ├── VentaRepository
│   ├── ProductoRepository
│   ├── ClienteRepository
│   └── EmpleadoRepository
│
└── entities/            ← Todas las entidades juntas
    ├── Venta
    ├── Producto
    ├── Cliente
    └── Empleado
```

**Problemas**:
- 🔴 **Baja cohesión funcional**: Clases de diferentes dominios mezcladas
- 🔴 Cambio en dominio "Ventas" requiere tocar múltiples paquetes
- 🔴 Difícil encontrar todo lo relacionado con una feature
- 🔴 Acopla dominios que deberían estar separados
- 🔴 Dificulta modularización futura (microservicios)

#### ✅ Alta Cohesión: Paquetes por Dominio/Feature

```
// PATRÓN RECOMENDADO: Organización por dominio (alta cohesión)
com.empresa.backoffice
│
├── ventas/                      ← Todo lo de VENTAS
│   ├── VentaController
│   ├── VentaService
│   ├── VentaRepository
│   ├── Venta (entity)
│   ├── VentaDTO
│   ├── VentaMapper
│   ├── DescuentoStrategy
│   └── exceptions/
│       └── VentaInvalidaException
│
├── productos/                   ← Todo lo de PRODUCTOS
│   ├── ProductoController
│   ├── ProductoService
│   ├── ProductoRepository
│   ├── Producto (entity)
│   ├── CategoriaProducto (enum)
│   ├── StockService
│   └── PrecioCalculator
│
├── clientes/                    ← Todo lo de CLIENTES
│   ├── ClienteController
│   ├── ClienteService
│   ├── ClienteRepository
│   ├── Cliente (entity)
│   ├── DireccionService
│   └── validators/
│       └── ClienteValidator
│
├── empleados/                   ← Todo lo de RRHH
│   ├── EmpleadoController
│   ├── EmpleadoService
│   ├── EmpleadoRepository
│   ├── Empleado (entity)
│   ├── AsistenciaService
│   └── SalarioCalculator
│
├── reportes/                    ← Todo lo de REPORTES
│   ├── ReporteController
│   ├── ReporteService
│   ├── generators/
│   │   ├── PDFReporteGenerator
│   │   └── ExcelReporteGenerator
│   └── templates/
│       └── ReporteVentasTemplate
│
└── shared/                      ← Código compartido
    ├── exceptions/
    ├── security/
    ├── audit/
    └── config/
```

**Beneficios de alta cohesión por dominio**:
- ✅ Todo lo relacionado con "Ventas" está en un solo lugar
- ✅ Cambios en ventas no afectan otros dominios
- ✅ Fácil encontrar código relacionado
- ✅ Facilita extracción a microservicios
- ✅ Equipos pueden trabajar en dominios separados
- ✅ Tests organizados por dominio

#### Ejemplo Real: Módulo de Ventas

```java
// com.empresa.backoffice.ventas

// 1. Controlador
@RestController
@RequestMapping("/api/ventas")
public class VentaController {
    private final VentaService ventaService;
    
    @PostMapping
    public ResponseEntity<VentaDTO> crear(@RequestBody @Valid CrearVentaRequest request) {
        return ResponseEntity.ok(ventaService.crear(request));
    }
}

// 2. Servicio de dominio
@Service
public class VentaService {
    private final VentaRepository repository;
    private final DescuentoStrategy descuentoStrategy;
    private final StockService stockService; // ← Dependencia de otro dominio
    
    @Transactional
    public VentaDTO crear(CrearVentaRequest request) {
        // Lógica de negocio
    }
}

// 3. Repositorio
@Repository
public interface VentaRepository extends JpaRepository<Venta, Long> {
    List<Venta> findByClienteIdAndFechaBetween(Long clienteId, LocalDate inicio, LocalDate fin);
}

// 4. Entidad
@Entity
@Table(name = "venta")
public class Venta {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @ManyToOne
    private Cliente cliente; // ← Dependencia de otro dominio (OK)
    
    private BigDecimal total;
    private LocalDate fecha;
    
    @OneToMany(mappedBy = "venta")
    private List<VentaItem> items;
}

// 5. DTOs
public record VentaDTO(
    Long id,
    ClienteDTO cliente,
    BigDecimal total,
    LocalDate fecha,
    List<VentaItemDTO> items
) {}

// 6. Estrategias específicas del dominio
public interface DescuentoStrategy {
    BigDecimal calcular(Venta venta);
}

@Component
public class DescuentoPorVolumenStrategy implements DescuentoStrategy {
    public BigDecimal calcular(Venta venta) {
        // Lógica específica de ventas
    }
}
```

**Regla**: Todo lo que un desarrollador necesita para trabajar en "Ventas" está en el paquete `ventas/`.

---

## Acoplamiento

### Tipos de Acoplamiento

De **peor** (alto acoplamiento) a **mejor** (bajo acoplamiento):

| Nivel | Tipo | Descripción | Ejemplo |
|-------|------|-------------|---------|
| 6️⃣ | **Content** | Un módulo modifica datos internos de otro | Acceso directo a fields privados vía reflection |
| 5️⃣ | **Common** | Módulos comparten datos globales | Variables estáticas compartidas |
| 4️⃣ | **Control** | Un módulo controla el flujo de otro | Pasar flags para cambiar comportamiento |
| 3️⃣ | **Stamp** | Módulos comparten estructuras de datos complejas | Pasar objeto completo cuando solo se necesita un campo |
| 2️⃣ | **Data** | Módulos comparten datos simples | Pasar parámetros primitivos o DTOs |
| 1️⃣ | **Message** | Módulos se comunican vía interfaces/mensajes | Comunicación vía interfaces o eventos |

**Objetivo**: Lograr acoplamiento **Data** o **Message**.

---

### Acoplamiento a Nivel de Clase

#### ❌ Alto Acoplamiento: Dependencias Concretas

```java
// ACOPLAMIENTO COMÚN: Compartir estado global
public class ConfiguracionGlobal {
    // ❌ Variables estáticas mutables
    public static String DATABASE_URL = "jdbc:mysql://localhost:3306/database";
    public static int CONNECTION_POOL_SIZE = 20;
    public static boolean DEBUG_MODE = false;
}

public class VentaService {
    public void procesarVenta(Venta venta) {
        // ❌ Dependencia de estado global
        if (ConfiguracionGlobal.DEBUG_MODE) {
            System.out.println("Debug: Procesando venta " + venta.getId());
        }
        
        // ❌ Uso directo de configuración global
        Connection conn = DriverManager.getConnection(ConfiguracionGlobal.DATABASE_URL);
    }
}

public class ReporteService {
    public void generar() {
        // ❌ Múltiples clases acopladas al mismo estado global
        if (ConfiguracionGlobal.DEBUG_MODE) {
            System.out.println("Debug: Generando reporte");
        }
    }
}
```

**Problemas**:
- 🔴 **Acoplamiento Común** (nivel 5)
- 🔴 Cambio en configuración afecta todas las clases
- 🔴 Imposible testear (no se puede mockear estado estático)
- 🔴 Race conditions en ambientes concurrentes
- 🔴 Difícil rastrear quién modifica el estado

```java
// ACOPLAMIENTO DE CONTROL: Pasar flags
public class ReporteService {
    
    // ❌ Flag controla el flujo interno
    public byte[] generarReporte(String tipo, boolean incluirGraficos, 
                                 boolean incluirDetalle, boolean comprimido) {
        if (tipo.equals("PDF")) {
            if (incluirGraficos) {
                // Genera PDF con gráficos
            } else {
                // Genera PDF sin gráficos
            }
            
            if (incluirDetalle) {
                // Agrega sección de detalle
            }
            
            if (comprimido) {
                // Comprime el PDF
            }
        }
        // Método de 200+ líneas con múltiples branches
    }
}

// Cliente acoplado a lógica interna
public class VentaController {
    public ResponseEntity<?> descargarReporte() {
        // ❌ Cliente debe conocer flags internos
        byte[] pdf = reporteService.generarReporte("PDF", true, false, true);
        return ResponseEntity.ok(pdf);
    }
}
```

**Problemas**:
- 🔴 **Acoplamiento de Control** (nivel 4)
- 🔴 Cliente conoce lógica interna del servicio
- 🔴 Difícil agregar nuevas opciones (explosión combinatoria)
- 🔴 Código con muchos `if` anidados

```java
// ACOPLAMIENTO STAMP: Pasar objetos completos innecesariamente
public class EmailService {
    
    // ❌ Recibe objeto completo cuando solo necesita email
    public void enviarNotificacion(Usuario usuario, String mensaje) {
        // Solo usa usuario.getEmail(), pero recibe TODO el objeto
        sendMail(usuario.getEmail(), mensaje);
    }
}

public class AuditoriaService {
    
    // ❌ Recibe Venta completa cuando solo necesita ID y usuario
    public void registrar(Venta venta) {
        // Solo usa venta.getId() y venta.getUsuario()
        // Pero tiene acceso a items, cliente, totales, etc.
        log.info("Venta {} creada por {}", venta.getId(), venta.getUsuario());
    }
}
```

**Problemas**:
- 🔴 **Acoplamiento Stamp** (nivel 3)
- 🔴 Dependencias innecesarias (cambios en `Usuario` afectan `EmailService`)
- 🔴 Dificulta testing (mock de objeto complejo)
- 🔴 Riesgo de uso indebido de datos

#### ✅ Bajo Acoplamiento: Interfaces y DTOs

```java
// ACOPLAMIENTO DATA: Pasar datos simples
@Service
public class EmailService {
    
    private final JavaMailSender mailSender;
    
    // ✅ Solo recibe datos necesarios
    public void enviarNotificacion(String email, String mensaje) {
        SimpleMailMessage mail = new SimpleMailMessage();
        mail.setTo(email);
        mail.setText(mensaje);
        mailSender.send(mail);
    }
}

@Service
public class AuditoriaService {
    
    // ✅ DTO con solo datos necesarios
    public void registrar(AuditoriaDTO dto) {
        AuditoriaLog log = new AuditoriaLog(
            dto.entidad(),
            dto.entidadId(),
            dto.operacion(),
            dto.usuario()
        );
        repository.save(log);
    }
}

public record AuditoriaDTO(
    String entidad,
    Long entidadId,
    String operacion,
    String usuario
) {}
```

**Beneficios**:
- ✅ **Acoplamiento Data** (nivel 2)
- ✅ Dependencias explícitas y mínimas
- ✅ Fácil de testear (datos primitivos o DTOs simples)
- ✅ Cambios en entidades no afectan servicios

```java
// ACOPLAMIENTO MESSAGE: Interfaces y eventos
public interface NotificacionService {
    void notificar(Notificacion notificacion);
}

@Service
public class EmailNotificacionService implements NotificacionService {
    
    @Override
    public void notificar(Notificacion notificacion) {
        // Envío por email
    }
}

@Service
public class SMSNotificacionService implements NotificacionService {
    
    @Override
    public void notificar(Notificacion notificacion) {
        // Envío por SMS
    }
}

// Cliente desacoplado de implementación
@Service
public class VentaService {
    
    private final List<NotificacionService> notificadores;
    
    public void procesarVenta(Venta venta) {
        // Lógica de negocio
        
        Notificacion notif = new Notificacion(
            venta.getCliente().getEmail(),
            "Comprobante de venta",
            "Gracias por su compra"
        );
        
        // ✅ Desacoplado: no sabe cómo se notifica
        notificadores.forEach(n -> n.notificar(notif));
    }
}
```

**Beneficios**:
- ✅ **Acoplamiento Message** (nivel 1 - el mejor)
- ✅ Cliente no conoce implementaciones concretas
- ✅ Fácil agregar nuevos notificadores (SMS, Push, WhatsApp)
- ✅ Testeable con mocks

```java
// PATRÓN EVENT-DRIVEN: Acoplamiento mínimo
@Component
public class VentaService {
    
    private final ApplicationEventPublisher eventPublisher;
    
    @Transactional
    public Venta crear(VentaDTO dto) {
        Venta venta = ventaRepository.save(new Venta(dto));
        
        // ✅ Publicar evento sin conocer quién lo escucha
        eventPublisher.publishEvent(new VentaCreadaEvent(venta));
        
        return venta;
    }
}

// Listener 1: Notificaciones (otro módulo)
@Component
public class NotificacionListener {
    
    @EventListener
    @Async
    public void onVentaCreada(VentaCreadaEvent event) {
        // Enviar email al cliente
        emailService.enviarComprobante(event.getVenta());
    }
}

// Listener 2: Reportes (otro módulo)
@Component
public class ReporteListener {
    
    @EventListener
    public void onVentaCreada(VentaCreadaEvent event) {
        // Actualizar métricas en tiempo real
        metricsService.incrementarVentas();
    }
}

// Listener 3: Contabilidad (otro módulo)
@Component
public class ContabilidadListener {
    
    @EventListener
    public void onVentaCreada(VentaCreadaEvent event) {
        // Registrar asiento contable
        contabilidadService.registrarIngreso(event.getVenta());
    }
}
```

**Beneficios**:
- ✅ **Acoplamiento mínimo**: VentaService no conoce listeners
- ✅ Agregar funcionalidad sin modificar VentaService (OCP)
- ✅ Módulos completamente independientes
- ✅ Procesamiento asíncrono posible

---

### Acoplamiento a Nivel de Paquete

#### ❌ Alto Acoplamiento: Dependencias Cíclicas

```
// ANTI-PATRÓN: Dependencias circulares
┌─────────────────┐
│   ventas/       │
│  VentaService   │───┐
└─────────────────┘   │ usa
         ↑            ↓
         │      ┌─────────────────┐
         │      │  clientes/      │
         │      │ ClienteService  │
         │      └─────────────────┘
         │            │
         └────────────┘
              usa

❌ VentaService   → usa → ClienteService
❌ ClienteService → usa → VentaService

PROBLEMA: Dependencia cíclica (circular dependency)
```

**Ejemplo de código problemático**:

```java
// com.empresa.ventas.VentaService
@Service
public class VentaService {
    
    private final ClienteService clienteService; // ❌ Depende de clientes
    
    public Venta crear(VentaDTO dto) {
        // Necesita validar cliente
        Cliente cliente = clienteService.obtenerActivo(dto.getClienteId());
        
        if (!cliente.isActivo()) {
            throw new BusinessException("Cliente inactivo");
        }
        
        Venta venta = new Venta(dto);
        return ventaRepository.save(venta);
    }
}

// com.empresa.clientes.ClienteService
@Service
public class ClienteService {
    
    private final VentaService ventaService; // ❌ Depende de ventas
    
    public ClienteResumen obtenerResumen(Long clienteId) {
        Cliente cliente = clienteRepository.findById(clienteId).orElseThrow();
        
        // Necesita estadísticas de ventas
        List<Venta> ventas = ventaService.obtenerVentasCliente(clienteId);
        
        return new ClienteResumen(
            cliente,
            ventas.size(),
            calcularTotalComprado(ventas)
        );
    }
}
```

**Problemas**:
- 🔴 Dependencia circular (imposible determinar orden de inicialización)
- 🔴 Módulos fuertemente acoplados
- 🔴 Difícil extracción a microservicios
- 🔴 Spring puede fallar al inicializar (circular bean dependency)

#### ✅ Bajo Acoplamiento: Dependencias Unidireccionales

**Solución 1: Invertir Dependencia**

```
✅ CORRECTO: Dependencia unidireccional
┌─────────────────┐
│   ventas/       │
│  VentaService   │───┐
└─────────────────┘   │ usa
                      ↓
                ┌─────────────────┐
                │  clientes/      │
                │ ClienteService  │
                └─────────────────┘

VentaService → ClienteService (OK)
ClienteService NO usa VentaService
```

```java
// com.empresa.ventas.VentaService
@Service
public class VentaService {
    
    private final ClienteService clienteService;
    private final VentaRepository ventaRepository;
    
    public Venta crear(VentaDTO dto) {
        Cliente cliente = clienteService.obtenerActivo(dto.getClienteId());
        
        if (!cliente.isActivo()) {
            throw new BusinessException("Cliente inactivo");
        }
        
        Venta venta = new Venta(dto);
        return ventaRepository.save(venta);
    }
    
    // ✅ Método para obtener ventas de cliente (ahora en VentaService)
    public List<Venta> obtenerPorCliente(Long clienteId) {
        return ventaRepository.findByClienteId(clienteId);
    }
}

// com.empresa.clientes.ClienteService
@Service
public class ClienteService {
    
    // ✅ NO depende de VentaService
    private final ClienteRepository clienteRepository;
    
    public Cliente obtenerActivo(Long clienteId) {
        return clienteRepository.findByIdAndActivoTrue(clienteId)
            .orElseThrow(() -> new EntityNotFoundException("Cliente no encontrado"));
    }
    
    // ✅ Resumen SIN ventas (responsabilidad única)
    public ClienteBasicoDTO obtenerBasico(Long clienteId) {
        Cliente cliente = clienteRepository.findById(clienteId).orElseThrow();
        return ClienteBasicoDTO.from(cliente);
    }
}

// Nueva clase de más alto nivel que orquesta
@Service
public class ClienteResumenService {
    
    private final ClienteService clienteService;
    private final VentaService ventaService;
    
    // ✅ Orquestador que depende de ambos
    public ClienteResumenDTO obtenerResumen(Long clienteId) {
        ClienteBasicoDTO cliente = clienteService.obtenerBasico(clienteId);
        List<Venta> ventas = ventaService.obtenerPorCliente(clienteId);
        
        return new ClienteResumenDTO(
            cliente,
            ventas.size(),
            calcularTotal(ventas)
        );
    }
}
```

**Solución 2: Extraer Interfaz Compartida**

```
✅ PATRÓN: Interfaz compartida en módulo común
┌─────────────────┐         ┌─────────────────┐
│   ventas/       │         │  clientes/      │
│  VentaService   │         │ ClienteService  │
└─────────────────┘         └─────────────────┘
         ↓                           ↓
         └────────┬──────────────────┘
                  ↓
          ┌─────────────────┐
          │   shared/       │
          │ ClienteProvider │ ← Interfaz
          └─────────────────┘

VentaService → ClienteProvider (interfaz)
ClienteService implementa ClienteProvider
```

```java
// com.empresa.shared.cliente.ClienteProvider
public interface ClienteProvider {
    Optional<Cliente> obtenerActivo(Long id);
    boolean existeYEstaActivo(Long id);
}

// com.empresa.clientes.ClienteService
@Service
public class ClienteService implements ClienteProvider {
    
    @Override
    public Optional<Cliente> obtenerActivo(Long id) {
        return clienteRepository.findByIdAndActivoTrue(id);
    }
    
    @Override
    public boolean existeYEstaActivo(Long id) {
        return clienteRepository.existsByIdAndActivoTrue(id);
    }
    
    // Métodos adicionales específicos
    public ClienteResumenDTO obtenerResumen(Long id) {
        // ...
    }
}

// com.empresa.ventas.VentaService
@Service
public class VentaService {
    
    // ✅ Depende de interfaz, no de implementación
    private final ClienteProvider clienteProvider;
    
    public Venta crear(VentaDTO dto) {
        Cliente cliente = clienteProvider.obtenerActivo(dto.getClienteId())
            .orElseThrow(() -> new BusinessException("Cliente inactivo o no existe"));
        
        Venta venta = new Venta(dto);
        return ventaRepository.save(venta);
    }
}
```

**Solución 3: Eventos Asíncronos**

```java
// com.empresa.ventas.VentaService
@Service
public class VentaService {
    
    private final ApplicationEventPublisher eventPublisher;
    
    @Transactional
    public Venta crear(VentaDTO dto) {
        Venta venta = ventaRepository.save(new Venta(dto));
        
        // ✅ Publicar evento (acoplamiento mínimo)
        eventPublisher.publishEvent(new VentaCreadaEvent(venta));
        
        return venta;
    }
}

// com.empresa.clientes.ClienteService
@Service
public class ClienteService {
    
    // ✅ Escuchar evento (no depende de VentaService)
    @EventListener
    @Async
    public void onVentaCreada(VentaCreadaEvent event) {
        Long clienteId = event.getVenta().getClienteId();
        
        // Actualizar estadísticas del cliente
        clienteRepository.incrementarTotalCompras(clienteId);
    }
}
```

---

## Métricas y Medición

### Métricas de Cohesión

**LCOM (Lack of Cohesion of Methods)**

Mide la falta de cohesión en una clase. Valores bajos = alta cohesión.

```java
// ✅ LCOM Bajo (alta cohesión)
public class VentaCalculator {
    private BigDecimal subtotal;      // Usado por todos los métodos
    private BigDecimal descuento;     // Usado por todos los métodos
    private BigDecimal impuesto;      // Usado por todos los métodos
    
    public BigDecimal calcularSubtotal() {
        // Usa subtotal
    }
    
    public BigDecimal calcularDescuento() {
        // Usa subtotal y descuento
    }
    
    public BigDecimal calcularImpuesto() {
        // Usa subtotal, descuento e impuesto
    }
    
    public BigDecimal calcularTotal() {
        // Usa subtotal, descuento e impuesto
    }
}
// Todos los métodos usan los mismos campos → Alta cohesión

// ❌ LCOM Alto (baja cohesión)
public class UtilService {
    private Connection dbConnection;  // Solo usado por métodos de BD
    private SmtpClient emailClient;   // Solo usado por métodos de email
    private PDFRenderer pdfRenderer;  // Solo usado por métodos de PDF
    
    public void guardarEnBD() {
        // Solo usa dbConnection
    }
    
    public void enviarEmail() {
        // Solo usa emailClient
    }
    
    public void generarPDF() {
        // Solo usa pdfRenderer
    }
}
// Métodos no comparten campos → Baja cohesión (debería ser 3 clases)
```

**Herramientas para medir cohesión**:
- SonarQube
- Checkstyle
- IntelliJ IDEA metrics
- JDepend

### Métricas de Acoplamiento

**Afferent Coupling (Ca)**: Cuántas clases dependen de esta clase (acoplamiento entrante).

**Efferent Coupling (Ce)**: De cuántas clases depende esta clase (acoplamiento saliente).

**Instability (I) = Ce / (Ce + Ca)**:
- `I = 0`: Clase estable (muchas clases dependen de ella, no depende de otras)
- `I = 1`: Clase inestable (depende de muchas, pocas dependen de ella)

```java
// ✅ Clase estable (I cercano a 0)
public interface VentaRepository {
    // 20 clases dependen de esta interfaz (Ca = 20)
    // No depende de nadie más que JPA (Ce = 1)
    // I = 1 / (1 + 20) = 0.047 → Muy estable
}

// ❌ Clase inestable (I cercano a 1)
public class ReporteComplexService {
    // Depende de 15 clases (Ce = 15)
    // Solo 1 clase la usa (Ca = 1)
    // I = 15 / (15 + 1) = 0.937 → Muy inestable
    
    private VentaService ventaService;
    private ClienteService clienteService;
    private ProductoService productoService;
    // ... 12 dependencias más
}
```

**Herramientas para medir acoplamiento**:
- JDepend
- Structure101
- SonarGraph
- IntelliJ IDEA Dependency Matrix

---

## Principios Relacionados

### Relación con SOLID

| Principio SOLID | Relación con Cohesión/Acoplamiento |
|-----------------|-----------------------------------|
| **SRP** | ✅ Alta cohesión: Una clase, una responsabilidad |
| **OCP** | ✅ Bajo acoplamiento: Extender sin modificar (usar abstracciones) |
| **LSP** | ✅ Bajo acoplamiento: Sustituibilidad de subclases |
| **ISP** | ✅ Alta cohesión: Interfaces específicas y cohesivas |
| **DIP** | ✅ Bajo acoplamiento: Depender de abstracciones |

### Ley de Demeter (Principle of Least Knowledge)

> "Habla solo con tus amigos inmediatos, no con extraños"

```java
// ❌ VIOLA Ley de Demeter (alto acoplamiento)
public class VentaController {
    
    public BigDecimal obtenerPrecioProducto(Long ventaId, int itemIndex) {
        // ❌ Cadena larga de llamadas (train wreck)
        return ventaService
            .obtenerVenta(ventaId)
            .getItems()
            .get(itemIndex)
            .getProducto()
            .getPrecio()
            .getMontoBase();
        // Acoplado a estructura interna de 5 clases
    }
}

// ✅ RESPETA Ley de Demeter (bajo acoplamiento)
public class VentaController {
    
    public BigDecimal obtenerPrecioProducto(Long ventaId, int itemIndex) {
        // ✅ Delegar al servicio
        return ventaService.obtenerPrecioItem(ventaId, itemIndex);
    }
}

@Service
public class VentaService {
    
    public BigDecimal obtenerPrecioItem(Long ventaId, int itemIndex) {
        Venta venta = ventaRepository.findById(ventaId).orElseThrow();
        VentaItem item = venta.getItems().get(itemIndex);
        // Encapsula la lógica interna
        return item.getPrecioUnitario();
    }
}
```

---

## Ejemplos de Proyecto

### Ejemplo 1: Módulo de Reportes

**❌ ANTES: Baja cohesión, alto acoplamiento**

```java
// Un solo servicio hace TODO
@Service
public class ReporteService {
    
    private final VentaRepository ventaRepository;
    private final ClienteRepository clienteRepository;
    private final ProductoRepository productoRepository;
    private final EmpleadoRepository empleadoRepository;
    private final JavaMailSender mailSender;
    
    public byte[] generarReporteVentas(LocalDate inicio, LocalDate fin, String formato) {
        List<Venta> ventas = ventaRepository.findByFechaBetween(inicio, fin);
        
        if (formato.equals("PDF")) {
            return generarPDF(ventas);
        } else if (formato.equals("EXCEL")) {
            return generarExcel(ventas);
        } else {
            return generarCSV(ventas);
        }
    }
    
    public byte[] generarReporteClientes(String formato) {
        // Similar pero con clientes
    }
    
    public byte[] generarReporteProductos(String formato) {
        // Similar pero con productos
    }
    
    public void enviarReportePorEmail(String tipo, String email) {
        // Generación + envío mezclados
    }
    
    private byte[] generarPDF(List<?> datos) {
        // 200 líneas de generación PDF
    }
    
    private byte[] generarExcel(List<?> datos) {
        // 150 líneas de generación Excel
    }
    
    private byte[] generarCSV(List<?> datos) {
        // 50 líneas de generación CSV
    }
}
```

**✅ DESPUÉS: Alta cohesión, bajo acoplamiento**

```
com.empresa.reportes/
├── domain/
│   ├── Reporte.java
│   └── TipoReporte.java
│
├── generators/                    ← Alta cohesión: Solo generación
│   ├── ReporteGenerator.java     (interfaz)
│   ├── PDFReporteGenerator.java
│   ├── ExcelReporteGenerator.java
│   └── CSVReporteGenerator.java
│
├── providers/                     ← Alta cohesión: Solo obtención de datos
│   ├── ReporteDataProvider.java  (interfaz)
│   ├── VentasDataProvider.java
│   ├── ClientesDataProvider.java
│   └── ProductosDataProvider.java
│
├── services/
│   ├── ReporteService.java       ← Orquestador (bajo acoplamiento)
│   └── ReporteDistributionService.java
│
└── controllers/
    └── ReporteController.java
```

```java
// 1. Interfaz de generación (bajo acoplamiento)
public interface ReporteGenerator {
    byte[] generar(ReporteData data);
    String getFormato();
}

// 2. Implementaciones cohesivas
@Component
public class PDFReporteGenerator implements ReporteGenerator {
    
    private final PDFRenderer renderer;
    
    @Override
    public byte[] generar(ReporteData data) {
        // Solo generación PDF (alta cohesión)
        Document doc = renderer.createDocument();
        agregarEncabezado(doc, data);
        agregarContenido(doc, data);
        return doc.toByteArray();
    }
    
    @Override
    public String getFormato() {
        return "PDF";
    }
    
    // Métodos privados cohesivos (todos trabajan hacia generar PDF)
    private void agregarEncabezado(Document doc, ReporteData data) { }
    private void agregarContenido(Document doc, ReporteData data) { }
}

// 3. Proveedores de datos cohesivos
@Service
public class VentasDataProvider implements ReporteDataProvider {
    
    private final VentaRepository ventaRepository;
    
    @Override
    public ReporteData obtenerDatos(ReporteRequest request) {
        // Solo obtención de datos de ventas (alta cohesión)
        List<Venta> ventas = ventaRepository.findByFechaBetween(
            request.getFechaInicio(),
            request.getFechaFin()
        );
        return ReporteData.fromVentas(ventas);
    }
}

// 4. Servicio orquestador (bajo acoplamiento)
@Service
public class ReporteService {
    
    // Dependencias de interfaces (bajo acoplamiento)
    private final Map<String, ReporteGenerator> generators;
    private final Map<String, ReporteDataProvider> dataProviders;
    
    public ReporteService(List<ReporteGenerator> generatorList,
                         List<ReporteDataProvider> providerList) {
        this.generators = generatorList.stream()
            .collect(Collectors.toMap(ReporteGenerator::getFormato, g -> g));
        this.dataProviders = providerList.stream()
            .collect(Collectors.toMap(ReporteDataProvider::getTipo, p -> p));
    }
    
    public byte[] generar(ReporteRequest request) {
        // Obtener datos
        ReporteDataProvider provider = dataProviders.get(request.getTipo());
        ReporteData data = provider.obtenerDatos(request);
        
        // Generar reporte
        ReporteGenerator generator = generators.get(request.getFormato());
        return generator.generar(data);
    }
}
```

**Beneficios**:
- ✅ **Alta cohesión**: Cada clase hace una cosa específica
- ✅ **Bajo acoplamiento**: Dependencias vía interfaces
- ✅ Fácil agregar nuevo formato (nueva clase `JsonReporteGenerator`)
- ✅ Fácil agregar nuevo tipo de reporte (nueva clase `EmpleadosDataProvider`)
- ✅ Testeable (mocks de interfaces)

### Ejemplo 2: Cálculo de Precio de Venta

**❌ ANTES: Bajo cohesión en calculador monolítico**

```java
@Service
public class PrecioCalculator {
    
    public BigDecimal calcularPrecioFinal(Venta venta) {
        BigDecimal subtotal = BigDecimal.ZERO;
        
        // Calcular subtotal
        for (VentaItem item : venta.getItems()) {
            subtotal = subtotal.add(
                item.getPrecioUnitario().multiply(new BigDecimal(item.getCantidad()))
            );
        }
        
        // Aplicar descuento por cantidad
        if (venta.getItems().size() > 10) {
            subtotal = subtotal.multiply(new BigDecimal("0.95")); // 5% descuento
        }
        
        // Aplicar descuento por cliente frecuente
        if (venta.getCliente().getComprasAnuales() > 100) {
            subtotal = subtotal.multiply(new BigDecimal("0.90")); // 10% descuento
        }
        
        // Aplicar impuestos
        BigDecimal iva = subtotal.multiply(new BigDecimal("0.21"));
        
        // Aplicar cargos adicionales
        BigDecimal cargoServicio = BigDecimal.ZERO;
        if (venta.getTipoEntrega().equals("DOMICILIO")) {
            cargoServicio = new BigDecimal("200.00");
        }
        
        return subtotal.add(iva).add(cargoServicio);
    }
}
```

**✅ DESPUÉS: Alta cohesión con Strategy Pattern**

```java
// 1. Calculadores cohesivos específicos
@Component
public class SubtotalCalculator {
    
    public BigDecimal calcular(List<VentaItem> items) {
        return items.stream()
            .map(item -> item.getPrecioUnitario()
                .multiply(new BigDecimal(item.getCantidad())))
            .reduce(BigDecimal.ZERO, BigDecimal::add);
    }
}

@Component
public class ImpuestoCalculator {
    
    private static final BigDecimal IVA = new BigDecimal("0.21");
    
    public BigDecimal calcular(BigDecimal base) {
        return base.multiply(IVA);
    }
}

// 2. Estrategias de descuento cohesivas
public interface DescuentoStrategy {
    BigDecimal aplicar(Venta venta, BigDecimal subtotal);
    boolean aplica(Venta venta);
}

@Component
public class DescuentoPorCantidadStrategy implements DescuentoStrategy {
    
    @Override
    public BigDecimal aplicar(Venta venta, BigDecimal subtotal) {
        if (venta.getItems().size() > 10) {
            BigDecimal descuento = subtotal.multiply(new BigDecimal("0.05"));
            return subtotal.subtract(descuento);
        }
        return subtotal;
    }
    
    @Override
    public boolean aplica(Venta venta) {
        return venta.getItems().size() > 10;
    }
}

@Component
public class DescuentoClienteFrecuenteStrategy implements DescuentoStrategy {
    
    @Override
    public BigDecimal aplicar(Venta venta, BigDecimal subtotal) {
        if (venta.getCliente().getComprasAnuales() > 100) {
            BigDecimal descuento = subtotal.multiply(new BigDecimal("0.10"));
            return subtotal.subtract(descuento);
        }
        return subtotal;
    }
    
    @Override
    public boolean aplica(Venta venta) {
        return venta.getCliente() != null && 
               venta.getCliente().getComprasAnuales() > 100;
    }
}

// 3. Orquestador con bajo acoplamiento
@Service
public class PrecioCalculator {
    
    private final SubtotalCalculator subtotalCalculator;
    private final ImpuestoCalculator impuestoCalculator;
    private final List<DescuentoStrategy> descuentos;
    private final CargoAdicionalCalculator cargoCalculator;
    
    public BigDecimal calcularPrecioFinal(Venta venta) {
        // Calcular subtotal
        BigDecimal subtotal = subtotalCalculator.calcular(venta.getItems());
        
        // Aplicar descuentos aplicables
        for (DescuentoStrategy descuento : descuentos) {
            if (descuento.aplica(venta)) {
                subtotal = descuento.aplicar(venta, subtotal);
            }
        }
        
        // Aplicar impuestos
        BigDecimal impuesto = impuestoCalculator.calcular(subtotal);
        
        // Aplicar cargos
        BigDecimal cargo = cargoCalculator.calcular(venta);
        
        return subtotal.add(impuesto).add(cargo);
    }
}
```

**Beneficios**:
- ✅ Cada calculador tiene alta cohesión (una responsabilidad clara)
- ✅ Bajo acoplamiento (estrategias independientes)
- ✅ Fácil agregar nuevo descuento (nueva clase Strategy)
- ✅ Testeable (cada estrategia por separado)

---

## Referencias

### Libros
- **Object-Oriented Software Construction** - Bertrand Meyer (acuñó "cohesión" y "acoplamiento")
- **Structured Design** - Edward Yourdon & Larry Constantine (definieron niveles)
- **Clean Architecture** - Robert C. Martin
- **Domain-Driven Design** - Eric Evans (organización por dominio)


### Documentación Interna
- [SOLID Principles](solid-principles.md)
- [Design Patterns](../design_patterns/README.md)
- [Architecture ADRs](../productos/backoffice/docs/08-arquitectura/adr/README.md)

---

**Última actualización**: 2026-01-08  
**Versión**: 1.0.0  
**Autor**: @team-architecture  
**Revisores**: @team-backend, @team-docs
