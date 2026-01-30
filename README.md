# Apuntes Consolidados: Programación Orientada a Objetos - Bimestre II

---

**AUTOR:** Samara Salazar

## 1. Java Swing – Interfaces Gráficas de Usuario

### ¿Qué es Java Swing?
Es la biblioteca oficial de Java para crear interfaces gráficas de escritorio. Permite construir ventanas, botones, formularios y otros componentes visuales de manera multiplataforma.

### ¿Cómo se usa?
- **JFrame**: Es la ventana principal. Se configura con tamaño, posición y comportamiento de cierre.
- **JPanel**: Son contenedores que organizan otros componentes dentro del JFrame.
- **Componentes**: Botones (JButton), campos de texto (JTextField), etiquetas (JLabel), etc.
- **Layout Managers**: Controlan cómo se disponen los componentes dentro de los contenedores.

### Características importantes:
- **Independiente del SO**: La misma aplicación funciona en Windows, Linux y macOS.
- **Basado en eventos**: Los componentes responden a acciones del usuario (clics, teclas).
- **Arquitectura MVC**: Separa los datos (Modelo) de la vista (Vista) y el control (Controlador).

### Mejores prácticas:
- Usar múltiples JPanels para organizar la interfaz.
- Separar la lógica de la interfaz (no poner código de negocio en los ActionListener).
- Usar hilos separados para operaciones largas (SwingWorker).

### 1.1 Componentes principales

| Componente   | Descripción                                                                                     | Ejemplo de uso                                                                                     |
|--------------|-------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------|
| **JFrame**   | Ventana principal de la aplicación.                                                             | `JFrame frame = new JFrame("Título"); frame.setSize(500,400); frame.setVisible(true);`             |
| **JPanel**   | Contenedor para organizar componentes visualmente.                                              | `JPanel panel = new JPanel(); frame.add(panel, BorderLayout.CENTER);`                               |
| **JButton**  | Botón que ejecuta acciones mediante `ActionListener`.                                           | `JButton btn = new JButton("Guardar"); btn.addActionListener(e -> {...});`                         |
| **JLabel**   | Muestra texto estático o imágenes.                                                              | `JLabel lbl = new JLabel("Texto"); lbl.setFont(new Font("Arial", Font.BOLD, 18));`                 |
| **JTextField / JPasswordField** | Campos para ingreso de datos.                                                                  | `JTextField txt = new JTextField(15);`                                                             |

**Buenas prácticas:**
- Usar múltiples `JPanel` dentro de un `JFrame` para mejorar organización y mantenimiento.
- Separar la lógica de la interfaz (patrón MVC simplificado).

---

## 2. SQLite – Base de Datos Embebida

### ¿Qué es SQLite?
Un sistema de gestión de bases de datos ligero que funciona como una biblioteca dentro de la aplicación, sin necesidad de servidor.

### ¿Cómo se usa desde Java?
1. **Conexión**: Usando JDBC con el driver de SQLite.
2. **Operaciones**: Ejecutando sentencias SQL a través de Statement o PreparedStatement.
3. **Transacciones**: Agrupando operaciones para mantener consistencia.

### Ventajas:
- **Sin instalación**: Solo necesita un archivo .db
- **Portátil**: La base de datos es un solo archivo.
- **Ideal para aplicaciones de escritorio**: Simple y eficiente.

### Seguridad crítica:
- **PreparedStatement**: Siempre usar para evitar inyecciones SQL.
- **Validación**: Validar datos antes de enviarlos a la base.

### Operaciones avanzadas:
- **JOINs**: Combinar datos de múltiples tablas.
- **Vistas**: Consultas predefinidas para simplificar acceso.
- **Índices**: Mejorar rendimiento en búsquedas frecuentes.

<img width="718" height="468" alt="17697975950127709696201318257078" src="https://github.com/user-attachments/assets/284efb21-30f1-456d-a8b6-a625a984a192" />


### 2.2 Operaciones CRUD básicas

