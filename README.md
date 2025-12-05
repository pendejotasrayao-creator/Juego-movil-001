# Juego móvil iOS (gratis y personal)

Este plan resume cómo crear un juego móvil para iOS destinado solo a ti, sin monetización ni costos adicionales.

## 1. Concepto y alcance
- Define la fantasía central y el ciclo de juego (qué haces cada 30 s, 5 min, 30 min).
- Modo offline y sin compras/ads para mantenerlo 100 % gratis.
- Progresión ligera: metas diarias o un sistema de niveles sencillo.

## 2. Stack recomendado (iOS)
- **Motor**: Xcode + Swift/SwiftUI con **SpriteKit** (2D) o **SceneKit** (3D ligero). Es gratis y está bien integrado.
- **Herramientas**: Xcode, iOS Simulator y un iPhone para pruebas reales.
- **Control de versiones**: Git con ramas cortas por feature.

## 3. Crear el proyecto base
1. Abre Xcode → *iOS App* → Swift + SwiftUI; añade SpriteKit si es 2D.
2. Activa *Game Center* solo si necesitas logros; si no, mantenlo desactivado.
3. Configura nombres de bundle y firma con tu Apple ID (cuenta gratuita funciona para uso personal; el provisioning dura 7 días por build).
4. Añade targets de *Debug* y *Release*; en *Release* habilita reducción de tamaño (*Bitcode* ya no aplica, usa *Strip Debug Symbols* y *Optimize for Size*).

## 4. Prototipo jugable rápido
- Implementa movimiento + feedback inmediato (vibración háptica ligera y sonidos cortos).
- Usa place‑holders (formas básicas) para validar el control táctil.
- Prueba en un dispositivo: ajusta sensibilidad y tamaño de botones para pulgares.

## 5. Contenido y UI
- Sustituye place‑holders por sprites/sonidos libres (CC0/CC‑BY). Optimiza a resoluciones @2x/@3x.
- UI con SwiftUI: HUD mínimo (puntuación/vida), menú de pausa, reinicio rápido.
- Guarda progreso en `UserDefaults` o un archivo local; no necesitas backend.

## 6. Calidad y rendimiento
- Apunta a 60 fps; limita partículas y sombras en dispositivos antiguos.
- Usa *Instruments* (Time Profiler, Game Performance) para ubicar cuellos de botella.
- Habilita *Metal API Validation* en Debug y desactívala en Release.

## 7. Build y distribución personal
- Compila en Release para tu dispositivo; firma con tu Apple ID gratuita (expira en 7 días, vuelve a compilar cuando caduque).
- Usa *TestFlight* solo si pagas el Apple Developer Program; para uso personal no es necesario.
- Guarda un checklist de build: versión, ícono, pantalla de inicio, permisos (idealmente ninguno), y pruebas básicas.

## 8. Próximos pasos sugeridos
- Implementar un modo infinito con marcador de máxima puntuación local.
- Añadir vibración háptica configurable y control de volumen en ajustes.
- Crear un tutorial de 30 segundos con tooltips.
- Automatizar builds con `xcodebuild` + un script simple para exportar `.ipa` cuando conectes tu dispositivo.
