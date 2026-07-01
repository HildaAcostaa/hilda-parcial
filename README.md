# Parcial Práctico #1 - Sistema de Gestión de Colaboradores

Este repositorio contiene la solución correspondiente al Primer Examen Parcial Práctico de la asignatura de Ingeniería de Software. Consiste en un sistema web básico para la captura, estandarización y exportación de información del personal técnico utilizando arquitectura limpia en PHP.

## 🚀 Características del Proyecto

- **Estandarización de Datos (Data Sanitization):** Implementación de políticas de consistencia de formatos. El sistema convierte automáticamente nombres y apellidos a mayúsculas, y correos electrónicos a minúsculas, eliminando espacios redundantes antes del almacenamiento.
- **Persistencia Segura:** Uso de la API de Objetos de Datos de PHP (PDO) con manejo robusto de excepciones para mitigar fallos críticos.
- **Exportación de Reportes:** Módulo independiente para la generación y descarga en caliente de nóminas de personal en formato CSV.
- **Carga de Componentes:** Mecanismo de mapeo dinámico a través de un cargador automático basado en estándares profesionales de interoperabilidad.

## 📁 Estructura del Directorio

```text
hilda-parcial/
│
├── clases/
│   └── myConexionPDO.php    # Capa de datos (Modelo) y gestión de conexión PDO
│
├── index.php                # Interfaz de usuario (Vista) y lógica de procesamiento
├── generar.php              # Script de extracción y exportación a archivo CSV
├── autoload.php             # Administrador de carga automática de dependencias
├── .gitignore               # Configuración de exclusión de archivos para Git
└── README.md                # Documentación del proyecto