| Operación | SQL Ejemplo                                                                                     |
|-----------|-------------------------------------------------------------------------------------------------|
| **CREATE** | `CREATE TABLE usuario (id INTEGER PRIMARY KEY AUTOINCREMENT, nombre TEXT, edad INTEGER);`       |
| **INSERT** | `INSERT INTO usuario(nombre, edad) VALUES ('Ana', 22);`                                         |
| **UPDATE** | `UPDATE usuario SET edad = 23 WHERE id = 1;`                                                    |
| **DELETE** | `DELETE FROM usuario WHERE id = 1;`                                                             |

### 2.3 PreparedStatement (seguridad)

```java
String sql = "INSERT INTO usuario(nombre, edad) VALUES (?, ?)";
PreparedStatement ps = conn.prepareStatement(sql);
ps.setString(1, "Carlos");
ps.setInt(2, 30);
ps.executeUpdate();
```
**Ventaja:** Previene inyecciones SQL.

---

## 3. Arquitectura N-Capas (N-Tier/N-Layer)


### ¿Qué es?
Una arquitectura que organiza el código en capas o niveles separados, cada uno con responsabilidades específicas.

### Las 4 capas principales:

#### 1. **Capa de Presentación (GUI/UI)**
- **Función**: Mostrar información al usuario y capturar sus acciones.
- **Componentes**: Ventanas, formularios, botones.
- **Responsabilidad**: Solo se encarga de la interfaz visual, NO contiene lógica de negocio.

#### 2. **Capa de Lógica de Negocio (Business Logic/BL)**
- **Función**: Aplicar las reglas del negocio, validaciones y coordinación.
- **Ejemplos**: Validar que un usuario tenga edad suficiente, calcular impuestos, verificar permisos.
- **Importante**: Esta capa no sabe nada de la interfaz gráfica.

#### 3. **Capa de Acceso a Datos (Data Access/DAL)**
- **Función**: Comunicarse con la base de datos.
- **Componentes**: DAOs (Data Access Objects) que ejecutan SQL.
- **Responsabilidad**: Solo ejecuta operaciones CRUD (Create, Read, Update, Delete).

#### 4. **Capa de Entidades/DTOs**
- **Función**: Transportar datos entre capas.
- **Componentes**: Clases simples con atributos, getters y setters.
- **Importante**: No tienen lógica, solo datos.

### Beneficios de esta arquitectura:
- **Mantenibilidad**: Cada capa se puede modificar sin afectar las otras.
- **Reutilización**: La lógica de negocio sirve para diferentes interfaces.
- **Testabilidad**: Se puede probar cada capa por separado.
- **Trabajo en equipo**: Diferentes personas pueden trabajar en diferentes capas.

### Flujo típico:
1. Usuario hace clic en botón (GUI)
2. GUI llama a un método de la capa BL
3. BL valida reglas y llama a DAL si es necesario
4. DAL ejecuta SQL en la base de datos
5. Los datos viajan de regreso a través de DTOs



### 3.1 Capas principales

| Capa              | Responsabilidad                                                                 | Componentes típicos                                    |
|-------------------|---------------------------------------------------------------------------------|--------------------------------------------------------|
| **Presentación (GUI)** | Interfaz visual, captura eventos, muestra datos.                               | JFrame, JPanel, JButton, JTable                        |
| **Lógica de Negocio (BL)** | Aplica reglas del dominio, valida datos, coordina procesos.                    | Clases Service, Manager, DTOs, procesos ETL            |
| **Acceso a Datos (DAC/DAO)** | Comunicación directa con la base de datos.                                    | Clases DAO, DataHelper, CRUD, PreparedStatement        |
| **Entidades (Model)** | Transporte de datos entre capas.                                               | POJOs, DTOs, Beans                                     |

### 3.2 Principios clave
- **Separación de responsabilidades:** Cada capa tiene una única función.
- **Bajo acoplamiento:** Las capas se comunican mediante interfaces o DTOs.
- **Alta cohesión:** Cada capa agrupa funcionalidades relacionadas.
- **Conocimiento unidireccional:** Una capa solo conoce a la inmediatamente inferior.

