# 🐸 Slice Demo *The Frog*


## 📖 Descripción

***The Frog*** es un *vertical slice* de un juego 2D de acción y exploración inspirado en un *bullet hell*. El jugador controla a una rana mágica que combina disparos con su bastón, dash para esquivar y un inventario de pociones para sobrevivir a las salas del nivel y al jefe final.

El objetivo del slice es validar las mecánicas principales, la arquitectura de código y el pipeline de arte antes de escalar a un proyecto completo.

### Mecánicas principales

- 🎮 **Movimiento y dash** Movimiento fluido del personaje con dashes para esquivar.
- ❤️ **Vida** Sistema de vida que al perderla toda reiniciará el nivel.
- 🔥 **Combate a distancia** con el bastón mágico puede lanzar proyectiles para acabar con los enemigos.
- 🧪 **Inventario de pociones** (vida y cargas) con UI propia.
- 🤖 **Enemigos variados**: cuerpo a cuerpo, de rango y un jefe con patrones distintos.
- 💥 **Sistema de retroceso (knockback)** e invulnerabilidad temporal.
- 🪙 **Monedas y mercado** para comprar mejoras.
- 🗺️ **Tilemaps** para la construcción de niveles.
- 🎵 **Audio dinámico**: música normal vs. música de jefe.

## 👥 Autores

- Rubén García Vilches
- Adam El Fakhouri
- Carlos Sánchez Herrero
- Franco Aldair Sosa Martinez
- Da Wei Wu Chen


## 🏗️ Arquitectura 

### 🎨 Diseño

El proyecto está montado sobre el modelo de componentes de Unity, pero intentando no meter todo en un solo script gigante. La idea fue separar responsabilidades por carpetas y por tipo de script, de forma que cuando algo falla sepas más o menos dónde mirar.

#### 🐸 Jugador

- `Movimiento` — lee `Move` y `Dash` del Input System, mueve el `Rigidbody2D` y activa invulnerabilidad durante el dash.
- `PlayerDisparo` — dispara con click izquierdo hacia el ratón, gasta cargas y las recarga con el tiempo.
- `VidaPlayer` — 3 corazones, invulnerabilidad tras recibir daño y muerte al llegar a 0.
- `Animacion` — balanceo del sprite al moverse y voltereta durante el dash.
- `Inventario` — 3 slots para pociones, con UI propia y teclas `1`, `2`, `3` para seleccionar y `F` para usar.

<br>

<p align="center">
  <img src="Assets/Sprites/Rana-pixel(1).png" alt="La Rana" width="140"/>
</p>

#### 🧪 Pociones

Hay dos tipos porque cubren necesidades distintas del combate:

- **Poción de Vida** — cura 1 corazón. Útil para aguantar peleas largas.
- **Poción de Carga** — aumenta en 1 el máximo de balas para siempre. Útil para no quedarte sin disparos en mitad de un boss.

<br>
<br>

<p align="center">
  <img src="Assets/Sprites/IventarioVida.png" alt="Poción de Vida" width="220"/>
  <img src="Assets/Sprites/InventarioCargas.png" alt="Poción de Cargas" width="220"/>
</p>

<br>

#### 🤖 Enemigos y patrones

Cada enemigo tiene su propio script para que sea fácil tocar uno sin romper los demás:

- `EnemigoLigero` — persigue y ataca cuerpo a cuerpo.
- `EnemigoRango` — mantiene distancia y dispara si tiene línea de visión.
- `EnemigoBoss` — usa orbes que lo hacen invulnerable, rush contra el jugador y 3 fases con bullet hell (círculos, conos, espirales y muros).

<p align="center">
  <img src="Assets/Sprites/Melee pix.png" alt="Melee" width="160"/>
  <img src="Assets/Sprites/Bossito.png" alt="Boss Conejo" width="220"/>
  <img src="Assets/Sprites/Distance.png" alt="Distancia" width="120"/>
</p>

#### 🪙 Inventario, monedas y mercado

