# Principios SOLID

Los principios SOLID son cinco principios fundamentales de diseño orientado a objetos que ayudan a crear software más mantenible, escalable y flexible.

## Tabla de Contenidos

- [¿Qué es SOLID?](#qué-es-solid)
- [S - Single Responsibility Principle](#s---single-responsibility-principle-srp)
- [O - Open/Closed Principle](#o---openclosed-principle-ocp)
- [L - Liskov Substitution Principle](#l---liskov-substitution-principle-lsp)
- [I - Interface Segregation Principle](#i---interface-segregation-principle-isp)
- [D - Dependency Inversion Principle](#d---dependency-inversion-principle-dip)
- [Resumen Comparativo](#resumen-comparativo)
- [Referencias](#referencias)

---

## ¿Qué es SOLID?

**SOLID** es un acrónimo que agrupa cinco principios de diseño orientado a objetos introducidos por **Robert C. Martin (Uncle Bob)**:

| Letra | Principio | Concepto Clave |
|-------|-----------|----------------|
| **S** | Single Responsibility | Una clase, un motivo para cambiar |
| **O** | Open/Closed | Abierto para extensión, cerrado para modificación |
| **L** | Liskov Substitution | Las subclases deben ser sustituibles por sus clases base |
| **I** | Interface Segregation | Interfaces pequeñas y específicas |
| **D** | Dependency Inversion | Depender de abstracciones, no de implementaciones |

### Beneficios de Aplicar SOLID

✅ **Mantenibilidad**: Código más fácil de entender y modificar  
✅ **Escalabilidad**: Facilita agregar nuevas funcionalidades  
✅ **Testabilidad**: Componentes más fáciles de testear en aislamiento  
✅ **Reducción de bugs**: Cambios localizados reducen efectos secundarios  
✅ **Reutilización**: Componentes desacoplados son más reutilizables  

---

## S - Single Responsibility Principle (SRP)

> "Una clase debe tener una, y solo una, razón para cambiar"  
> — Robert C. Martin

### Concepto

Cada clase debe tener **una única responsabilidad** o motivo de cambio. Si una clase hace demasiadas cosas, los cambios en una responsabilidad pueden afectar otras funcionalidades.

### ❌ Problema: Clase con Múltiples Responsabilidades

```java
// VIOLA SRP: Maneja lógica de negocio, persistencia Y reportes
public class EmpleadoService {
    
    public void crearEmpleado(Empleado empleado) {
        // Validación de negocio
        if (empleado.getSalario() < 0) {
            throw new IllegalArgumentException("Salario inválido");
        }
        
        // Persistencia en BD (segunda responsabilidad)
        Connection conn = DriverManager.getConnection("jdbc:...");
        PreparedStatement stmt = conn.prepareStatement(
            "INSERT INTO empleado (nombre, salario) VALUES (?, ?)"
        );
        stmt.setString(1, empleado.getNombre());
        stmt.setDouble(2, empleado.getSalario());
        stmt.executeUpdate();
        
        // Envío de email (tercera responsabilidad)
        EmailService emailService = new EmailService();
        emailService.enviarBienvenida(empleado.getEmail());
        
        // Generación de reporte (cuarta responsabilidad)
        generarReporteAlta(empleado);
    }
    
    private void generarReporteAlta(Empleado empleado) {
        PDFGenerator pdf = new PDFGenerator();
        pdf.addText("Nuevo empleado: " + empleado.getNombre());
        pdf.save("reporte_" + empleado.getId() + ".pdf");
    }
}
```

**Problemas**:
- 🔴 Cambios en BD afectan la clase completa
- 🔴 Cambios en formato de email afectan la clase
- 🔴 Cambios en generación de reportes afectan la clase
- 🔴 Difícil de testear (requiere BD real, servidor de email, etc.)
- 🔴 Clase con 200+ líneas, difícil de mantener

### ✅ Solución: Separación de Responsabilidades

```java
// 1. Servicio de dominio - Solo lógica de negocio
@Service
public class EmpleadoService {
    
    private final EmpleadoRepository repository;
    private final NotificacionService notificacionService;
    private final ReporteService reporteService;
    
    public EmpleadoService(EmpleadoRepository repository,
                          NotificacionService notificacionService,
                          ReporteService reporteService) {
        this.repository = repository;
        this.notificacionService = notificacionService;
        this.reporteService = reporteService;
    }
    
    public Empleado crearEmpleado(EmpleadoDTO dto) {
        // Solo validación de negocio
        validarDatosEmpleado(dto);
        
        Empleado empleado = new Empleado(dto);
        
        // Delegar persistencia
        Empleado saved = repository.save(empleado);
        
        // Delegar notificaciones
        notificacionService.enviarBienvenida(saved);
        
        // Delegar reportería
        reporteService.generarReporteAlta(saved);
        
        return saved;
    }
    
    private void validarDatosEmpleado(EmpleadoDTO dto) {
        if (dto.getSalario() < 0) {
            throw new IllegalArgumentException("Salario inválido");
        }
        // Más validaciones de negocio...
    }
}

// 2. Repositorio - Solo persistencia
@Repository
public interface EmpleadoRepository extends JpaRepository<Empleado, Long> {
    Optional<Empleado> findByDocumentoIdentidad(String documento);
}

// 3. Servicio de notificaciones - Solo envío de emails
@Service
public class NotificacionService {
    
    private final JavaMailSender mailSender;
    
    public void enviarBienvenida(Empleado empleado) {
        SimpleMailMessage message = new SimpleMailMessage();
        message.setTo(empleado.getEmail());
        message.setSubject("Bienvenido");
        message.setText("Hola " + empleado.getNombre() + "...");
        mailSender.send(message);
    }
}

// 4. Servicio de reportes - Solo generación de documentos
@Service
public class ReporteService {
    
    private final PDFGenerator pdfGenerator;
    
    public void generarReporteAlta(Empleado empleado) {
        Document doc = pdfGenerator.createDocument();
        doc.addHeading("Alta de Empleado");
        doc.addText("Nombre: " + empleado.getNombre());
        doc.addText("Salario: " + empleado.getSalario());
        doc.save("reportes/alta_" + empleado.getId() + ".pdf");
    }
}
```

**Beneficios**:
- ✅ Cada clase tiene un único motivo para cambiar
- ✅ Fácil de testear (mocks de dependencias)
- ✅ Cambios en BD solo afectan al repositorio
- ✅ Cambios en emails solo afectan a NotificacionService
- ✅ Clases pequeñas y enfocadas (< 100 líneas)

### Ejemplo: VentaService

```java
// ❌ ANTES: VentaService hace TODO
public class VentaService {
    public void procesarVenta(Venta venta) {
        // Validar stock, calcular totales, aplicar descuentos,
        // guardar en BD, generar factura, enviar email,
        // actualizar inventario, registrar en contabilidad...
        // 500+ líneas de código
    }
}

// ✅ DESPUÉS: Responsabilidades separadas
@Service
public class VentaService {
    private final VentaRepository ventaRepository;
    private final StockService stockService;
    private final PrecioCalculator precioCalculator;
    private final DescuentoService descuentoService;
    private final FacturacionService facturacionService;
    private final NotificacionService notificacionService;
    private final ContabilidadService contabilidadService;
    
    public Venta procesarVenta(VentaDTO dto) {
        // 1. Validar stock
        stockService.validarDisponibilidad(dto.getItems());
        
        // 2. Calcular precios
        BigDecimal subtotal = precioCalculator.calcular(dto.getItems());
        BigDecimal descuento = descuentoService.aplicar(dto.getCliente(), subtotal);
        
        // 3. Crear venta
        Venta venta = new Venta(dto, subtotal, descuento);
        Venta saved = ventaRepository.save(venta);
        
        // 4. Actualizar stock
        stockService.descontar(dto.getItems());
        
        // 5. Generar factura
        Factura factura = facturacionService.generar(saved);
        
        // 6. Notificar cliente
        notificacionService.enviarComprobante(saved, factura);
        
        // 7. Registrar contabilidad
        contabilidadService.registrarIngreso(saved);
        
        return saved;
    }
}
```

---

## O - Open/Closed Principle (OCP)

> "Las entidades de software deben estar abiertas para extensión, pero cerradas para modificación"  
> — Bertrand Meyer

### Concepto

Debes poder **agregar nuevas funcionalidades sin modificar el código existente**. Usa abstracción (interfaces, clases abstractas) para permitir extensión.

### ❌ Problema: Modificar Código Existente

```java
// VIOLA OCP: Cada nuevo tipo de reporte requiere modificar la clase
public class ReporteGenerator {
    
    public void generarReporte(String tipo, List<Venta> ventas) {
        if (tipo.equals("PDF")) {
            // Generar PDF
            PDFDocument pdf = new PDFDocument();
            for (Venta venta : ventas) {
                pdf.addLine(venta.toString());
            }
            pdf.save("reporte.pdf");
            
        } else if (tipo.equals("EXCEL")) {
            // Generar Excel
            Workbook workbook = new XSSFWorkbook();
            Sheet sheet = workbook.createSheet("Ventas");
            // ... código Excel
            
        } else if (tipo.equals("CSV")) {
            // Generar CSV
            FileWriter writer = new FileWriter("reporte.csv");
            for (Venta venta : ventas) {
                writer.write(venta.toCsv());
            }
            writer.close();
            
        } else if (tipo.equals("JSON")) {
            // NUEVA FUNCIONALIDAD: ¡Requiere modificar esta clase!
            ObjectMapper mapper = new ObjectMapper();
            String json = mapper.writeValueAsString(ventas);
            // ...
        }
    }
}
```

**Problemas**:
- 🔴 Cada nuevo formato requiere modificar `generarReporte()`
- 🔴 Método con 100+ líneas y muchos `if-else`
- 🔴 Riesgo de romper funcionalidad existente al agregar nueva
- 🔴 Difícil de testear (todos los formatos en un método)

### ✅ Solución: Usar Abstracción para Extensión

```java
// 1. Interfaz que define el contrato
public interface ReporteGenerator {
    void generar(List<Venta> ventas, OutputStream output);
    String getFormato();
}

// 2. Implementaciones concretas (extensión sin modificación)
@Component
public class PDFReporteGenerator implements ReporteGenerator {
    
    @Override
    public void generar(List<Venta> ventas, OutputStream output) {
        PDFDocument pdf = new PDFDocument();
        pdf.addHeading("Reporte de Ventas");
        
        for (Venta venta : ventas) {
            pdf.addTable(venta.getItems());
        }
        
        pdf.writeTo(output);
    }
    
    @Override
    public String getFormato() {
        return "PDF";
    }
}

@Component
public class ExcelReporteGenerator implements ReporteGenerator {
    
    @Override
    public void generar(List<Venta> ventas, OutputStream output) {
        Workbook workbook = new XSSFWorkbook();
        Sheet sheet = workbook.createSheet("Ventas");
        
        int rowNum = 0;
        for (Venta venta : ventas) {
            Row row = sheet.createRow(rowNum++);
            row.createCell(0).setCellValue(venta.getId());
            row.createCell(1).setCellValue(venta.getTotal().doubleValue());
        }
        
        workbook.write(output);
    }
    
    @Override
    public String getFormato() {
        return "EXCEL";
    }
}

// 3. NUEVA FUNCIONALIDAD: Solo crear nueva clase (sin modificar existentes)
@Component
public class JsonReporteGenerator implements ReporteGenerator {
    
    private final ObjectMapper objectMapper;
    
    @Override
    public void generar(List<Venta> ventas, OutputStream output) {
        objectMapper.writeValue(output, ventas);
    }
    
    @Override
    public String getFormato() {
        return "JSON";
    }
}

// 4. Servicio que usa los generadores (Factory Pattern)
@Service
public class ReporteService {
    
    private final Map<String, ReporteGenerator> generators;
    
    // Spring inyecta automáticamente todos los beans de tipo ReporteGenerator
    public ReporteService(List<ReporteGenerator> generatorList) {
        this.generators = generatorList.stream()
            .collect(Collectors.toMap(
                ReporteGenerator::getFormato,
                Function.identity()
            ));
    }
    
    public void generarReporte(String formato, List<Venta> ventas, OutputStream output) {
        ReporteGenerator generator = generators.get(formato.toUpperCase());
        
        if (generator == null) {
            throw new IllegalArgumentException("Formato no soportado: " + formato);
        }
        
        generator.generar(ventas, output);
    }
    
    public Set<String> getFormatosSoportados() {
        return generators.keySet();
    }
}
```

**Beneficios**:
- ✅ Agregar nuevo formato = crear nueva clase (sin tocar existentes)
- ✅ Código existente cerrado para modificación
- ✅ Sistema abierto para extensión
- ✅ Cada implementación es pequeña y enfocada
- ✅ Fácil de testear (cada generador por separado)
- ✅ Spring detecta automáticamente nuevos generadores

### Ejemplo: DescuentoStrategy

```java
// Descuentos extensibles sin modificar código existente
public interface DescuentoStrategy {
    BigDecimal aplicar(Venta venta);
    boolean aplica(Venta venta);
}

@Component
public class DescuentoPorcentajeStrategy implements DescuentoStrategy {
    public BigDecimal aplicar(Venta venta) {
        return venta.getSubtotal().multiply(venta.getDescuentoPorcentaje());
    }
    
    public boolean aplica(Venta venta) {
        return venta.getDescuentoPorcentaje() != null;
    }
}

@Component
public class DescuentoClienteFrecuenteStrategy implements DescuentoStrategy {
    public BigDecimal aplicar(Venta venta) {
        if (venta.getCliente().getComprasAnuales() > 100) {
            return venta.getSubtotal().multiply(new BigDecimal("0.15"));
        }
        return BigDecimal.ZERO;
    }
    
    public boolean aplica(Venta venta) {
        return venta.getCliente() != null && 
               venta.getCliente().getComprasAnuales() > 50;
    }
}

// NUEVA ESTRATEGIA: Solo crear clase, sin modificar existentes
@Component
public class DescuentoBlackFridayStrategy implements DescuentoStrategy {
    public BigDecimal aplicar(Venta venta) {
        LocalDate hoy = LocalDate.now();
        if (hoy.getMonthValue() == 11 && hoy.getDayOfMonth() >= 20) {
            return venta.getSubtotal().multiply(new BigDecimal("0.30"));
        }
        return BigDecimal.ZERO;
    }
    
    public boolean aplica(Venta venta) {
        return venta.getFecha().getMonth() == Month.NOVEMBER;
    }
}
```

---

## L - Liskov Substitution Principle (LSP)

> "Los objetos de un programa deberían ser reemplazables por instancias de sus subtipos sin alterar el correcto funcionamiento del programa"  
> — Barbara Liskov

### Concepto

Una **subclase debe poder sustituir a su clase base** sin que el código cliente se rompa o cambie su comportamiento. Las subclases deben **respetar el contrato** de la clase base.

### ❌ Problema: Subclase que Rompe el Contrato

```java
// VIOLA LSP: Pinguino rompe el contrato de Ave
public abstract class Ave {
    protected String nombre;
    
    public abstract void volar();
    
    public void comer() {
        System.out.println(nombre + " está comiendo");
    }
}

public class Aguila extends Ave {
    public Aguila(String nombre) {
        this.nombre = nombre;
    }
    
    @Override
    public void volar() {
        System.out.println(nombre + " vuela alto en el cielo");
    }
}

public class Pinguino extends Ave {
    public Pinguino(String nombre) {
        this.nombre = nombre;
    }
    
    @Override
    public void volar() {
        // ¡PROBLEMA! Los pingüinos NO vuelan
        throw new UnsupportedOperationException("Los pingüinos no pueden volar");
    }
}

// Cliente que usa Ave
public class ZoologicoService {
    
    public void ejercitarAves(List<Ave> aves) {
        for (Ave ave : aves) {
            ave.volar(); // ¡BOOM! Explota con Pinguino
        }
    }
}

// Uso
ZoologicoService zoo = new ZoologicoService();
List<Ave> aves = Arrays.asList(
    new Aguila("Águila Real"),
    new Pinguino("Pingüino Emperador") // ¡Causa excepción!
);
zoo.ejercitarAves(aves); // UnsupportedOperationException
```

**Problemas**:
- 🔴 `Pinguino` no puede sustituir a `Ave` sin romper el código
- 🔴 El cliente debe conocer el tipo concreto (violación de OCP también)
- 🔴 Lanza excepciones inesperadas
- 🔴 Diseño jerárquico incorrecto

### ✅ Solución: Diseño de Jerarquía Correcta

```java
// 1. Abstraer solo lo común a TODAS las aves
public abstract class Ave {
    protected String nombre;
    
    public Ave(String nombre) {
        this.nombre = nombre;
    }
    
    public void comer() {
        System.out.println(nombre + " está comiendo");
    }
    
    public abstract void moverse();
}

// 2. Interfaz para capacidades específicas
public interface Volador {
    void volar();
    int getAlturaMaximaVuelo();
}

public interface Nadador {
    void nadar();
    int getProfundidadMaximaNado();
}

// 3. Implementaciones específicas
public class Aguila extends Ave implements Volador {
    
    public Aguila(String nombre) {
        super(nombre);
    }
    
    @Override
    public void moverse() {
        volar();
    }
    
    @Override
    public void volar() {
        System.out.println(nombre + " vuela alto en el cielo");
    }
    
    @Override
    public int getAlturaMaximaVuelo() {
        return 3000; // metros
    }
}

public class Pinguino extends Ave implements Nadador {
    
    public Pinguino(String nombre) {
        super(nombre);
    }
    
    @Override
    public void moverse() {
        nadar();
    }
    
    @Override
    public void nadar() {
        System.out.println(nombre + " nada velozmente bajo el agua");
    }
    
    @Override
    public int getProfundidadMaximaNado() {
        return 250; // metros
    }
}

public class Pato extends Ave implements Volador, Nadador {
    
    public Pato(String nombre) {
        super(nombre);
    }
    
    @Override
    public void moverse() {
        System.out.println(nombre + " puede volar y nadar");
    }
    
    @Override
    public void volar() {
        System.out.println(nombre + " vuela a baja altura");
    }
    
    @Override
    public int getAlturaMaximaVuelo() {
        return 500;
    }
    
    @Override
    public void nadar() {
        System.out.println(nombre + " nada en la superficie");
    }
    
    @Override
    public int getProfundidadMaximaNado() {
        return 5;
    }
}

// 4. Cliente correcto
public class ZoologicoService {
    
    public void ejercitarAves(List<Ave> aves) {
        for (Ave ave : aves) {
            ave.moverse(); // ✅ Funciona para todas las aves
            ave.comer();   // ✅ Funciona para todas las aves
        }
    }
    
    public void entrenamientoVuelo(List<Volador> voladores) {
        for (Volador volador : voladores) {
            volador.volar(); // ✅ Solo aves que vuelan
        }
    }
    
    public void entrenamientoNatacion(List<Nadador> nadadores) {
        for (Nadador nadador : nadadores) {
            nadador.nadar(); // ✅ Solo aves que nadan
        }
    }
}
```

**Beneficios**:
- ✅ Todas las subclases de `Ave` pueden sustituirla correctamente
- ✅ Interfaces segregadas según capacidades específicas
- ✅ No hay excepciones inesperadas
- ✅ Cliente solo depende de abstracciones necesarias

### Ejemplo: UsuarioService

```java
// ❌ ANTES: Rompe LSP
public class Usuario {
    public void cambiarContraseña(String nuevaContraseña) {
        // Cambio de contraseña estándar
    }
}

public class UsuarioOAuth extends Usuario {
    @Override
    public void cambiarContraseña(String nuevaContraseña) {
        // ¡PROBLEMA! OAuth no maneja contraseñas localmente
        throw new UnsupportedOperationException("OAuth no soporta cambio de contraseña");
    }
}

// ✅ DESPUÉS: Respeta LSP
public abstract class Usuario {
    protected String email;
    protected String nombre;
    
    public abstract void autenticar(String credencial);
    public abstract boolean puedeModificarContraseña();
}

public interface CambioContraseñaCapable {
    void cambiarContraseña(String actual, String nueva);
}

public class UsuarioLocal extends Usuario implements CambioContraseñaCapable {
    
    @Override
    public void autenticar(String credencial) {
        // Autenticación con password hash
    }
    
    @Override
    public boolean puedeModificarContraseña() {
        return true;
    }
    
    @Override
    public void cambiarContraseña(String actual, String nueva) {
        // Validar contraseña actual
        // Hashear nueva contraseña
        // Guardar en BD
    }
}

public class UsuarioOAuth extends Usuario {
    
    @Override
    public void autenticar(String credencial) {
        // Autenticación con token OAuth
    }
    
    @Override
    public boolean puedeModificarContraseña() {
        return false; // ✅ Indica capacidad claramente
    }
}

// Servicio que respeta LSP
@Service
public class UsuarioService {
    
    public void procesarCambioContraseña(Usuario usuario, String actual, String nueva) {
        if (!usuario.puedeModificarContraseña()) {
            throw new BusinessException("Usuario no soporta cambio de contraseña");
        }
        
        if (usuario instanceof CambioContraseñaCapable) {
            ((CambioContraseñaCapable) usuario).cambiarContraseña(actual, nueva);
        }
    }
}
```

---

## I - Interface Segregation Principle (ISP)

> "Los clientes no deberían estar forzados a depender de interfaces que no usan"  
> — Robert C. Martin

### Concepto

Es mejor tener **muchas interfaces pequeñas y específicas** que una interfaz grande y general. Los clientes solo deben conocer los métodos que realmente necesitan.

### ❌ Problema: Interfaz Demasiado Grande

```java
// VIOLA ISP: Interfaz "fat" con métodos que no todos los clientes necesitan
public interface Trabajador {
    void trabajar();
    void comer();
    void dormir();
    void cobrarSalario();
    void tomarDescanso();
    void recibirBeneficios();
    void asistirReunion();
    void reportarActividades();
}

// Implementación forzada a definir métodos irrelevantes
public class Robot implements Trabajador {
    
    @Override
    public void trabajar() {
        System.out.println("Robot trabajando 24/7");
    }
    
    @Override
    public void comer() {
        // ❌ Los robots NO comen
        throw new UnsupportedOperationException("Robot no come");
    }
    
    @Override
    public void dormir() {
        // ❌ Los robots NO duermen
        throw new UnsupportedOperationException("Robot no duerme");
    }
    
    @Override
    public void cobrarSalario() {
        // ❌ Los robots NO cobran salario
        throw new UnsupportedOperationException("Robot no cobra salario");
    }
    
    @Override
    public void tomarDescanso() {
        // ❌ Solo mantenimiento
        throw new UnsupportedOperationException("Robot no toma descansos");
    }
    
    @Override
    public void recibirBeneficios() {
        throw new UnsupportedOperationException();
    }
    
    @Override
    public void asistirReunion() {
        throw new UnsupportedOperationException();
    }
    
    @Override
    public void reportarActividades() {
        System.out.println("Enviando métricas de rendimiento");
    }
}

public class Empleado implements Trabajador {
    // Debe implementar TODOS los métodos aunque algunos no apliquen
    // para ciertos tipos de empleados (pasantes, freelancers, etc.)
}
```

**Problemas**:
- 🔴 Clases forzadas a implementar métodos irrelevantes
- 🔴 Muchas excepciones `UnsupportedOperationException`
- 🔴 Interfaz difícil de mantener (cambios afectan a todos)
- 🔴 Clientes acoplados a métodos que no usan

### ✅ Solución: Interfaces Segregadas

```java
// 1. Interfaces pequeñas y cohesivas
public interface Trabajador {
    void trabajar();
    void reportarActividades();
}

public interface NecesidadesBasicas {
    void comer();
    void dormir();
}

public interface EmpleadoRemunerado {
    void cobrarSalario();
    BigDecimal getSalarioMensual();
}

public interface TrabajadorConDescansos {
    void tomarDescanso();
    int getMinutosDescanso();
}

public interface Beneficiado {
    void recibirBeneficios();
    List<Beneficio> getBeneficios();
}

public interface ParticipanteReunion {
    void asistirReunion(Reunion reunion);
}

// 2. Implementaciones específicas
public class Robot implements Trabajador {
    
    @Override
    public void trabajar() {
        System.out.println("Robot trabajando 24/7");
    }
    
    @Override
    public void reportarActividades() {
        System.out.println("Enviando métricas de rendimiento");
    }
    
    // ✅ No necesita implementar métodos irrelevantes
}

public class EmpleadoTiempoCompleto implements 
    Trabajador, 
    NecesidadesBasicas, 
    EmpleadoRemunerado, 
    TrabajadorConDescansos,
    Beneficiado,
    ParticipanteReunion {
    
    @Override
    public void trabajar() {
        System.out.println("Empleado trabajando");
    }
    
    @Override
    public void reportarActividades() {
        System.out.println("Reporte de actividades del día");
    }
    
    @Override
    public void comer() {
        System.out.println("Hora de almuerzo");
    }
    
    @Override
    public void dormir() {
        System.out.println("Fin de jornada");
    }
    
    @Override
    public void cobrarSalario() {
        System.out.println("Procesando pago mensual");
    }
    
    @Override
    public BigDecimal getSalarioMensual() {
        return new BigDecimal("3000.00");
    }
    
    @Override
    public void tomarDescanso() {
        System.out.println("Descanso de 15 minutos");
    }
    
    @Override
    public int getMinutosDescanso() {
        return 30;
    }
    
    @Override
    public void recibirBeneficios() {
        System.out.println("Seguro médico, vacaciones, aguinaldo");
    }
    
    @Override
    public List<Beneficio> getBeneficios() {
        return Arrays.asList(
            new Beneficio("Seguro Médico"),
            new Beneficio("Vacaciones 20 días")
        );
    }
    
    @Override
    public void asistirReunion(Reunion reunion) {
        System.out.println("Asistiendo a: " + reunion.getTema());
    }
}

public class Freelancer implements 
    Trabajador, 
    NecesidadesBasicas {
    
    // ✅ Solo implementa interfaces relevantes
    // No necesita EmpleadoRemunerado (cobra por proyecto)
    // No necesita Beneficiado (sin beneficios)
    // No necesita ParticipanteReunion (trabajo remoto autónomo)
    
    @Override
    public void trabajar() {
        System.out.println("Freelancer trabajando en proyecto");
    }
    
    @Override
    public void reportarActividades() {
        System.out.println("Reporte semanal de horas");
    }
    
    @Override
    public void comer() {
        System.out.println("Almuerzo flexible");
    }
    
    @Override
    public void dormir() {
        System.out.println("Horario flexible");
    }
}

// 3. Servicios que usan interfaces específicas
@Service
public class NominaService {
    
    // ✅ Solo depende de EmpleadoRemunerado
    public void procesarNomina(List<EmpleadoRemunerado> empleados) {
        for (EmpleadoRemunerado empleado : empleados) {
            empleado.cobrarSalario();
            // No necesita saber si come, duerme, asiste a reuniones, etc.
        }
    }
}

@Service
public class ReunionService {
    
    // ✅ Solo depende de ParticipanteReunion
    public void convocarReunion(Reunion reunion, List<ParticipanteReunion> participantes) {
        for (ParticipanteReunion participante : participantes) {
            participante.asistirReunion(reunion);
        }
    }
}

@Service
public class RecursosHumanosService {
    
    // ✅ Solo depende de Beneficiado
    public void distribuirBeneficios(List<Beneficiado> beneficiados) {
        for (Beneficiado beneficiado : beneficiados) {
            beneficiado.recibirBeneficios();
        }
    }
}
```

**Beneficios**:
- ✅ Clases solo implementan métodos relevantes
- ✅ No hay métodos "dummy" ni excepciones `UnsupportedOperationException`
- ✅ Servicios dependen solo de interfaces específicas
- ✅ Fácil agregar nuevos tipos de trabajadores
- ✅ Cambios en una interfaz no afectan a clientes que no la usan

### Ejemplo: ReporteService

```java
// ❌ ANTES: Interfaz grande
public interface ReporteService {
    byte[] generarPDF(ReporteRequest request);
    byte[] generarExcel(ReporteRequest request);
    byte[] generarCSV(ReporteRequest request);
    void enviarPorEmail(ReporteRequest request, String email);
    void imprimirDirectamente(ReporteRequest request, String impresora);
    void guardarEnServidor(ReporteRequest request, String ruta);
    List<Reporte> listarHistorial();
    void programarReporte(ReporteRequest request, CronExpression cron);
}

// Clientes forzados a conocer TODO
public class VentaController {
    private final ReporteService reporteService;
    
    public ResponseEntity<?> descargarReportePDF() {
        // Solo necesito generarPDF, pero debo inyectar toda la interfaz
        byte[] pdf = reporteService.generarPDF(request);
        return ResponseEntity.ok(pdf);
    }
}

// ✅ DESPUÉS: Interfaces segregadas
public interface ReporteGenerator {
    byte[] generar(ReporteRequest request);
}

public interface ReporteDistributor {
    void enviarPorEmail(byte[] contenido, String email);
    void guardarEnServidor(byte[] contenido, String ruta);
}

public interface ReportePrinter {
    void imprimir(byte[] contenido, String impresora);
}

public interface ReporteScheduler {
    void programar(ReporteRequest request, CronExpression cron);
    void cancelar(String reporteId);
}

public interface ReporteHistorial {
    List<Reporte> listar(FiltroReporte filtro);
    Optional<Reporte> obtener(String id);
}

// Implementaciones específicas
@Service
public class PDFReporteGenerator implements ReporteGenerator {
    public byte[] generar(ReporteRequest request) {
        // Generación PDF
    }
}

@Service
public class EmailReporteDistributor implements ReporteDistributor {
    public void enviarPorEmail(byte[] contenido, String email) {
        // Envío por email
    }
    
    public void guardarEnServidor(byte[] contenido, String ruta) {
        // Guardado en servidor
    }
}

// Clientes específicos
@RestController
public class VentaController {
    private final ReporteGenerator pdfGenerator; // ✅ Solo lo que necesita
    
    public ResponseEntity<?> descargarReportePDF() {
        byte[] pdf = pdfGenerator.generar(request);
        return ResponseEntity.ok(pdf);
    }
}

@Service
public class ReporteAutomaticoService {
    private final ReporteScheduler scheduler; // ✅ Solo lo que necesita
    
    public void programarReporteMensual() {
        scheduler.programar(request, CronExpression.parse("0 0 1 * *"));
    }
}
```

---

## D - Dependency Inversion Principle (DIP)

> "Los módulos de alto nivel no deben depender de módulos de bajo nivel. Ambos deben depender de abstracciones"  
> "Las abstracciones no deben depender de los detalles. Los detalles deben depender de las abstracciones"  
> — Robert C. Martin

### Concepto

- **Módulos de alto nivel** (lógica de negocio) no deben depender de **módulos de bajo nivel** (implementaciones concretas)
- Ambos deben depender de **abstracciones** (interfaces)
- Facilita cambiar implementaciones sin afectar la lógica de negocio

### ❌ Problema: Dependencia de Implementaciones Concretas

```java
// VIOLA DIP: VentaService depende directamente de clases concretas
public class VentaService {
    
    // ❌ Dependencias concretas (bajo nivel)
    private MySQLVentaRepository ventaRepository;
    private SmtpEmailService emailService;
    private Log4jLogger logger;
    
    public VentaService() {
        // ❌ Creación directa de dependencias
        this.ventaRepository = new MySQLVentaRepository();
        this.emailService = new SmtpEmailService();
        this.logger = new Log4jLogger();
    }
    
    public Venta procesarVenta(VentaDTO dto) {
        logger.info("Procesando venta");
        
        // Lógica de negocio
        Venta venta = new Venta(dto);
        
        // ❌ Acoplado a implementación MySQL
        ventaRepository.guardar(venta);
        
        // ❌ Acoplado a SMTP
        emailService.enviarComprobante(venta);
        
        logger.info("Venta procesada: " + venta.getId());
        
        return venta;
    }
}
```

**Problemas**:
- 🔴 Imposible cambiar de MySQL a PostgreSQL sin modificar `VentaService`
- 🔴 Imposible cambiar de SMTP a SendGrid sin modificar `VentaService`
- 🔴 Imposible testear (requiere BD real, servidor SMTP real)
- 🔴 Violación de OCP (cerrado para modificación)
- 🔴 Alto acoplamiento

### ✅ Solución: Inversión de Dependencias

```java
// 1. Definir abstracciones (interfaces)
public interface VentaRepository {
    Venta save(Venta venta);
    Optional<Venta> findById(Long id);
    List<Venta> findByFecha(LocalDate fecha);
}

public interface EmailService {
    void enviarComprobante(Venta venta);
    void enviarNotificacion(String destinatario, String mensaje);
}

public interface Logger {
    void info(String mensaje);
    void error(String mensaje, Throwable ex);
}

// 2. Módulo de alto nivel depende de abstracciones
@Service
public class VentaService {
    
    // ✅ Dependencias de abstracciones (interfaces)
    private final VentaRepository ventaRepository;
    private final EmailService emailService;
    private final Logger logger;
    
    // ✅ Inyección de dependencias (Spring lo resuelve)
    @Autowired
    public VentaService(VentaRepository ventaRepository,
                       EmailService emailService,
                       Logger logger) {
        this.ventaRepository = ventaRepository;
        this.emailService = emailService;
        this.logger = logger;
    }
    
    public Venta procesarVenta(VentaDTO dto) {
        logger.info("Procesando venta");
        
        // Lógica de negocio
        Venta venta = new Venta(dto);
        
        // ✅ Usa abstracción, no implementación concreta
        Venta saved = ventaRepository.save(venta);
        
        // ✅ Usa abstracción
        emailService.enviarComprobante(saved);
        
        logger.info("Venta procesada: " + saved.getId());
        
        return saved;
    }
}

// 3. Implementaciones concretas (bajo nivel) dependen de abstracciones
@Repository
public class MySQLVentaRepository implements VentaRepository {
    
    @PersistenceContext
    private EntityManager entityManager;
    
    @Override
    public Venta save(Venta venta) {
        entityManager.persist(venta);
        return venta;
    }
    
    @Override
    public Optional<Venta> findById(Long id) {
        return Optional.ofNullable(entityManager.find(Venta.class, id));
    }
    
    @Override
    public List<Venta> findByFecha(LocalDate fecha) {
        return entityManager.createQuery(
            "SELECT v FROM Venta v WHERE DATE(v.fecha) = :fecha", 
            Venta.class
        )
        .setParameter("fecha", fecha)
        .getResultList();
    }
}

@Service
public class SmtpEmailService implements EmailService {
    
    private final JavaMailSender mailSender;
    
    @Override
    public void enviarComprobante(Venta venta) {
        SimpleMailMessage message = new SimpleMailMessage();
        message.setTo(venta.getCliente().getEmail());
        message.setSubject("Comprobante de venta #" + venta.getId());
        message.setText("Gracias por su compra. Total: $" + venta.getTotal());
        mailSender.send(message);
    }
    
    @Override
    public void enviarNotificacion(String destinatario, String mensaje) {
        SimpleMailMessage message = new SimpleMailMessage();
        message.setTo(destinatario);
        message.setText(mensaje);
        mailSender.send(message);
    }
}

@Component
public class Slf4jLogger implements Logger {
    
    private static final org.slf4j.Logger log = 
        LoggerFactory.getLogger(Slf4jLogger.class);
    
    @Override
    public void info(String mensaje) {
        log.info(mensaje);
    }
    
    @Override
    public void error(String mensaje, Throwable ex) {
        log.error(mensaje, ex);
    }
}

// 4. CAMBIO DE IMPLEMENTACIÓN: Solo crear nueva clase y configurar Spring
@Repository
@Profile("postgres") // Se usa si profile es "postgres"
public class PostgreSQLVentaRepository implements VentaRepository {
    
    private final JdbcTemplate jdbcTemplate;
    
    @Override
    public Venta save(Venta venta) {
        // Implementación con PostgreSQL
        jdbcTemplate.update(
            "INSERT INTO venta (cliente_id, total, fecha) VALUES (?, ?, ?)",
            venta.getClienteId(), venta.getTotal(), venta.getFecha()
        );
        return venta;
    }
    
    // ... otros métodos
}

@Service
@Profile("sendgrid") // Se usa si profile es "sendgrid"
public class SendGridEmailService implements EmailService {
    
    private final SendGrid sendGrid;
    
    @Override
    public void enviarComprobante(Venta venta) {
        // Implementación con SendGrid API
        Email from = new Email("no-reply@example.com");
        Email to = new Email(venta.getCliente().getEmail());
        String subject = "Comprobante de venta #" + venta.getId();
        Content content = new Content("text/plain", 
            "Gracias por su compra. Total: $" + venta.getTotal());
        Mail mail = new Mail(from, subject, to, content);
        
        Request request = new Request();
        request.setMethod(Method.POST);
        request.setEndpoint("mail/send");
        request.setBody(mail.build());
        sendGrid.api(request);
    }
    
    @Override
    public void enviarNotificacion(String destinatario, String mensaje) {
        // Implementación con SendGrid
    }
}

// 5. Testing simplificado (usar mocks)
@ExtendWith(MockitoExtension.class)
class VentaServiceTest {
    
    @Mock
    private VentaRepository ventaRepository;
    
    @Mock
    private EmailService emailService;
    
    @Mock
    private Logger logger;
    
    @InjectMocks
    private VentaService ventaService;
    
    @Test
    void testProcesarVenta() {
        // Arrange
        VentaDTO dto = new VentaDTO();
        Venta venta = new Venta(dto);
        venta.setId(1L);
        
        when(ventaRepository.save(any(Venta.class))).thenReturn(venta);
        
        // Act
        Venta result = ventaService.procesarVenta(dto);
        
        // Assert
        assertNotNull(result);
        verify(ventaRepository).save(any(Venta.class));
        verify(emailService).enviarComprobante(venta);
        verify(logger, times(2)).info(anyString());
    }
}
```

**Beneficios**:
- ✅ Cambio de implementación sin modificar lógica de negocio
- ✅ Fácil testear (usar mocks/stubs)
- ✅ Bajo acoplamiento
- ✅ Spring maneja la inyección de dependencias
- ✅ Configuración flexible con profiles

### Ejemplo: PagoService

```java
// ❌ ANTES: Alto acoplamiento
public class PagoService {
    private MercadoPagoGateway mercadoPago = new MercadoPagoGateway();
    
    public Pago procesarPago(Venta venta) {
        return mercadoPago.cobrar(venta.getTotal(), venta.getClienteId());
    }
}

// ✅ DESPUÉS: Inversión de dependencias
public interface PaymentGateway {
    Pago procesarPago(BigDecimal monto, String clienteId);
    EstadoPago consultarEstado(String transaccionId);
    void reembolsar(String transaccionId, BigDecimal monto);
}

@Service
public class PagoService {
    
    private final PaymentGateway paymentGateway;
    
    public PagoService(PaymentGateway paymentGateway) {
        this.paymentGateway = paymentGateway;
    }
    
    public Pago procesarPago(Venta venta) {
        return paymentGateway.procesarPago(
            venta.getTotal(), 
            venta.getClienteId()
        );
    }
}

// Implementaciones intercambiables
@Service
@Primary // Por defecto
public class MercadoPagoGateway implements PaymentGateway {
    public Pago procesarPago(BigDecimal monto, String clienteId) {
        // Integración con MercadoPago API
    }
}

@Service
@ConditionalOnProperty(name = "payment.gateway", havingValue = "stripe")
public class StripeGateway implements PaymentGateway {
    public Pago procesarPago(BigDecimal monto, String clienteId) {
        // Integración con Stripe API
    }
}

@Service
@Profile("test")
public class MockPaymentGateway implements PaymentGateway {
    public Pago procesarPago(BigDecimal monto, String clienteId) {
        // Mock para testing
        return new Pago(UUID.randomUUID().toString(), monto, EstadoPago.APROBADO);
    }
}
```

---

## Resumen Comparativo

| Principio | ❌ Violación | ✅ Cumplimiento | Beneficio Principal |
|-----------|--------------|-----------------|---------------------|
| **SRP** | Clase con múltiples responsabilidades | Una clase, una responsabilidad | Cambios localizados |
| **OCP** | Modificar código existente para extender | Extender sin modificar | Código estable |
| **LSP** | Subclase rompe contrato de la base | Subclases sustituibles | Polimorfismo correcto |
| **ISP** | Interfaces grandes y generales | Interfaces pequeñas y específicas | Desacoplamiento |
| **DIP** | Depender de implementaciones concretas | Depender de abstracciones | Flexibilidad |

### Checklist SOLID

Al diseñar una clase, verifica:

**Single Responsibility:**
- [ ] ¿La clase tiene un único motivo para cambiar?
- [ ] ¿El nombre de la clase describe una única responsabilidad?
- [ ] ¿La clase tiene < 200 líneas?

**Open/Closed:**
- [ ] ¿Puedo agregar funcionalidad sin modificar código existente?
- [ ] ¿Uso abstracciones (interfaces/clases abstractas)?
- [ ] ¿Evito largos bloques if-else/switch?

**Liskov Substitution:**
- [ ] ¿Las subclases respetan el contrato de la clase base?
- [ ] ¿Evito lanzar excepciones inesperadas en subclases?
- [ ] ¿Las precondiciones/postcondiciones se mantienen?

**Interface Segregation:**
- [ ] ¿Mis interfaces son pequeñas y cohesivas?
- [ ] ¿Los clientes solo dependen de métodos que usan?
- [ ] ¿Evito métodos "dummy" o que lanzan `UnsupportedOperationException`?

**Dependency Inversion:**
- [ ] ¿Mi lógica de negocio depende de interfaces, no de implementaciones?
- [ ] ¿Uso inyección de dependencias?
- [ ] ¿Puedo cambiar implementaciones sin tocar mi lógica de negocio?

---

## Referencias

### Libros
- **Clean Code** - Robert C. Martin
- **Clean Architecture** - Robert C. Martin
- **Agile Software Development, Principles, Patterns, and Practices** - Robert C. Martin
- **Design Patterns: Elements of Reusable Object-Oriented Software** - Gang of Four

### Documentación Interna
- [Design Patterns](../design_patterns/README.md)
- [Casos de Uso](../productos/backoffice/docs/05-casos-uso/README.md)
- [ADRs](../productos/backoffice/docs/08-arquitectura/adr/README.md)

---

**Última actualización**: 2026-01-08  
**Versión**: 1.0.0  
**Autor**: @team-architecture  
**Revisores**: @team-backend, @team-docs