### 3.3 Estructura de paquetes sugerida
```
src/
├── ui/           (GUI - Presentación)
├── service/      (Business Logic - Lógica de negocio)
├── dao/          (Data Access Object - Acceso a datos)
├── dto/          (Data Transfer Objects - Entidades)
└── util/         (Utilidades, helpers, estilos)
```

---



## 4. Modelado de Datos (MER)

### ¿Qué es?
El Modelo Entidad-Relación es una representación gráfica de cómo se organizarán los datos en la base de datos.

### Elementos principales:
- **Entidades**: Objetos del mundo real (Usuario, Producto, Factura).
- **Atributos**: Propiedades de las entidades (nombre, precio, fecha).
- **Relaciones**: Cómo se conectan las entidades entre sí.
- **Cardinalidades**: Cantidad de relaciones (1:1, 1:N, N:M).

### ¿Cómo se usa?
1. Identificar las entidades importantes del sistema.
2. Determinar sus atributos y tipos de datos.
3. Definir cómo se relacionan.
4. Transformar en tablas SQL.




### 4.2 Mapeo MER → Clases Java
Cada tabla se mapea a una clase Java (DTO/Entity):

```java
public class Usuario {
    private int id;
    private String nombre;
    private int edad;
    // getters y setters
}
```

---

## 5. Control de Errores y Logs


### Manejo de Excepciones:
- **Try-Catch**: Capturar errores sin que el programa se detenga.
- **Finally**: Ejecutar código siempre (como cerrar conexiones).
- **Tipos de excepciones**: SQLException, IOException, NullPointerException.

### ¿Por qué es importante?
- **Experiencia de usuario**: Mostrar mensajes amigables en lugar de errores técnicos.
- **Estabilidad**: El programa sigue funcionando aunque algo falle.
- **Depuración**: Saber qué salió mal para corregirlo.

### Sistema de Logs:
- **¿Qué es?**: Un registro de eventos del sistema.
- **¿Para qué sirve?**: Para auditar, monitorear y depurar.
- **Niveles**: INFO (flujo normal), WARNING (algo extraño), ERROR (fallo grave).

### Implementación:
- Usar Logger en lugar de System.out.println().
- Registrar eventos importantes: inicio de sesión, errores, accesos.
- Configurar diferentes niveles según el entorno (desarrollo/producción).



### 5.1 Manejo de excepciones
```java
try {
    // Código susceptible a errores
} catch (ExceptionType e) {
    // Manejo del error
} finally {
    // Código que siempre se ejecuta (ej. cerrar conexiones)
}
```

### 5.2 Logs (bitácoras)
Reemplazar `System.out.println()` con un sistema de logging:
```java
Logger logger = Logger.getLogger("AppLog");
logger.info("Sistema iniciado");
logger.warning("Advertencia detectada");
logger.severe("Error grave");
```
**Niveles típicos:** INFO, WARN, ERROR.

---

## 6. Relaciones entre Clases (UML)

## Patrones de Diseño Aplicados

### 1. **DAO (Data Access Object)**
- **Propósito**: Separar la lógica de acceso a datos del resto de la aplicación.
- **Cómo funciona**: Cada entidad tiene su DAO con métodos CRUD.
- **Beneficio**: Si cambia la base de datos, solo se modifica el DAO.

### 2. **DTO (Data Transfer Object)**
- **Propósito**: Transportar datos entre capas sin exponer detalles internos.
- **Cómo funciona**: Clases simples que solo contienen datos.
- **Beneficio**: Desacoplamiento entre capas.

### 3. **Factory (Fábrica)**
- **Propósito**: Centralizar la creación de objetos.
- **Cómo funciona**: Una clase que crea instancias de otras clases.
- **Beneficio**: Facilita cambios y pruebas.

### 4. **Singleton (Única instancia)**
- **Propósito**: Asegurar que una clase tenga solo una instancia.
- **Uso común**: Para conexiones a base de datos.
- **Beneficio**: Controlar el acceso a recursos compartidos.



