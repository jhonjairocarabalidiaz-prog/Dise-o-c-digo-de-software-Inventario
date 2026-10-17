# Sistema multiplataforma de ventas e inventario — XYZ

MVP académico basado en el caso de estudio: una interfaz web adaptable, una API central para productos/ventas y carpetas preparadas para escritorio (Electron) y móvil (Capacitor). El backend de demostración guarda los datos en un archivo JSON local para facilitar su ejecución sin instalar una base de datos. Se incluye `database/schema.sql` como esquema de referencia para una futura integración con MySQL.

## Requisitos
- Node.js 18 o superior (incluye npm).
- Para compilar escritorio: dependencias de Electron y herramientas de empaquetado.
- Para compilar móvil: Android Studio/SDK para Android; Xcode en macOS para iOS.

## Iniciar el sistema (web)
1. Descomprima el ZIP.
2. Abra una terminal en la carpeta raíz `proyecto_multiplataforma`.
3. Ejecute `npm start`.
4. Abra `apps/web/index.html` en un navegador moderno. Mantenga la terminal abierta mientras utiliza el sistema.
5. La API queda disponible en `http://localhost:3000/api/health`.

No se requiere `npm install` para iniciar la API ni para ejecutar las pruebas porque el servidor usa módulos nativos de Node.js. Los datos iniciales se crean automáticamente en `server/data/store.json` la primera vez que se ejecuta la API.

## Funciones incluidas
- Consultar y buscar productos.
- Crear productos con código único, precio y existencias iniciales.
- Registrar ventas con validación de cantidad y stock disponible.
- Descontar existencias al registrar una venta.
- Consultar resumen de inventario, ingresos e historial de ventas.
- API REST básica y pruebas unitarias de reglas de negocio.

## Pruebas
Desde la carpeta raíz:

```bash
npm test
```

## Aplicación de escritorio (Electron)
```bash
npm install
npm run desktop
```
Para generar instaladores, después de instalar dependencias:

```bash
npm run desktop:build
```
Los artefactos se guardan en `apps/desktop/dist/`. La API debe estar iniciada en otra terminal (`npm start`).

## Aplicación móvil (Capacitor)
La configuración base se encuentra en `apps/mobile/capacitor.config.json`. Para completar la plataforma móvil, instale las dependencias y añada la plataforma deseada desde la carpeta raíz, por ejemplo:

```bash
npm install
cd apps/mobile
npx cap add android
npx cap sync android
n```
La configuración actual de la interfaz web necesita que `API` en `apps/web/app.js` apunte a la dirección accesible del servidor. En un teléfono real, `localhost` se refiere al propio teléfono; cambie `http://localhost:3000/api` por la dirección IP del equipo servidor o por la URL HTTPS publicada. No exponga el servidor en una red pública sin autenticación, HTTPS y controles de seguridad.

## Persistencia y seguridad: límites del prototipo
- El almacenamiento JSON es para demostración local y no sustituye una base de datos concurrente para producción.
- La API de ejemplo no incluye autenticación ni autorización; no debe publicarse en Internet tal cual.
- Para producción, implemente MySQL con transacciones InnoDB, control de concurrencia (por ejemplo, `SELECT ... FOR UPDATE`), autenticación, autorización, validación reforzada, HTTPS, copias de seguridad y secretos seguros.
- El archivo `database/schema.sql` ofrece una base inicial para la estructura MySQL.

## Árbol de carpetas
```text
proyecto_multiplataforma/
├── apps/
│   ├── web/          # Interfaz responsiva HTML/CSS/JavaScript
│   ├── desktop/      # Envoltorio Electron
│   └── mobile/       # Configuración Capacitor
├── database/         # Esquema SQL de referencia para MySQL
├── server/
│   ├── src/index.js  # API REST y reglas básicas
│   └── test/         # Pruebas con node:test
├── packages/shared/  # Reservado para componentes compartidos
└── .github/workflows/ci.yml
```

## Relación con el informe académico
La estructura materializa la propuesta de versiones web, escritorio y móvil, API centralizada, reutilización, pruebas y automatización. La versión web y la API local son ejecutables; el empaquetado Electron y la compilación móvil requieren instalar sus herramientas y dependencias. No se afirma que se haya desplegado en Azure ni que se haya conectado ya a MySQL.
