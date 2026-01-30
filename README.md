# Apuntes Consolidados: Programación Orientada a Objetos - Bimestre II

---

## 1. Java Swing – Interfaces Gráficas de Usuario

Java Swing es una biblioteca gráfica multiplataforma para crear interfaces de escritorio en Java.  
**Características clave:**
- Independiente del sistema operativo.
- Basado en componentes reutilizables y contenedores.

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

## 2. Persistencia de Datos con SQLite

SQLite es un gestor de base de datos embebido, ligero y sin servidor, ideal para aplicaciones de escritorio.

### 2.1 Conexión desde Java (JDBC)

```java
Connection conn = DriverManager.getConnection("jdbc:sqlite:app.db");
```

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

Arquitectura que separa responsabilidades en capas para facilitar mantenimiento, escalabilidad y trabajo colaborativo.

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

## 4. Modelado de Datos (MER) y Mapeo a Clases

### 4.1 Elementos del MER
- **Entidades:** Representan objetos del dominio (ej. Usuario, Producto).
- **Atributos:** Propiedades de las entidades.
- **Claves:** 
  - **PK (Primary Key):** Identificador único (recomendado: `INTEGER AUTOINCREMENT`).
  - **FK (Foreign Key):** Relación con otra tabla.
- **Cardinalidades:** Define relaciones (1:1, 1:N, N:M).

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

| Relación       | Descripción                                                                                     | Ejemplo en código                                                                                  |
|----------------|-------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------|
| **Asociación** | Relación estructural entre objetos.                                                             | `Profesor trabaja en Departamento`                                                                 |
| **Composición**| Relación fuerte: el hijo no existe sin el padre (ciclo de vida compartido).                     | `Automovil` crea `Motor` en su constructor.                                                        |
| **Agregación** | Relación débil: el hijo puede existir independientemente del padre.                             | `Automovil` recibe `Motor` desde fuera (inyección).                                                |
| **Herencia**   | Relación “es-un”. Java no soporta herencia múltiple de clases (usa interfaces).                 | `class Coche extends Vehiculo`                                                                     |
| **Interfaz**   | Contrato que define comportamientos. Permite simular herencia múltiple.                         | `class Coche implements IVolante, IPito`                                                           |

---

## 7. Patrones y Buenas Prácticas

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

Estos fundamentos permiten desarrollar **aplicaciones de escritorio robustas, escalables y bien estructuradas**, preparadas para entornos empresariales reales.