| Relación       | Descripción                                                                                     | Ejemplo en código                                                                                  |
|----------------|-------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------|
| **Asociación** | Relación estructural entre objetos.                                                             | `Profesor trabaja en Departamento`                                                                 |
| **Composición**| Relación fuerte: el hijo no existe sin el padre (ciclo de vida compartido).                     | `Automovil` crea `Motor` en su constructor.                                                        |
| **Agregación** | Relación débil: el hijo puede existir independientemente del padre.                             | `Automovil` recibe `Motor` desde fuera (inyección).                                                |
| **Herencia**   | Relación “es-un”. Java no soporta herencia múltiple de clases (usa interfaces).                 | `class Coche extends Vehiculo`                                                                     |
| **Interfaz**   | Contrato que define comportamientos. Permite simular herencia múltiple.                         | `class Coche implements IVolante, IPito`                                                           |

---

## 7. Patrones y Buenas Prácticas

## Desarrollo de Aplicaciones Empresariales

### Características de aplicaciones empresariales:
1. **Procesos complejos**: Flujos de trabajo con múltiples pasos.
2. **Reglas estrictas**: Validaciones específicas del negocio.
3. **Seguridad**: Autenticación y control de acceso.
4. **Persistencia**: Datos que deben guardarse permanentemente.
5. **Escalabilidad**: Capacidad de crecer con el tiempo.

### Enfoque profesional:
- **Diseño antes de codificar**: Planear la arquitectura y modelo de datos.
- **Separación de responsabilidades**: Cada componente tiene una tarea específica.
- **Código mantenible**: Otros programadores deben poder entenderlo.
- **Documentación**: Comentar el código y mantener documentación.

### Ciclo de desarrollo:
1. Análisis de requisitos
2. Diseño de arquitectura y MER
3. Implementación por capas
4. Pruebas y depuración
5. Documentación
6. Mantenimiento


### 7.1 Factory Pattern
Centraliza la creación de objetos (ej. instanciación de clases BL o DAO).

### 7.2 DTO (Data Transfer Object)
Clases simples para transportar datos entre capas sin exponer lógica interna.

### 7.3 Estilos globales (AppStyle)
Clase con constantes de diseño (colores, fuentes) para mantener consistencia visual.

### 7.4 Componentes personalizados
Reutilización de controles Swing con estilos aplicados desde `AppStyle`.

---

## 8. Configuración del Entorno
## Integración de Conocimientos

### Proyecto típico del bimestre:
1. **Diseño**: MER y diagramas UML para planear la aplicación.
2. **Base de datos**: Crear tablas en SQLite según el MER.
3. **Backend**: Implementar DAOs, DTOs y lógica de negocio.
4. **Frontend**: Crear interfaz con Java Swing.
5. **Integración**: Conectar todas las capas.
6. **Pruebas**: Verificar que todo funcione correctamente.

### Habilidades desarrolladas:
- **Pensamiento arquitectónico**: Diseñar antes de programar.
- **Programación estructurada**: Organizar el código en capas.
- **Persistencia de datos**: Guardar y recuperar información.
- **Interfaces de usuario**: Crear aplicaciones visuales.
- **Resolución de problemas**: Debuggear y corregir errores.

### Aplicaciones prácticas:
- Sistemas de gestión (inventario, ventas, usuarios)
- Aplicaciones de escritorio para pequeñas empresas
- Herramientas administrativas
- Sistemas de registro y control



### 8.1 Driver SQLite JDBC
- Librería requerida: `sqlite-jdbc-3.40.0.0.jar`
- Añadir al classpath del proyecto.

### 8.2 Estructura de proyecto recomendada
- Separación clara por capas (ui, service, dao, dto, util).
- Uso de paquetes descriptivos.
- Configuración centralizada (ej. `DataHelper` para conexiones).

---

## 9. Conclusiones del Bimestre

Durante el bimestre se integraron:
- **Java Swing** para interfaces gráficas.
- **SQLite** para persistencia de datos.
- **Arquitectura N-Capas** para organización profesional del código.
- **UML** para modelado de relaciones entre clases.
- **Control de errores y logs** para robustez.
- **Patrones de diseño** (Factory, DTO) y buenas prácticas de POO.

Estos fundamentos permiten desarrollar aplicaciones de escritorio robustas, escalables y bien estructuradas, preparadas para entornos empresariales reales.
.
