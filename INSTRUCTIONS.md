# Instrucciones de instalación y ejecución

## 📋 Requisitos

- **Unity Hub**
- **Unity Editor**, Recomendado: **Unity 6 (6000.x LTS)**

## 📥 Instalación

### 1. Clonar el repositorio

```bash
git clone https://github.com/carlossanchezh/slice-demo-the-frog.git
```

### 2. Abrir el proyecto en Unity

1. Abre **Unity Hub**.

2. Pulsa **Add → Add project from disk**.

3. Selecciona la carpeta raíz del proyecto donde se encuentre la carpeta `Assets/` (`slice-demo-the-frog/`).

4. Si Unity avisa de que falta la versión del editor, instálala desde el propio Hub.

5. Abre el proyecto. La primera importación puede tardar varios minutos.

## ▶️ Ejecución

### Ejecutar dentro del editor (modo desarrollo)

1. En Unity, abre la escena:
   ```
   Assets/Scenes/Tutorial.unity
   ```

2. Pulsa el botón **Play** en la parte superior del editor.

3. El juego comenzará en la ventana **Game**.

### Ejecutar una build (versión compilada)

Si ya tienes una build generada:

- **Windows:** ejecuta `TheFrog.exe`.

- **macOS:** abre `TheFrog.app`.

- **Linux:** ejecuta `./TheFrog.x86_64`.

Si quieres **generar tu propia build**:

1. En Unity: **File → Build Settings…**

2. Selecciona la plataforma destino (Windows, macOS, Linux, WebGL…).

3. Pulsa **Build** y elige una carpeta de salida.

4. Ejecuta el binario generado.

## 🎮 Cómo jugar

### Objetivo

Avanzar por las salas para escapar con vida. Por el camino tendrás que gestionar tus recursos: vida, cargas y pociones, y enfrentarte a enemigos.

### Reglas básicas

1. Tienes **3 corazones** de vida. Si llegas a 0, el nivel se reinicia.

2. El bastón tiene **cargas limitadas**. Cada disparo gasta una y se recargan solas con el tiempo.

3. Las **pociones** se guardan en un inventario de 3 slots. Solo puedes llevar 3 a la vez.

4. Las **monedas** se usan en el mercado para comprar pociones.

5. Al recibir daño eres **invulnerable durante un instante**.

6. El **dash** también te hace invulnerable mientras dura. Úsalo para esquivar balas.

### Consejos

- **Prioriza esquivar antes que disparar.** El dash es tu mejor defensa contra el bullet hell.

- **No malgastes cargas** disparando a lo loco. Espera a tener al enemigo cerca y apunta bien.

- **Guarda pociones.** Las salas se complicarán a medida que avanza el juego.

- **Explora bien los cofres.** Sueltan drops aleatorios que pueden darte pociones gratis.

- **Las cajas también sueltan objetos.** Recuerda, no las ignores.

- **Si te quedas sin cargas, usa el dash para huir** y espera a que se recarguen.

- **Fíjate en los patrones de los enemigos.** Apréndetelos.

## 🕹️ Controles

| Acción | Tecla / Botón |
|--------|---------------|
| Mover | `WASD` o flechas |
| Dash | `Espacio` |
| Disparar | Click izquierdo |
| Apuntar el disparo| Puntero del ratón |
| Abrir cofre | `E` |
| Usar poción | `F` |
| Seleccionar slot inventario | `1`, `2`, `3` |
| Pausa | `Esc` o botón de pausa |

> Se recomienda jugar con teclado y ratón para que la experiencia de apuntado y disparo sea la mejor posible.

## 🌐 Jugar online 

Puedes jugar a ***The Frog*** directamente en el navegador a través de unity play con este enlace [The Frog](https://play.unity.com/en/games/28b815df-f834-4061-aa80-67b9b66f3a93/the-frog)
