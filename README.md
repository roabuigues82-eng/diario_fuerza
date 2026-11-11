# 💪 Diario de Fuerza

App web para registrar mis entrenamientos Full Body (lunes, miércoles y viernes), seguir el progreso de los pesos y controlar el peso corporal en la fase de volumen.

## Funciones

- **Entrenar**: rutina del día cargada; apuntas kg y repeticiones por serie. Muestra lo que hiciste la última vez y sugiere el peso de hoy (si completaste todas las series → sube; si no → mantén). Temporizador de descanso con aviso sonoro.
- **Progreso**: gráfica por ejercicio con peso máximo y 1RM estimado (fórmula de Epley), % de mejora y volumen semanal total.
- **Peso**: registro del peso corporal, cambio total y ritmo de las últimas 4 semanas (kg/semana).
- **Calorías**: buscador con ~150 alimentos y platos españoles (lentejas, tortilla, pizza, bocadillos…) en raciones caseras (plato, rebanada, vaso, puñado o gramos) que **calcula las kcal y la proteína solo**; "Tu plato" para juntar varios alimentos en una comida y guardarla como comida rápida; comidas usadas hace poco; proteína, actividad en casa y en el trabajo, y **balance comido − gastado**. Avisa si gastas más de lo que comes y sugiere qué comida añadir para compensar.
- **Historial**: editar o borrar sesiones, descargar/importar copias de seguridad en JSON (incluye rutina, calorías, actividad, peso y ajustes).
- **Ajustes**: perfil (para calcular el gasto), editor de rutina (cambiar, añadir, quitar y reordenar ejercicios) y comidas favoritas.
- **Avisos automáticos** al abrir la app: entreno del día, entrenos de la semana, si vas retrasada con la comida según la hora, balance de ayer, pesaje semanal, ajuste de calorías según la tendencia del peso (con botón para aplicarlo) y ejercicios estancados.
- **Resumen de los últimos 7 días** en Progreso.
- **Importa energía activa, pasos y peso de la app Salud del iPhone** mediante un Atajo.
- **Instalable y funciona sin conexión** (PWA).

## Publicarla con GitHub Pages

1. Crea un repositorio nuevo en GitHub (por ejemplo `diario-fuerza`).
2. Sube todos los archivos de esta carpeta (`index.html`, `manifest.json`, `sw.js`, `icon.svg`, `README.md`) con **Add file → Upload files**.
3. Ve a **Settings → Pages**, en *Source* elige **Deploy from a branch**, rama `main` y carpeta `/ (root)`. Guarda.
4. En un par de minutos estará en `https://TU-USUARIO.github.io/diario-fuerza/`.
5. En el móvil, ábrela y elige **Añadir a pantalla de inicio** para usarla como una app.

## Dónde se guardan los datos

Los datos se guardan en el **almacenamiento local del navegador** (localStorage) del dispositivo donde la usas. No se suben a ningún servidor ni a GitHub.

- Cada dispositivo tiene sus propios datos. Para pasarlos de uno a otro: **Historial → Descargar copia** en uno e **Importar copia** en el otro.
- Si borras los datos de navegación, se pierden: descarga una copia de vez en cuando.

## Cómo se calcula el gasto de calorías

```
Gasto del día = basal (Mifflin-St Jeor) × factor de día normal
              + gimnasio (si hay sesión registrada ese día)
              + actividades apuntadas
```

- Actividades: kcal netas = (MET − 1) × peso (kg) × horas. Se resta 1 MET porque el reposo ya está en el basal.
- El peso usado es el último registrado en la pestaña Peso.
- Balance = comido − gasto. El objetivo es que sea ≥ al "superávit mínimo" de Ajustes (0 = comer al menos lo que se gasta).
- Son **estimaciones**: la referencia real es la báscula. Si el peso no sube en 2–3 semanas, sube el objetivo de kcal.

## Personalizar la rutina

Se edita desde la propia app en **Ajustes → Rutina**. La rutina original está en `index.html`, en el objeto `DEFAULT_RUTINA` (botón "Restaurar rutina original"). Cada ejercicio tiene:

| Campo | Significado |
|---|---|
| `id` | Identificador único (E1, E2…). No lo cambies si ya tienes datos guardados. |
| `series`, `reps` | Objetivo de series y repeticiones |
| `descanso` | Segundos de descanso (temporizador) |
| `rir` | Repeticiones en recámara objetivo |
| `inc` | Kg que sugiere subir cuando completas todo |

## Tecnologías

HTML, CSS y JavaScript sin frameworks · [Chart.js](https://www.chartjs.org/) para las gráficas · localStorage · Service Worker.

## Conectar con Salud (iPhone)

Una app web no puede leer Salud directamente, así que se usa un Atajo que copia tus datos y la app los pega.

**Crear el atajo «Salud → Diario»** (app Atajos → **+**):

1. **Buscar muestras de salud** → Tipo: *Energía activa* · Fecha de inicio: *es hoy*.
2. **Calcular estadísticas** → *Suma* de *Muestras de salud*.
3. **Redondear número** → a *Unidades*.
4. Repite 1–3 con Tipo: *Pasos*.
5. **Texto** → escribe `activa=` + variable *Número redondeado* (el 1.º) + `;pasos=` + variable *Número redondeado* (el 2.º). Queda así: `activa=512;pasos=8423`
6. **Copiar al portapapeles** → *Texto*.
7. **Mostrar notificación** → «Datos copiados: abre Diario de Fuerza y pulsa Pegar datos de Salud».

La primera vez pedirá permiso para leer Salud. Opcional: añade `;peso=` con tu último peso (Buscar muestras de salud → Tipo *Peso* · Ordenar por *Fecha de inicio* · *Más reciente primero* · Limitar a 1).

**Usarlo**: ejecuta el atajo (desde un widget o automatización a las 21:30) → abre la app → *Calorías* → **Pegar datos de Salud**. Al importar energía activa, la app pasa a usar Salud como fuente de actividad (cámbialo en Ajustes → Perfil). Si llevas Apple Watch en el gimnasio, marca la casilla para no contar el gimnasio dos veces.

> Si usas la app desde Safari (sin instalar), el atajo puede terminar con **Abrir URL** `https://TU-USUARIO.github.io/REPO/?activa=…&pasos=…` y se importa sola.

## Recordatorios en el iPhone

Atajos → **Automatización** → **+** → *Hora del día* → elige hora y días → *Ejecutar inmediatamente* → acción **Mostrar notificación**. Ejemplos:

| Cuándo | Mensaje |
|---|---|
| L, X, V · 17:00 | 🏋️ Hoy toca gimnasio |
| Diario · 10:30 | 🥤 Batido de media mañana |
| Diario · 16:30 | 🍓 Merienda pre-entreno |
| Lunes · 07:45 | ⚖️ Pésate en ayunas y apúntalo |
| Diario · 21:30 | Ejecutar el atajo *Salud → Diario* (en vez de notificación, acción **Ejecutar atajo**) |
