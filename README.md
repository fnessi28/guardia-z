# Guardia Z

Shooter por turnos (estilo Doom + combates tipo Pokémon) para estudiar Microbiología I.
Cada paciente zombie es una bacteria u hongo con sus síntomas exagerados; las armas son los antibióticos, ordenados por mecanismo de acción.

Sitio 100% estático: un solo `public/index.html`, sin build ni dependencias.

## Probarlo en tu compu

```bash
npm run dev
```

Abrí http://localhost:3000

## Publicarlo en Vercel

### Opción A: desde la terminal (más rápido)

```bash
npm i -g vercel
vercel login
vercel          # primer deploy (preview)
vercel --prod   # deploy a producción
```

Cuando pregunte, aceptá los valores por defecto: no hay build command y la carpeta de salida es `public` (ya está en `vercel.json`).

### Opción B: con GitHub (se actualiza solo en cada push)

```bash
git init
git add .
git commit -m "Guardia Z v1"
gh repo create guardia-z --public --source=. --push
```

Después en vercel.com → Add New → Project → importás el repo `guardia-z` → Deploy.
Framework preset: **Other**. Sin build command.

## Estructura

```
public/index.html   el juego completo (HTML + CSS + JS)
vercel.json         config de Vercel
CLAUDE.md           contexto para Claude Code
```
