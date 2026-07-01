 # — Proyecto Práctico

Breve guía del sistema de inscripción para DevConnect. Esta aplicación, construida en PHP con un patrón MVC ligero, gestiona el registro de asistentes, verifica integridad mediante firma digital y permite exportar los datos a un archivo Excel estilizado.

Contenido rápido
- Visión general
- Requisitos y tecnologías
- Organización del repositorio
- Instalación y ejecución local
- Estructura de datos y reglas
- Validaciones y seguridad
- Problemas conocidos y soluciones
- Créditos

Resumen del proyecto
--------------------
Aplicación web que recopila información básica (nombre, apellido, identidad), datos de contacto y áreas de interés. Cada registro guarda una firma digital (OpenSSL/HMAC) para detectar modificaciones. Los informes son visibles en la interfaz y se pueden exportar como `.xlsx` con estilos para resaltar integridad.

Tecnologías principales
- PHP 8.4 — Lógica y servidores locales
- MySQL — Persistencia de datos (pdo)
- WAMP Server — Entorno de desarrollo recomendado
- Composer — Dependencias (PhpSpreadsheet)
- OpenSSL / HMAC-SHA256 — Firma y verificación de registros

Estructura del proyecto
-----------------------
El repositorio sigue una disposición simple:

```
parcial_practico/
├─ index.php
├─ reporte.php
├─ config/Database.php
├─ controllers/InscriptorController.php
├─ models/InscriptorModel.php
├─ views/form_view.php
├─ views/report_view.php
├─ helpers/{Validator,Sanitizer}.php
├─ exports/export.php
├─ assets/css/style.css
└─ sql/parcial.sql
```

Esquema y restricciones de la base de datos
------------------------------------------
Tablas clave: `paises`, `inscriptores`, `inscriptor_areas`, `areas_interes`.
Las claves foráneas usan políticas restrictivas de borrado y actualización (ON DELETE RESTRICT, ON UPDATE CASCADE) para preservar consistencia.

Instalación rápida (local)
-------------------------
1) Copia el proyecto a tu carpeta `www` de WAMP (por ejemplo `C:\wamp64\www\parcial_practico\`).
2) Desde la raíz del proyecto ejecuta:

```bash
composer install
```

3) Importa la base de datos en phpMyAdmin ejecutando `sql/parcial.sql`.
4) Abre en el navegador:

- Formulario: `http://localhost/parcial_practico/`
- Reporte: `http://localhost/parcial_practico/reporte.php`
- Exportar Excel: `http://localhost/parcial_practico/exports/export.php`

Validaciones implementadas
--------------------------
- `isValidIdentidad()` — formato alfanumérico con guiones (4–20 chars)
- `isValidNombre()` / `isValidApellido()` — solo letras UTF-8, mínimo 2 caracteres
- `isValidEdad()` — rango 1–120
- `isValidSexo()` — valores permitidos: M, F, Otro
- `isValidCorreo()` — validación estándar con `FILTER_VALIDATE_EMAIL`
- `isValidCelular()` — dígitos, signos `+` y guiones
- `isValidAreas()` — al menos un área seleccionada

Seguridad y firma de registros
-----------------------------
Para proteger la integridad se genera una firma sobre los campos principales:

```
Payload = nombre|apellido|identidad|correo|celular|sexo
Firma   = openssl_sign(SHA256) o fallback HMAC-SHA256
```

En el reporte se marca cada fila como **Íntegro** o **Comprometido** según la verificación de la firma.

Problemas comunes y su manejo
----------------------------
- `openssl_pkey_new()` falla en algunos entornos WAMP: el proyecto detecta la configuración `openssl.cnf` y cae a HMAC si es necesario.
- Normalización de nombres y tildes: se aplica `mb_convert_case(..., MB_CASE_TITLE, 'UTF-8')` cuando corresponde.
- Inserciones con múltiples áreas: envueltas en transacciones PDO para asegurar atomicidad.

Autor y créditos
----------------
Desarrollado por **Hilda Acosta** — Ingeniería de Software, Universidad Tecnológica de Panamá.

Curso: Desarrollo de Software VII — Docente: Ing. Irina Fong — Año: 2025

Licencia y notas
----------------
Este repositorio es un ejercicio académico. Mantén la carpeta `keys/` fuera del alcance público en despliegues reales y no modifiques la estructura de la base de datos sin respaldos.