- `Inventario` maneja 3 slots y su UI (panel + sprite).
- `InventarioManager` guarda las pociones que compres o recojas para la siguiente escena.
- `Mercado` gasta monedas de `ScoreManager` y mete pociones en el inventario.

<br>
<br>

<p align="center">
  <img src="Assets/Sprites/llave.png" alt="Llave" width="90"/>
  <img src="Assets/Sprites/cofre_cerrado.png" alt="Cofre cerrado" width="90"/>
  <img src="Assets/Sprites/pocion vida.png" alt="pocion vida" width="90"/>
  <img src="Assets/Sprites/pocion cargas.png" alt="pot cargas" width="90"/>
  <img src="Assets/Sprites/moneda.png" alt="Moneda" width="90"/>
</p>

<br>

#### 💥 Sistema de daño

Todo lo que puede recibir daño implementa `IRecibeImpactoRetroceso`, que recibe la cantidad y el origen del impacto. El origen sirve para calcular el retroceso (knockback) y que el empujón salga en la dirección correcta.

#### 🖥️ UI

Cada elemento se pinta desde su propio script para no tener un "UI Manager" gigante:
- `BalaGestor` → balas de carga.
- `DashGestor` → indicador de dash.
- `VidaPlayer` → corazones.
- `Score` → contador de monedas.
- `PauseMenu` → panel de pausa y bloqueo de input.

#### 🔊 Audio

- `AudioManager` — singleton que cambia entre música normal y de boss, y expone `PlaySFX` para disparos, impactos y recogidas.

### ⚙️ Funcionamiento 

A continuación se describe el flujo completo de una partida:

#### 1. 🏠 Menú principal

El jugador llega al menú. Desde ahí puede empezar partida, ajustar opciones o salir. El menú se apoya en `PanelAjustes` para saber si el jugador quiere ver los paneles guía durante la partida.

#### 2. 📜 Instrucciones iniciales

Al pulsar Jugar, `GameManager` comprueba si es la primera vez (`juegoIniciado == false`). Si lo es:
- Muestra el panel de instrucciones.
- Pausa el juego con `Time.timeScale = 0`.
- Bloquea el disparo con `PlayerDisparo.puedeDisparar = false`.

El jugador lee y pulsa continuar → `OcultarInstrucciones()` devuelve el tiempo a la normalidad y habilita el disparo.

#### 3. 🌍 Niveles — exploración

El jugador se mueve con el Input System:
- `Movimiento.cs` lee `Move` y `Dash` del `InputActionAsset`.
- El dash aplica velocidad extra durante `dashDuration` y activa invulnerabilidad temporal vía `VidaPlayer.HacerInvulnerable()`.
- `Animacion.cs` balancea el sprite mientras se mueve y hace una voltereta durante el dash.
- `SeguimientoCamara.cs` sigue al jugador con suavizado.

Mientras explora:
- Puede romper **cajas** (`Caja.cs`) que a veces sueltan pociones.
- Puede abrir **cofres** (`Cofre.cs`) pulsando `E` cerca, con drops aleatorios ponderados.
- Puede recoger **pociones** (`PocionVida`, `PocionCarga`) que se añaden al inventario.
- Puede recoger **monedas** que incrementan `ScoreManager.score`.
- Puede coger la **llave** (`Llave.cs`), que lo sigue y abre la puerta correspondiente.

#### 4. 💥 Combate

El disparo se controla desde `PlayerDisparo`:
- Click izquierdo dispara una bala desde el bastón hacia el ratón.
- Cada disparo gasta una carga (se ve en la UI de `BalaGestor`).
- Las cargas se recargan automáticamente con el tiempo.
- Las pociones de carga aumentan el máximo de cargas para siempre.

Contra los enemigos:
- Los proyectiles comprueban tags ignorados (`ignoreTags`) y aplican daño vía `IRecibeImpactoRetroceso`.
- Cada enemigo tiene su vida, retroceso y stun.
- Al morir, sueltan una moneda (`OnDestroy`).

#### 5. 👹 Encuentro con el boss

