# HERDR - Cheatsheet

## Herdr — Cheatsheet

> Versión de referencia: **herdr 0.9.3**. Docs oficiales: [herdr.dev/docs](https://herdr.dev/docs/)

### ¿Qué es herdr?

**herdr** es un _terminal workspace manager_: un multiplexor como **tmux/zellij**, pero con soporte nativo para agentes de IA. Detecta si en un panel corre Claude Code, Codex, etc. y muestra su estado (`working`, `blocked`, `done`, `idle`) en la barra lateral, con notificaciones cuando un agente termina o pide permiso.

Además:

* Las sesiones son **persistentes** (como tmux): haces _detach_ y todo sigue corriendo.
* Es **mouse-native**: puedes hacer clic en paneles, pestañas y workspaces, arrastrar bordes para redimensionar y usar menú con clic derecho.
* Tiene **CLI y socket API** para controlar paneles por script.
* Soporta **máquinas remotas por SSH** y **git worktrees**.

{% hint style="info" %}
**¿Trae editor de código?** No como tal: herdr **no incluye un editor ni explorador de archivos propio**. Lo que sí puedes hacer es:

* Abrir `nvim`, `vim`, `micro`, etc. en cualquier panel (es una terminal real).
* Lanzar el editor en un **popup flotante** con un atajo propio (ver Comandos personalizados).
* `prefix + e` abre el **scrollback** (historial) del panel en tu `$EDITOR`.
* Ampliar herdr con **plugins** (scripts en Bash, JS, Lua, Rust...).
{% endhint %}

### Conceptos

| Concepto      | Qué es                                                                                  |
| ------------- | --------------------------------------------------------------------------------------- |
| **Session**   | Servidor persistente en segundo plano. `herdr` se conecta a la sesión por defecto.      |
| **Workspace** | Contenedor por proyecto (normalmente uno por repo o tarea). Se ven en la barra lateral. |
| **Tab**       | Una distribución de paneles dentro de un workspace (como una _window_ de tmux).         |
| **Pane**      | Una terminal real dentro de una pestaña. Sigue viva aunque hagas detach.                |
| **Agent**     | Un agente de IA que herdr detecta corriendo en un panel.                                |

#### Modos

| Modo              | Qué hace                                                            |
| ----------------- | ------------------------------------------------------------------- |
| **Terminal mode** | Modo normal: las teclas van al panel enfocado.                      |
| **Prefix mode**   | Tras pulsar el prefijo, la siguiente tecla es una orden para herdr. |
| **Navigate mode** | Navegación persistente por workspaces y paneles (`prefix + w`).     |
| **Copy mode**     | Moverse por el historial y copiar texto (`prefix + [`).             |
| **Resize mode**   | Redimensionar paneles con el teclado (`prefix + r`).                |

### El prefijo

El prefijo por defecto es **`Ctrl + b`** (igual que tmux).

`prefix + c` significa: pulsa `Ctrl + b`, **suelta**, y luego pulsa `c`.

{% hint style="success" %}
`prefix + ?` muestra **todos los atajos activos** dentro de herdr. Pulsa `/` para filtrar.
{% endhint %}

{% hint style="warning" %}
Si ejecutas herdr **dentro de tmux**, tmux se come el `Ctrl + b`. Usa herdr fuera de tmux o cambia el prefijo a `ctrl+a` en la config.
{% endhint %}

### Los 5 atajos esenciales

| Acción                        | Atajo                       |
| ----------------------------- | --------------------------- |
| Nueva pestaña                 | `prefix + c`                |
| Dividir a la derecha / abajo  | `prefix + v` / `prefix + -` |
| Moverse entre paneles         | `prefix + h / j / k / l`    |
| Navegar workspaces            | `prefix + w`                |
| Detach (dejar todo corriendo) | `prefix + q`                |

### Paneles (dividir pantalla)

| Acción                                    | Atajo                                 |
| ----------------------------------------- | ------------------------------------- |
| Dividir vertical (panel a la derecha)     | `prefix + v`                          |
| Dividir horizontal (panel abajo)          | `prefix + -`                          |
| Foco izquierda / abajo / arriba / derecha | `prefix + h` / `j` / `k` / `l`        |
| Siguiente / anterior panel                | `prefix + Tab` / `prefix + Shift+Tab` |
| Intercambiar panel (swap)                 | `prefix + Shift + h / j / k / l`      |
| Zoom (pantalla completa del panel)        | `prefix + z`                          |
| Modo redimensionar                        | `prefix + r` (luego flechas/hjkl)     |
| Cerrar panel                              | `prefix + x`                          |
| Renombrar panel                           | `prefix + Shift + p`                  |
| Abrir scrollback en `$EDITOR`             | `prefix + e`                          |
| Modo copia                                | `prefix + [`                          |

> **Equivalencias con tmux:** `%` → `prefix + v`, `"` → `prefix + -`.

### Pestañas (tabs)

| Acción               | Atajo                       |
| -------------------- | --------------------------- |
| Nueva pestaña        | `prefix + c`                |
| Siguiente / anterior | `prefix + n` / `prefix + p` |
| Ir a la pestaña 1–9  | `prefix + 1..9`             |
| Renombrar pestaña    | `prefix + Shift + t`        |
| Cerrar pestaña       | `prefix + Shift + x`        |

### Workspaces y sesión

| Acción                                  | Atajo                |
| --------------------------------------- | -------------------- |
| Selector / navegación de workspaces     | `prefix + w`         |
| Nuevo workspace                         | `prefix + Shift + n` |
| Renombrar workspace                     | `prefix + Shift + w` |
| Cerrar workspace                        | `prefix + Shift + d` |
| Nuevo git worktree                      | `prefix + Shift + g` |
| **Goto picker** (saltar a agente/panel) | `prefix + g`         |
| Ir al origen de la notificación         | `prefix + o`         |
| Mostrar / ocultar barra lateral         | `prefix + b`         |
| Ajustes (settings)                      | `prefix + s`         |
| Recargar config                         | `prefix + Shift + r` |
| Ayuda (todos los atajos)                | `prefix + ?`         |
| Detach                                  | `prefix + q`         |

#### Goto picker (`prefix + g`)

Lista cada agente/terminal agrupado por workspace. `Enter` para saltar.

| Tecla             | Acción                                            |
| ----------------- | ------------------------------------------------- |
| `/`               | Buscar por nombre, agente, pestaña, rama o ruta   |
| `j / k` o flechas | Moverse por la lista                              |
| `← / →`           | Saltar al workspace anterior / siguiente          |
| `b / w / i / d`   | Filtrar agentes _blocked / working / idle / done_ |
| `a`               | Mostrar todos de nuevo                            |

### Modo copia (`prefix + [`)

| Tecla                                      | Acción                            |
| ------------------------------------------ | --------------------------------- |
| `h j k l`                                  | Mover cursor                      |
| `w b e` / `W B E`                          | Saltar por palabras               |
| `{` / `}`                                  | Saltar por párrafos               |
| `PageUp` / `PageDown`, `Ctrl+u` / `Ctrl+d` | Desplazarse                       |
| `/` / `?`                                  | Buscar hacia adelante / atrás     |
| `n` / `N`                                  | Siguiente / anterior coincidencia |
| `v` o `Espacio`                            | Empezar selección                 |
| `y` o `Enter`                              | Copiar selección                  |
| `q` o `Esc`                                | Salir                             |

{% hint style="info" %}
Con el ratón no hace falta el modo copia: **arrastra para seleccionar** y se copia automáticamente (`copy_on_select = true`).
{% endhint %}

### Configuración

Archivo: `~/.config/herdr/config.toml`

```bash
herdr --default-config        # imprime la config por defecto completa (con todos los atajos)
herdr config check            # valida config.toml
herdr server reload-config    # recarga sin reiniciar (o prefix + Shift + r)
herdr config reset-keys       # hace backup y borra atajos personalizados
```

#### Cambiar el prefijo

```toml
[keys]
prefix = "ctrl+a"
```

#### Atajos sin prefijo (`ctrl+alt`)

La familia `ctrl+alt` casi no la usa ningún terminal, así que es segura para atajos directos. Puedes mantener ambos:

```toml
[keys]
focus_pane_left  = ["prefix+h", "ctrl+alt+h"]
focus_pane_down  = ["prefix+j", "ctrl+alt+j"]
focus_pane_up    = ["prefix+k", "ctrl+alt+k"]
focus_pane_right = ["prefix+l", "ctrl+alt+l"]
previous_tab     = ["prefix+p", "ctrl+alt+["]
next_tab         = ["prefix+n", "ctrl+alt+]"]
new_tab          = ["prefix+c", "ctrl+alt+c"]
split_vertical   = ["prefix+v", "ctrl+alt+d"]
split_horizontal = ["prefix+minus", "ctrl+alt+shift+d"]
zoom             = ["prefix+z", "ctrl+alt+z"]
```

{% hint style="warning" %}
Evita en Linux: `ctrl+alt+flechas` (GNOME), `ctrl+alt+t` (abrir terminal), `ctrl+alt+l` (bloqueo KDE), `ctrl+alt+F1..F12` (TTYs).
{% endhint %}

#### Comandos personalizados (popups)

Puedes asignar atajos que lancen programas. Tipos:

| `type`  | Comportamiento                                       |
| ------- | ---------------------------------------------------- |
| `popup` | Terminal flotante encima del layout (no lo modifica) |
| `pane`  | Panel temporal que se cierra al terminar el comando  |
| `shell` | Se ejecuta en segundo plano, sin interfaz            |

```toml
# Editor en popup flotante
[[keys.command]]
key = "prefix+alt+e"
type = "popup"
command = "nvim ."
width = "90%"
height = "90%"

# lazygit en popup
[[keys.command]]
key = "prefix+alt+g"
type = "popup"
command = "lazygit"
width = "80%"
height = "80%"

# Monitor del sistema
[[keys.command]]
key = "prefix+t"
type = "popup"
command = "btop"
width = "90%"
height = "90%"
```

{% hint style="info" %}
Así se consigue la experiencia de "abrir el editor de código" dentro de herdr: `nvim`, `lazygit`, `yazi` (explorador de archivos), `btop`... en popups con un atajo.
{% endhint %}

#### Otras opciones útiles

```toml
[theme]
name = "catppuccin"         # también tokyo-night, gruvbox, ...

[terminal]
new_cwd = "follow"          # paneles nuevos abren en el directorio actual

[ui]
tab_bar_position = "top"    # o "bottom"
pane_borders = "auto"       # "always" | "off"
show_agent_labels_on_pane_borders = true

[ui.toast]
delivery = "herdr"          # notificación cuando un agente termina: herdr | terminal | system | off
```

### Comandos CLI

#### Sesión y servidor

```bash
herdr                             # abrir o reconectar a la sesión persistente
herdr --session trabajo           # sesión con nombre
herdr session list                # listar sesiones
herdr session attach <nombre>     # reconectar a una sesión
herdr session stop <nombre>       # parar una sesión
herdr session delete <nombre>     # borrar una sesión parada
herdr status                      # estado de cliente y servidor
herdr server stop                 # parar el servidor
herdr update                      # actualizar herdr
herdr completion zsh              # autocompletado para zsh
```

#### Paneles, pestañas y workspaces (por script)

```bash
herdr pane list                   # listar paneles
herdr pane split                  # dividir panel
herdr pane focus                  # enfocar panel vecino
herdr pane resize                 # redimensionar
herdr pane zoom                   # zoom on/off
herdr pane run                    # ejecutar un comando en un panel
herdr pane send-text              # enviar texto a un panel
herdr pane send-keys              # enviar teclas a un panel
herdr pane read                   # leer la salida de un panel
herdr pane close                  # cerrar panel

herdr tab list | create | focus | rename | close
herdr workspace list | create | focus | rename | close
herdr worktree list | create | open | remove
```

{% hint style="info" %}
Usa `--help` en cualquier subcomando para ver sus opciones, p. ej. `herdr pane split --help`.
{% endhint %}

#### Agentes de IA

```bash
herdr agent list                  # agentes detectados y su estado
herdr agent read <id>             # leer la salida de un agente
herdr agent prompt <id> "..."     # enviarle un prompt
herdr agent wait <id>             # esperar a que cambie de estado
herdr agent attach <id>           # conectarse directo a su terminal
herdr agent start                 # iniciar un agente en un panel existente
herdr integration install <agente>  # instalar integración (claude, codex...)
herdr integration status
```

#### Máquinas remotas (SSH)

```bash
herdr --remote usuario@host       # abrir herdr en una máquina remota
herdr machine add                 # guardar una máquina SSH
herdr machine list
herdr machine status
herdr --machine <nombre> <cmd>    # ejecutar un comando en esa máquina
```

### Herdr vs tmux — chuleta rápida

| Acción             | tmux             | herdr            |
| ------------------ | ---------------- | ---------------- |
| Prefijo            | `Ctrl+b`         | `Ctrl+b`         |
| Split derecha      | `prefix %`       | `prefix v`       |
| Split abajo        | `prefix "`       | `prefix -`       |
| Moverse            | `prefix flechas` | `prefix h/j/k/l` |
| Zoom               | `prefix z`       | `prefix z`       |
| Cerrar panel       | `prefix x`       | `prefix x`       |
| Nueva ventana/tab  | `prefix c`       | `prefix c`       |
| Siguiente/anterior | `prefix n / p`   | `prefix n / p`   |
| Copy mode          | `prefix [`       | `prefix [`       |
| Detach             | `prefix d`       | `prefix q`       |
| Ayuda              | `prefix ?`       | `prefix ?`       |
| Reattach           | `tmux attach`    | `herdr`          |

