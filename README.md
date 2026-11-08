# 💪 Diario de Fuerza

App web para registrar mis entrenamientos Full Body (lunes, miércoles y viernes), seguir el progreso de los pesos y controlar el peso corporal en la fase de volumen.

## Funciones

- **Entrenar**: rutina del día cargada; apuntas kg y repeticiones por serie. Muestra lo que hiciste la última vez y sugiere el peso de hoy (si completaste todas las series → sube; si no → mantén). Temporizador de descanso con aviso sonoro.
- **Progreso**: gráfica por ejercicio con peso máximo y 1RM estimado (fórmula de Epley), % de mejora y volumen semanal total.
- **Peso**: registro del peso corporal, cambio total y ritmo de las últimas 4 semanas (kg/semana).
- **Calorías**: comidas del día (las 6 del plan como botones rápidos + comidas propias), proteína, actividad en casa y en el trabajo, y **balance comido − gastado**. Avisa si gastas más de lo que comes y sugiere qué comida añadir para compensar.
- **Historial**: editar o borrar sesiones, descargar/importar copias de seguridad en JSON (incluye rutina, calorías, actividad, peso y ajustes).
- **Ajustes**: perfil (para calcular el gasto), editor de rutina (cambiar, añadir, quitar y reordenar ejercicios) y comidas favoritas.
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
