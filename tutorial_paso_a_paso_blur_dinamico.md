# TUTORIAL PASO A PASO: Efecto Blur Dinámico para Reels (9:16)

## REQUISITOS PREVIOS
- After Effects 2020 o superior
- Conocimiento básico de After Effects
- 15 minutos de tiempo
- **FORMATO**: Vertical 9:16 para Instagram Reels, TikTok, YouTube Shorts

---

## PARTE 1: CONFIGURACIÓN INICIAL

### PASO 1: Crear Nueva Composición (Formato Vertical 9:16)
1. Abre **After Effects**
2. Click en **Composition** → **New Composition** (Ctrl+N)
3. En el diálogo que aparece:
   - **Preset**: Selecciona "Custom" (no hay preset 9:16 por defecto)
   - **Width**: 1080
   - **Height**: 1920
   - **Pixel Aspect Ratio**: Square Pixels
   - **Frame Rate**: 30 fps (ideal para reels)
   - **Duration**: 15:00 (15 segundos, típico para reels)
   - **Background Color**: Negro
4. Click **OK**
5. **Nombra tu composición**: "Blur_Reels_9x16"

### PASO 2: Crear Capa Base (Formato Vertical)
1. Click derecho en el **Panel Timeline** (área vacía)
2. Selecciona **New** → **Solid**
3. En el diálogo **Solid Settings**:
   - **Name**: "Base_Layer_Reels"
   - **Width**: 1080
   - **Height**: 1920
   - **Color**: Blanco (#FFFFFF)
4. Click **OK**

### PASO 3: Aplicar Efecto Gaussian Blur
1. Con la capa "Base_Layer_Reels" seleccionada
2. Ve al menú **Effect** → **Blur & Sharpen** → **Gaussian Blur**
3. Verás que aparece en el **Panel Effects Controls**
4. **NO toques nada aún**, solo verifica que esté aplicado

---

## PARTE 2: CREAR CONTROLES EN ESSENTIAL GRAPHICS

### PASO 4: Abrir Panel Essential Graphics
1. Ve a **Window** → **Essential Graphics** (si no está visible)
2. En el panel Essential Graphics, verás dos pestañas:
   - **Edit** (la que necesitamos)
   - **Browse**
3. Click en **Edit** si no está activa

### PASO 5: Crear Primer Control - Intensidad_Inicio
1. En **Essential Graphics Panel**, click en el ícono **+ Add Property**
2. Selecciona **Slider Control**
3. Se creará un nuevo slider, ahora configúralo:
   - **Name**: Cambia "Slider Control" por "Intensidad_Inicio"
   - **Value**: 40
   - **Min**: 0
   - **Max**: 100
4. Presiona **Enter** para confirmar el nombre

### PASO 6: Crear Segundo Control - Duracion_Inicio_Frames
1. Click nuevamente en **+ Add Property**
2. Selecciona **Slider Control**
3. Configura:
   - **Name**: "Duracion_Inicio_Frames"
   - **Value**: 4
   - **Min**: 1
   - **Max**: 30

### PASO 7: Crear Tercer Control - Intensidad_Fin
1. Click en **+ Add Property**
2. Selecciona **Slider Control**
3. Configura:
   - **Name**: "Intensidad_Fin"
   - **Value**: 40
   - **Min**: 0
   - **Max**: 100

### PASO 8: Crear Cuarto Control - Duracion_Fin_Frames
1. Click en **+ Add Property**
2. Selecciona **Slider Control**
3. Configura:
   - **Name**: "Duracion_Fin_Frames"
   - **Value**: 4
   - **Min**: 1
   - **Max**: 30

**VERIFICACIÓN**: Debes tener 4 sliders en el Essential Graphics Panel con los nombres exactos.

---

## PARTE 3: APLICAR LA EXPRESIÓN

### PASO 9: Preparar para Expresión
1. En el **Timeline Panel**, expande la capa "Base_Layer_Reels"
2. Expande **Effects**
3. Expande **Gaussian Blur**
4. Verás el parámetro **Blurriness** con valor por defecto

### PASO 10: Aplicar Expresión (CRÍTICO)
1. **Alt + Click** en el cronómetro (⏱️) junto a **Blurriness**
2. Se abrirá un campo de texto para expresión
3. **BORRA TODO** lo que esté ahí
4. **COPIA Y PEGA EXACTAMENTE** este código:

```javascript
// === EXPRESIÓN PARA BLUR DINÁMICO CON CONTROLES INDEPENDIENTES ===

// Obtener controles del Essential Graphics Panel
var intensidadInicio = effect("Intensidad_Inicio")("Slider");
var duracionInicioFrames = effect("Duracion_Inicio_Frames")("Slider");
var intensidadFin = effect("Intensidad_Fin")("Slider");
var duracionFinFrames = effect("Duracion_Fin_Frames")("Slider");

// Obtener información del tiempo
var duracionTotal = thisComp.duration;
var tiempoActual = time;
var fps = 1/thisComp.frameDuration;

// Convertir frames a tiempo
var tiempoInicio = duracionInicioFrames / fps;
var tiempoFin = duracionFinFrames / fps;

// Calcular puntos de transición
var inicioSalida = duracionTotal - tiempoFin;

// Variable para almacenar el valor del blur
var valorBlur = 0;

// FASE DE ENTRADA (primeros frames)
if (tiempoActual <= tiempoInicio) {
    var progreso = tiempoActual / tiempoInicio;
    var easeProgreso = ease(progreso, 0, 1);
    valorBlur = intensidadInicio * (1 - easeProgreso);
}

// FASE MEDIA (sin blur)
else if (tiempoActual > tiempoInicio && tiempoActual < inicioSalida) {
    valorBlur = 0;
}

// FASE DE SALIDA (últimos frames)  
else if (tiempoActual >= inicioSalida) {
    var progreso = (tiempoActual - inicioSalida) / tiempoFin;
    var easeProgreso = ease(progreso, 0, 1);
    valorBlur = intensidadFin * easeProgreso;
}

// Retornar el valor final del blur
valorBlur;
```

5. Presiona **Enter** para aplicar
6. **IMPORTANTE**: Si ves un mensaje de error, verifica que los nombres de los sliders coincidan EXACTAMENTE

### PASO 11: Vincular Expresión a Controles
1. En **Essential Graphics Panel**, click derecho en "Intensidad_Inicio"
2. Selecciona **Edit Properties**
3. En **Source**, debe aparecer tu composición "Blur_Reels_9x16"
4. Click en el dropdown y selecciona:
   - **Base_Layer_Reels** → **Effects** → **Intensidad_Inicio** → **Slider**
5. Click **OK**

**REPITE EL PASO 11 PARA LOS OTROS 3 SLIDERS:**
- Duracion_Inicio_Frames
- Intensidad_Fin  
- Duracion_Fin_Frames

---

## PARTE 4: PRUEBA DEL EFECTO

### PASO 12: Probar Funcionamiento
1. Mueve el **playhead** al frame 1
2. En Essential Graphics, cambia "Intensidad_Inicio" a 80
3. Presiona **espacio** para reproducir
4. **DEBES VER**:
   - Frames 1-4: Imagen muy borrosa que se va aclarando
   - Frames medios: Imagen nítida
   - Últimos 4 frames: Imagen se va difuminando

### PASO 13: Verificar Controles
Prueba cada slider y verifica:
- **Intensidad_Inicio**: Cambia blur inicial ✓
- **Duracion_Inicio_Frames**: Cambia duración de entrada ✓
- **Intensidad_Fin**: Cambia blur final ✓
- **Duracion_Fin_Frames**: Cambia duración de salida ✓

---

## PARTE 5: EXPORTAR COMO .MOGRT

### PASO 14: Preparar Exportación
1. **Guarda tu proyecto**: Ctrl+S
2. En **Project Panel**, selecciona la composición "Blur_Reels_9x16"
3. **Click derecho** → **Export as Motion Graphics Template**

### PASO 15: Configurar Exportación (Específico para Reels)
1. En el diálogo **Export as Motion Graphics Template**:
   - **Template Name**: "Blur_Reels_9x16_v1"
   - **Version**: 1.0.0
   - **Author**: Tu nombre
   - **Description**: "Efecto blur dinámico para Reels (9:16) - Instagram, TikTok, YouTube Shorts"
2. **Destination**: Elige dónde guardar
3. Click **OK**

### PASO 16: Verificar Archivo
1. Verifica que se creó el archivo con extensión **.mogrt**
2. El archivo debe tener aproximadamente 1-5 KB

---

## PARTE 6: USAR EN PREMIERE PRO

### PASO 17: Importar en Premiere
1. Abre **Premiere Pro**
2. Ve a **Graphics** → **Browse**
3. Click en **Install Motion Graphics Template**
4. Selecciona tu archivo .mogrt
5. Click **Open**

### PASO 18: Aplicar Efecto en Reels
1. Arrastra cualquier clip vertical (imagen, video 9:16) a la timeline
2. Ve a **Graphics** → **Browse**
3. Encuentra tu template "Blur_Reels_9x16_v1"
4. **Arrastra el template sobre tu clip de reel**
5. Ve al panel **Essential Graphics**
6. **¡VERÁS TUS 4 CONTROLES FUNCIONANDO EN FORMATO VERTICAL!**

---

## SOLUCIÓN DE PROBLEMAS ESPECÍFICOS

### ERROR: "Effect 'Intensidad_Inicio' is undefined"
**SOLUCIÓN**:
1. Verifica que el nombre del slider sea EXACTAMENTE "Intensidad_Inicio"
2. No debe tener espacios extra o caracteres especiales
3. Re-vincula el slider siguiendo el PASO 11

### ERROR: El efecto no se anima
**SOLUCIÓN**:
1. Verifica que aplicaste la expresión en **Blurriness** y NO en otro parámetro
2. Asegúrate de hacer **Alt + Click** en el cronómetro correcto
3. La expresión debe tener el texto en color rojo/marrón

### ERROR: Los controles no aparecen en Premiere
**SOLUCIÓN**:
1. Re-vincula TODOS los sliders (PASO 11)
2. Verifica que la exportación se completó sin errores
3. Re-instala el .mogrt en Premiere

### ERROR: El blur no cambia de intensidad
**SOLUCIÓN**:
1. Verifica que los valores Min/Max de los sliders sean correctos
2. Asegúrate de que los sliders estén correctamente vinculados
3. Comprueba que no hay keyframes manuales en Blurriness

---

## VENTAJAS ESPECÍFICAS PARA REELS

### ¿Por qué este efecto es perfecto para Reels?
- **Captura atención inmediata**: El blur inicial engancha al viewer
- **Transición suave**: Revela el contenido de forma elegante
- **Salida memorable**: El blur final crea un cierre profesional
- **Formato optimizado**: Diseñado específicamente para 9:16
- **Fácil aplicación**: Arrastra y suelta sobre cualquier clip vertical

### Casos de uso ideales:
- **Revelación de productos**: Perfecto para mostrar artículos
- **Transiciones entre clips**: Une diferentes tomas suavemente
- **Intro/outro dinámico**: Entrada y salida profesional
- **Destacar momentos**: Enfoca la atención en contenido clave

## TIPS FINALES

### Para Mejores Resultados en Reels:
- **Usa clips de 5-15 segundos** (duración típica de reels)
- **Frame Rate 30fps** es perfecto para plataformas sociales
- **Prueba con fotos y videos verticales** (9:16)
- **Guarda copias** de tu proyecto de After Effects
- **Valores recomendados para reels**:
  - Intensidad_Inicio: 30-50 (no muy agresivo)
  - Duracion_Inicio_Frames: 3-6 frames
  - Intensidad_Fin: 30-50 
  - Duracion_Fin_Frames: 3-6 frames

### Personalización Avanzada:
- Puedes cambiar los valores **Min/Max** de los sliders
- Puedes añadir más controles (color, dirección, etc.)
- Puedes modificar la función **ease()** por otras curvas

**¡EFECTO PARA REELS COMPLETADO!** 
Tu archivo .mogrt está listo para crear reels profesionales en Instagram, TikTok y YouTube Shorts con formato vertical 9:16.