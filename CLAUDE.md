# Guardia Z — contexto para Claude Code

Juego educativo de Microbiología I (medicina, USAL). Shooter por turnos en primera persona: los pacientes zombies son bacterias y hongos con síntomas exagerados; las armas son antibióticos y antifúngicos.

## Stack
- Sitio estático: todo vive en `public/index.html` (HTML + CSS + JS vanilla, canvas 320x180 escalado con `image-rendering: pixelated`).
- Sin build, sin framework, sin dependencias. Fuentes de Google Fonts (Pixelify Sans, Chakra Petch).
- Deploy en Vercel (`vercel.json` → `outputDirectory: public`).
- Progreso en `localStorage`: `gz-dex` (bestiario), `gz-best` (récord) y `gz-last` (última partida: historial por sala + registro completo, para el repaso).

## Dónde está cada cosa en `public/index.html`
- `TABS` y `W`: arsenal. Cada arma tiene `id`, `tab` (mecanismo), `fam`, `cls` (`ab` antibacteriano por defecto, `af` antifúngico, `sp` especial), `press` (presión de selección), `ammo` opcional y `desc` (`clase`, `mecanismo`, `cubre`, `noCubre`) que se ve en la ficha del arma (botón **i**).
- `E`: bestiario. Cada patógeno tiene `f` (piso 1-4), `boss`, `kind` (`hongo` o bacteria por defecto), `stain`, `flags` (`anaerobio`, `intracel`, `sinPared`, `myco`), `feats` (rasgos visuales que dibuja `drawZombie`), `sint` (primera impresión), `atk` (cada ataque anota su factor de virulencia en la historia clínica), y:
  - `pistas`: `interrogatorio` (4: edad y huésped → riesgos → exposición → evolución), `examen` (2 signos semiológicos), `lab` (exactamente 3: tinción → cultivo y bioquímica → confirmatoria). Se revelan de a una con I / E / S y cada una gasta turno.
  - `diferenciales`: `{idDeOtroPatogeno: 'el dato que los separa'}`, mínimo 3, con ids que existan. Las opciones de DIAGNOSTICAR salen de acá y el texto se muestra como «No es X: …» si errás. Arrancá en minúscula: va después de los dos puntos.
  - `clave`: la cadena de pistas que se muestra al acertar («Alcohólico + lóbulo superior + … = Klebsiella»).
  - La respuesta a cada arma:
  - `best`: arma de elección (daño alto) con explicación
  - `ok`: sirve pero no es de elección (daño bajo)
  - `res`: resistencia (el bicho se cura) con el mecanismo
  - `nul`: sin efecto con explicación específica
  - `allAb` / `abHurt`: caso ECEH (todo ATB empeora)
  - `ficha`: morfología, virulencia, cuadro, tratamiento, trampa de examen
- `CASOS` (después de `E`): varios pacientes por patógeno. `c0` nombra el cuadro del caso base (el del propio `E`); cada caso de `v` trae `cuadro`, `sint` (opcional), `pistas` (`interrogatorio` 4, `examen` 2, `lab` 3 opcional: si falta usa el del base), `clave` y `diferenciales` (opcional). Si el cuadro cambia el tratamiento, el caso trae `best`/`ok`/`res`/`nul`: cada arma listada ahí se saca de las otras categorías del base (ej.: pielonefritis saca la nitrofurantoína de `best`). `caseOf(id, ci)` arma el paciente; `nextEnemy()` elige el caso, `rec.ci` lo guarda para el repaso y `dex[id].cs` cuenta los cuadros vistos.
- Los patógenos sumados con `E.push(...)` (S. agalactiae, Proteus, C. psittaci, Coccidioides) se agregan como diferenciales de los viejos en la lista que está justo después de `EBY`.
- `generic()`: explicaciones por defecto cuando un arma no está listada (vancomicina vs Gram −, aminoglucósidos vs anaerobios, betalactámicos vs sin pared, etc.).
- `multFor(n)`: bonus del diagnóstico según pistas usadas (×1,6 con hasta 2, baja 0,075 por pista, piso ×1,3). Los turnos gastados en pistas llaman a `enemyTurn(true)` (el ataque pega la mitad).
- Registro: `say(html, tipo)` agrega una línea a `S.journal`; los tipos y sus filtros están en `TAG` (grupos `p` pistas, `d` disparos, `e` errores). `S.hist` guarda una ficha por sala (pistas vistas, diagnóstico, disparos) que alimenta `repaso()` y `repasoTxt()`.
- `drawCorridor`, `drawZombie`, `drawGun`, `frame`: render en canvas.

## Reglas para editar contenido
- Español rioplatense, voseo.
- Precisión médica primero: bibliografía de referencia Murray, Microbiología Médica (7ª ed.).
- Cada `best`, `ok` y `res` explica el POR QUÉ (mecanismo), no solo el nombre.
- Un rasgo nuevo en `feats` necesita su dibujo en `drawZombie`.
- No usar la palabra "azar".

## Ideas pendientes
- Más patógenos (Rickettsia, Bordetella, Nocardia, Actinomyces, M. leprae, Coccidioides, Malassezia).
- Modo "oral": caso clínico mezclado y hay que justificar el arma.
- Patógeno nuevo = completar `pistas`, `diferenciales` y `clave`, sumarlo como diferencial de los bichos con los que se confunde y darle al menos un caso extra en `CASOS`.
- Fuente de contenido: Notion MICRO 1 → «Hojas de memoria — Síndromes» y los «Resumen completo para el oral» de hongos.
- Sonido 8-bit y ranking compartido.
