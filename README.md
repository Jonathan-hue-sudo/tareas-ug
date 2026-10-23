# Panel de Tareas UG

Extensión de Chrome que agrega al **Área personal** del campus virtual (Moodle) un panel con tus tareas, cuestionarios y foros, organizados en **próximas, atrasadas, entregadas y pendientes**.

> Proyecto no oficial. No está afiliado a la Universidad de Guayaquil ni a Moodle.

## Características

- Pestañas: **Próximas**, **Atrasadas**, **Entregadas** y **Pendientes**, con contador en cada una.
- Soporta **tareas** (`assign`), **cuestionarios** (`quiz`) y **foros** (`forum`).
- Cuenta regresiva hasta la fecha límite: ámbar si vence en menos de 24 h, rojo si ya venció.
- Filtro por curso.
- Botón **Actualizar** para volver a consultar.
- Botón **✓** para marcar o desmarcar manualmente una actividad como entregada (se recuerda entre sesiones).
- Caché local: las entregas confirmadas no se vuelven a consultar, así que cargan más rápido después de la primera vez.

## Instalación

La extensión no está en la Chrome Web Store, así que se carga manualmente:

1. Descarga este repositorio: **Code → Download ZIP**, y descomprímelo.
2. Abre `chrome://extensions` en Chrome (también funciona en Edge, Brave y otros navegadores basados en Chromium).
3. Activa **Modo de desarrollador** (arriba a la derecha).
4. Pulsa **Cargar descomprimida** y selecciona la carpeta del proyecto (la que contiene `manifest.json`).
5. Entra a `https://campusvirtual2.ug.edu.ec/my/`. El panel **Mis actividades** aparece encima de "Próximos eventos".

## Cómo funciona

La extensión corre solo en `https://*.ug.edu.ec/my/*` y usa tu sesión ya iniciada en el navegador:

1. Lee el `sesskey` de la propia página de Moodle.
2. Consulta el calendario mediante la API interna de Moodle (`core_calendar_get_calendar_monthly_view`) para los últimos 3 meses y los próximos 2.
3. Para cada actividad abre su página y detecta si ya la entregaste:

| Tipo | Cómo se detecta como entregada |
|---|---|
| Tarea | La página muestra "Enviado para calificar" |
| Cuestionario | Existe un intento "Finalizado" |
| Foro | Apareces como autor en la lista de debates (heurística) |

4. Clasifica cada actividad según su fecha límite y su estado de entrega.

## Privacidad

- No se envían datos a ningún servidor externo: todas las consultas van únicamente a tu campus virtual.
- No se guardan contraseñas ni tokens. Solo se usa `chrome.storage.local` para recordar qué actividades están entregadas y tus marcas manuales.
- Permiso solicitado: `storage`.

## Limitaciones

- La detección en **foros** es aproximada. Si solo respondiste a debates de otros, puede no reconocerlo; usa el botón ✓ para corregirlo.
- La primera carga puede tardar unos segundos porque revisa la página de cada actividad.
- Depende de la estructura de Moodle. Si la universidad actualiza la plataforma o cambia el tema, puede requerir ajustes.
- El texto de detección está pensado para Moodle en **español** o **inglés**.

## Configuración

En `content.js`, al inicio del archivo:

| Constante | Descripción | Valor por defecto |
|---|---|---|
| `MONTHS_BACK` | Meses hacia atrás que se consultan | `3` |
| `MONTHS_AHEAD` | Meses hacia adelante que se consultan | `2` |
| `FALSE_TTL` | Tiempo antes de volver a revisar una actividad no entregada | 10 min |
| `EVENT_TYPES` | Tipos de evento que se muestran | `due`, `close` |

## Estructura

```
tareas-ug/
├── manifest.json   # Configuración de la extensión (Manifest V3)
├── content.js      # Lógica: consulta a Moodle, clasificación y panel
├── styles.css      # Estilos del panel
└── README.md
```

## Solución de problemas

- **No aparece el panel:** recarga la página con `Ctrl + F5` y verifica que la extensión esté activada en `chrome://extensions`.
- **Aparece un error en rojo:** abre la consola (`F12`), copia el mensaje y abre un *issue*.
- **Tu campus usa otro dominio:** edita `matches` en `manifest.json` para que coincida con tu URL.

## Contribuir

Los *issues* y *pull requests* son bienvenidos.

## Licencia

MIT. Agrega un archivo `LICENSE` si quieres aplicar esta licencia.
