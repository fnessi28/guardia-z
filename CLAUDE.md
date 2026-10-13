# Guardia Z — contexto para Claude Code

Juego educativo de Microbiología I (medicina, USAL). Shooter por turnos en primera persona: los pacientes zombies son bacterias y hongos con síntomas exagerados; las armas son antibióticos y antifúngicos.

## Stack
- Sitio estático: todo vive en `public/index.html` (HTML + CSS + JS vanilla, canvas 320x180 escalado con `image-rendering: pixelated`).
- Sin build, sin framework, sin dependencias. Fuentes de Google Fonts (Pixelify Sans, Chakra Petch).
- Deploy en Vercel (`vercel.json` → `outputDirectory: public`).
- Progreso (bestiario y récord) en `localStorage` con claves `gz-dex` y `gz-best`.

## Dónde está cada cosa en `public/index.html`
- `TABS` y `W`: arsenal. Cada arma tiene `id`, `tab` (mecanismo), `fam`, `cls` (`ab` antibacteriano por defecto, `af` antifúngico, `sp` especial), `press` (presión de selección) y `ammo` opcional.
- `E`: bestiario. Cada patógeno tiene `f` (piso 1-4), `boss`, `kind` (`hongo` o bacteria por defecto), `stain`, `flags` (`anaerobio`, `intracel`, `sinPared`, `myco`), `feats` (rasgos visuales que dibuja `drawZombie`), `epi`, `sint`, `lab`, `atk`, y la respuesta a cada arma:
  - `best`: arma de elección (daño alto) con explicación
  - `ok`: sirve pero no es de elección (daño bajo)
  - `res`: resistencia (el bicho se cura) con el mecanismo
  - `nul`: sin efecto con explicación específica
  - `allAb` / `abHurt`: caso ECEH (todo ATB empeora)
  - `ficha`: morfología, virulencia, cuadro, tratamiento, trampa de examen
- `generic()`: explicaciones por defecto cuando un arma no está listada (vancomicina vs Gram −, aminoglucósidos vs anaerobios, betalactámicos vs sin pared, etc.).
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
- Sonido 8-bit y ranking compartido.
