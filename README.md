# Juego móvil iOS (gratis y personal)

Guía paso a paso para crear un juego móvil en iOS para uso personal, sin monetización y sin experiencia previa.

## 0. Requisitos mínimos
- Mac con **Xcode** actualizado (desde App Store).
- **Apple ID gratuita** para firmar builds personales (certificados caducan cada 7 días).
- Espacio para el **iOS Simulator** y, si puedes, un **iPhone** para probar.
- Control de versiones con **Git** (opcional pero recomendado).

## 1. Crea el proyecto base en Xcode
1) Abre Xcode → **File > New > Project…** → *iOS App*.
2) Elige **Interface: SwiftUI** y **Language: Swift**. Marca **Include Tests** opcional.
3) Nombre del proyecto y **Bundle Identifier** (ej. `com.tuusuario.tujuego`).
4) Firma con tu Apple ID (Preferences → Accounts). Usa el **team personal** que aparece.
5) Selecciona **SpriteKit** (2D) o **SceneKit** (3D ligero) cuando Xcode lo ofrezca en la plantilla de juego. Para empezar, SpriteKit es más sencillo.
6) Guarda el proyecto y haz un commit inicial en Git si lo usas.

## 2. Prepara la escena inicial
1) Abre `GameScene.swift` y deja una escena simple:
   ```swift
   import SpriteKit
   import SwiftUI

   class GameScene: SKScene {
       override func didMove(to view: SKView) {
           backgroundColor = .black

           let label = SKLabelNode(text: "Hola iOS")
           label.fontName = "Avenir-Heavy"
           label.fontSize = 40
           label.position = CGPoint(x: frame.midX, y: frame.midY)
           addChild(label)
       }
   }
   ```
2) En `ContentView.swift`, muestra la escena:
   ```swift
   import SwiftUI
   import SpriteKit

   struct ContentView: View {
       var scene: SKScene {
           let scene = GameScene()
           scene.scaleMode = .resizeFill
           return scene
       }

       var body: some View {
           SpriteView(scene: scene)
               .ignoresSafeArea()
       }
   }
   ```
3) Ejecuta en el **iOS Simulator** (⌘+R). Debes ver el texto "Hola iOS".

## 3. Añade control táctil y movimiento básico
1) En `GameScene`, detecta toques y mueve un nodo:
   ```swift
   class GameScene: SKScene {
       let player = SKShapeNode(circleOfRadius: 24)

       override func didMove(to view: SKView) {
           backgroundColor = .black
           player.fillColor = .systemYellow
           player.position = CGPoint(x: frame.midX, y: frame.midY)
           addChild(player)
       }

       override func touchesBegan(_ touches: Set<UITouch>, with event: UIEvent?) {
           guard let location = touches.first?.location(in: self) else { return }

           let move = SKAction.move(to: location, duration: 0.25)
           player.run(move)
       }
   }
   ```
2) Prueba en el simulador: toca con el cursor y verifica que el círculo se mueve.
3) Si usas un iPhone real: conecta por USB → selecciona tu dispositivo en la barra superior → **Run**. Acepta los avisos de confianza en el dispositivo.

## 4. Añade arte y sonido
- Sustituye el círculo por un sprite: arrastra imágenes @2x/@3x al asset catalog (`Assets.xcassets`).
- Crea un `SKSpriteNode(imageNamed: "NOMBRE")` y añádelo en lugar del `SKShapeNode`.
- Sonidos: añade `.wav` o `.mp3` al proyecto y usa `run(SKAction.playSoundFileNamed("tap.wav", waitForCompletion: false))` en eventos.

## 5. HUD y progreso básico
- Crea un marcador con `SKLabelNode` para puntuación/vidas.
- Guarda progreso local en `UserDefaults`:
  ```swift
  struct ProgressStore {
      private let key = "highscore"
      func save(_ value: Int) { UserDefaults.standard.set(value, forKey: key) }
      func load() -> Int { UserDefaults.standard.integer(forKey: key) }
  }
  ```
- Usa `ProgressStore` al finalizar una partida para guardar el récord.

## 6. Rendimiento y depuración
- Objetivo: **60 fps**. Evita demasiadas partículas y sombras.
- Activa **Metal API Validation** solo en *Debug* (Scheme > Edit Scheme > Options).
- Usa **Instruments** → *Time Profiler* y *Game Performance* para encontrar cuellos de botella si notas lag.

## 7. Build y distribución personal (gratis)
1) En Xcode, **Product > Scheme > Edit Scheme…** → selecciona `Run` en modo *Release* para medir rendimiento final.
2) Conecta tu iPhone y elige el dispositivo en la barra de destinos.
3) **Product > Archive** (tarda unos minutos). Cuando termine, presiona **Distribute App > Development** para generar la `.ipa`.
4) Instala la build en tu iPhone desde el Organizer. Recuerda que la firma gratuita dura 7 días; cuando caduque, recompila.

## 8. Checklist rápido
- [ ] Proyecto SwiftUI + SpriteKit creado y ejecuta "Hola iOS" en simulador.
- [ ] Nodo jugador se mueve al tocar la pantalla.
- [ ] Sprites y sonidos cargan sin errores.
- [ ] HUD muestra puntuación y `UserDefaults` guarda el récord.
- [ ] Build en *Release* y probada en tu iPhone.

## 9. Próximos pasos opcionales
- Añadir modo infinito con obstáculos y puntuación creciente.
- Crear un tutorial corto con tooltips (SwiftUI overlay sobre la vista del juego).
- Configurar un workflow de `xcodebuild` + `xcrun altool` para automatizar `.ipa` si quieres iterar rápido.
