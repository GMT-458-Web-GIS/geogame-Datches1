# Geogame Final

This is a refactored version of the Turkey Celebrity GeoGame using Vite + React.

## Project Structure

- `public/`: Static assets (images, fonts, maps, sfx).
- `src/`: Source code.
  - `assets/`: Source assets (if any).
  - `components/`: Reusable UI components.
    - `Game/`: Game-specific components (Map, HUD, etc.).
  - `pages/`: Page components (Intro, Game).
  - `data/`: Data files.
- `vite.config.js`: Vite configuration.

## How to Run

1. Open a terminal in this directory (`GeogameFinal`).
2. Install dependencies:
   ```bash
   npm install
   ```
3. Start the development server:
   ```bash
   npm run dev
   ```
4. Build for production:
   ```bash
   npm run build
   ```

## Notes

- The map logic is encapsulated in `src/components/Game/GameMap.jsx`.
- Game state is managed in `src/pages/Game/Game.jsx`.
- Global styles and fonts are in `src/index.css`.