Cuando el jugador entra en la sala del jefe:
- `EnemigoBoss` comprueba línea de visión.
- Si la tiene, avisa a `AudioManager` para cambiar a `musicaBoss`.
- El boss alterna ataques normales (arco de balas) y ataques fuertes (rush o círculo 360°).
- En la fase de orbes, el boss es invulnerable hasta que destruyes todos los orbes (`Orbes.cs` avisa a `EnemigoBoss`).

#### 6. ⏸️ Pausa

En cualquier momento, `PauseMenu` puede:
- Mostrar el panel de pausa.
- Poner `Time.timeScale = 0`.
- Desactivar el `PlayerInput` para que no se mueva nada.

Al reanudar, vuelve el tiempo a la normalidad y se reactiva el input.

#### 7. 💀 Muerte del jugador

Si `VidaPlayer` llega a 0:
- Se cambia la música a la normal.
- Se muestra el panel de muerte.
- `Time.timeScale = 0` y el GameObject del jugador se desactiva.

Desde ahí se puede reiniciar con `BotonReiniciar`, que:
- Borra las monedas de la escena.
- Resetea score e inventario.
- Recarga la escena actual.

#### 8. 🚪 Cambio de escena

`CambioEscena.cs` detecta el trigger del jugador y muestra un panel de confirmación. Si el jugador acepta, se carga la escena indicada por nombre. Si cancela, hay un cooldown para que no se reactive al instante.

#### 9. 🏆 Final del slice

Al entrar en el trigger de `PantallaFinal`, se muestra el canvas final y se pausa el tiempo. Fin del slice.

### 🛠️ Tecnologías

- **Lenguaje:** C#
- **Motor:** Unity 6
- **UI:** TextMeshPro
- **Niveles:** Tilemap 2D
- **Tests:** Unity Test Framework + GitHub Actions
- **Arte:** Sprites PNG

## 📁 Estructura del proyecto

```plaintext

.
├── .github/workflows/
├── Assets/
│   ├── Animations/                    # Animaciones y controladores
│   ├── Audio/                         # Música y SFX
│   ├── Font/                          # Fuentes TMP
│   ├── Input/                         # InputSystem_Actions.inputactions
│   ├── Prefabs/                       # Prefabs reutilizables
│   ├── Scenes/                        # Escenas del juego
│   ├── Scripts/                       # Código C# 
│   ├── Settings/                      # URP y volumen global
│   ├── Sprites/                       # Sprites e imágenes
│   ├── Tests/                         # Tests EditMode y PlayMode
│   └── Tilemap/                       # Tilemaps y paletas
├── Packages/                          # Dependencias del proyecto
├── ProjectSettings/                   # Configuración del proyecto Unity
├── .gitattributes                     # Clasificación de archivos para GitHub
├── .gitignore                         # Archivos y carpetas ignorados por Git
├── INSTRUCTIONS.md                    # Instrucciones de instalación y ejecución del proyecto
└── README.md                          # Descripción del proyecto 

```

### Scripts principales

| Categoría | Scripts |
|-----------|---------|
| **Jugador** | `Movimiento`, `PlayerDisparo`, `VidaPlayer`, `Animacion`, `Inventario` |
| **Enemigos** | `EnemigoLigero`, `EnemigoRango`, `EnemigoBoss`, `EnemigoBase`, `Orbes` |
| **Sistemas** | `GameManager`, `AudioManager`, `ScoreManager`, `InventarioManager`, `PauseMenu`, `BotonReiniciar` |
| **Objetos** | `Caja`, `Cofre`, `Llave`, `Puerta`, `PocionVida`, `PocionCarga`, `Proyectil` |
| **UI** | `BalaGestor`, `DashGestor`, `Score`, `Mercado`, `Instrucciones`, `PantallaFinal` |
| **Interfaces** | `IRecibeImpacto`, `IRecibeImpactoRetroceso` |
| **Escenas / cámara** | `CambioEscena`, `SeguimientoCamara`, `GuiaMenu`, `PanelAjustes` |

## 🧰 Instalación y ejecución

Ver [INSTRUCTIONS.md](INSTRUCTIONS.md)
