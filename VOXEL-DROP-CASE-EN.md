# Voxel Drop 3D

An independent browser puzzle showing interactive 3D frontend work. I implemented the application and game interaction; it is a portfolio project, not paid work for a casino or gaming operator.

[Play the demo](https://voxel-drop-3d.vercel.app/) · [Read the source](https://github.com/fsaidad/voxel-drop-3d)

![Actual gameplay](voxel-drop-preview.png)

## Try it in a minute

1. Select **Focus** on the opening screen. The current UI and tutorial are in Russian; Focus is the calmer mode with a landing projection and three-piece preview.
2. Drag the board to change the camera angle. Movement follows the camera.
3. Use **W/A/S/D** to move a piece, **Q/E** to rotate around Y, and arrow keys to rotate around X/Z.
4. Press **Space** to drop, **C** to hold, and **P** or **Esc** to pause. The score, completed layers and next pieces show the session state.

## What this demonstrates

- Three.js integrated with React, TypeScript and Zustand.
- A deterministic game domain separated from rendering and independently testable.
- Instanced voxel rendering and on-demand updates while idle.
- Camera-relative controls, keyboard/touch support and versioned local progress.
- Linting, type checks, tests and a production build in CI.

[Verified main-branch CI run](https://github.com/fsaidad/voxel-drop-3d/actions/runs/40086065174)

## Useful for similar work

Interactive product interfaces, browser games, configurators and gamified experiences with complex state. A commercial brief still needs its own requirements and acceptance criteria.

## Boundaries

This demo does not demonstrate real-money payments, gaming-provider integrations, regulatory systems, a multiplayer backend or measured commercial conversion. No client revenue or user metrics are claimed.
