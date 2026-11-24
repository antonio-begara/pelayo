# Jarvis Pelayo

Artefacto personal para registrar el día a día de Pelayo (tomas, pañales, sueño, peso, salud) y recibir briefs y recomendaciones de Jarvis. Vive fuera de Lumiere a propósito: es personal.

- Artefacto: https://claude.ai/artifact/5KBBpYnsi81Ed36pQBoGHN
- Página: `pelayo.html` (se publica con la herramienta Artifact; capabilities `db` + `user` + `downloads`, reglas por defecto: quien tenga nivel Contributor o superior escribe)
- Identidad: fondo lavanda-leche, tinta ciruela, acento albaricoque; display Baloo 2, texto Mulish. Colores por tipo de evento: toma albaricoque, pañal verde salvia, sueño índigo, salud rosa.
- Para registrar desde la cuenta personal: invitarla por email como **Editor** desde el menú Compartir del artefacto. Un enlace público solo da lectura.

## Datos (base de datos del artefacto)

| Colección / doc | Qué guarda |
|---|---|
| `settings/profile` | `name`, `birth` (datetime-local), `birth_g`, `birth_cm`, `feeding` (`biberon` por defecto; `pecho`/`mixta`), `night` (`22-7`…), `ml_default`, `vitd` (bool: botón diario) |
| `events/<id>` | Un registro por evento. Comunes: `type`, `t` (ms epoch), `day` (`YYYY-MM-DD` local), `note`, `by` (id de usuario), `created`, `updated` |
| `messages/<id>` | Mensajes de Antonio a Jarvis: `text`, `t`, `day`, `by`, `read`, `reply` (Jarvis escribe `reply` y pone `read: true`) |
| `brief/latest` | El brief que muestra la pestaña Brief: `date`, `time`, `title`, `lead`, `blocks[{kind: rec|flag|'', title, items[]}]` |
| `briefs/<date>` | Archivo de briefs (misma forma que `brief/latest`) |

Tipos de evento y campos propios:

- `toma`: `source` (`pecho`/`biberon`), `side` (`izq`/`der`/`ambos`), `min_izq`, `min_der`, `ml` (ofrecido; o refuerzo tras pecho), `left_ml` (lo que deja; tomado = `ml - left_ml`), `milk` (`formula`/`materna`)
- `hito`: `key` (talon, rev1, rev1m, vac2m, vac4m, rev6m, vac11m, vac12m, vac15m), `what` · `cita`: `what`, `t`, `done` (bool)
- `vitd`: una por día cuando `settings.vitd` está activo (botón en cabecera)
- `panal`: `pis` (bool), `caca` (bool), `color` (`amarilla`/`verde`/`marrón`/`negra`/`otra`)
- `sueno`: `t` = se duerme, `end` (ms o null), `open` (true mientras duerme)
- `peso`: `g` · `talla`: `cm` · `temp`: `c` · `vitd` · `med`: `what` · `bano` · `extraccion`: `ml` · `nota`

## Brief

Una tarea programada (pendiente de crear cuando Antonio confirme la hora) leerá con ArtifactData los `events` de las últimas 24-48 h y los `messages` sin leer, escribirá `brief/latest` y `briefs/<date>`, y contestará cada mensaje en su campo `reply`.
