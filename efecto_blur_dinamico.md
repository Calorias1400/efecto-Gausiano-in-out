# Efecto de Blur Dinámico para After Effects (.mogrt)

## Configuración Inicial

### 1. Preparar la Composición
1. Crea una nueva composición en After Effects
2. Importa o crea una capa de referencia (puede ser un sólido de color)
3. Aplica el efecto **Gaussian Blur** a la capa

### 2. Configurar los Controles en Essential Graphics

Antes de aplicar la expresión, necesitas crear los sliders que serán expuestos en Premiere Pro:

1. Ve al panel **Essential Graphics**
2. Selecciona tu composición
3. Crea los siguientes controles:

#### Control 1: Intensidad de Inicio
- **Nombre**: "Intensidad_Inicio"
- **Tipo**: Slider
- **Valor por defecto**: 40
- **Rango**: 0 - 100

#### Control 2: Duración de Inicio
- **Nombre**: "Duracion_Inicio_Frames"
- **Tipo**: Slider  
- **Valor por defecto**: 4
- **Rango**: 1 - 30

#### Control 3: Intensidad de Fin
- **Nombre**: "Intensidad_Fin"
- **Tipo**: Slider
- **Valor por defecto**: 40
- **Rango**: 0 - 100

#### Control 4: Duración de Fin
- **Nombre**: "Duracion_Fin_Frames"
- **Tipo**: Slider
- **Valor por defecto**: 4
- **Rango**: 1 - 30

## Código de Expresión

Aplica esta expresión al parámetro **Blurriness** del efecto Gaussian Blur:

```javascript
// === EXPRESIÓN PARA BLUR DINÁMICO CON CONTROLES INDEPENDIENTES ===

// Obtener controles del Essential Graphics Panel
// Asegúrate de que estos nombres coincidan exactamente con los sliders creados
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
    // Progreso de 0 a 1 durante la entrada
    var progreso = tiempoActual / tiempoInicio;
    
    // Aplicar easing suave y calcular blur (de intensidadInicio a 0)
    var easeProgreso = ease(progreso, 0, 1);
    valorBlur = intensidadInicio * (1 - easeProgreso);
}

// FASE MEDIA (sin blur)
else if (tiempoActual > tiempoInicio && tiempoActual < inicioSalida) {
    valorBlur = 0;
}

// FASE DE SALIDA (últimos frames)  
else if (tiempoActual >= inicioSalida) {
    // Progreso de 0 a 1 durante la salida
    var progreso = (tiempoActual - inicioSalida) / tiempoFin;
    
    // Aplicar easing suave y calcular blur (de 0 a intensidadFin)
    var easeProgreso = ease(progreso, 0, 1);
    valorBlur = intensidadFin * easeProgreso;
}

// Retornar el valor final del blur
valorBlur;
```

## Instrucciones de Aplicación

### Paso 1: Aplicar la Expresión
1. Selecciona la capa con el efecto Gaussian Blur
2. Expande el efecto en el panel Timeline
3. **Alt + Click** en el cronómetro del parámetro **Blurriness**
4. Pega el código de expresión completo
5. Presiona **Enter** para aplicar

### Paso 2: Vincular Controles (Importante)
Después de aplicar la expresión, debes vincular los sliders creados:

1. En el panel **Essential Graphics**, busca cada slider creado
2. **Click derecho** en cada slider → **Edit Properties**
3. En **Source**, selecciona la composición actual
4. Vincula cada slider a su control correspondiente en la composición

### Paso 3: Probar el Efecto
1. Cambia la duración de tu composición para probar la adaptabilidad
2. Ajusta los valores de los sliders para verificar que funcionan
3. Reproduce la composición para ver las transiciones suaves

## Exportar como .mogrt

1. Selecciona tu composición en el panel **Project**
2. Ve a **File** → **Export** → **Motion Graphics Template**
3. Configura las opciones de exportación
4. Guarda el archivo .mogrt

## Uso en Premiere Pro

1. Importa el archivo .mogrt en Premiere Pro
2. Arrastra el efecto sobre cualquier clip (imagen, video, etc.)
3. Ajusta los controles en el panel **Essential Graphics**:
   - **Intensidad_Inicio**: Intensidad del blur al comenzar el clip
   - **Duracion_Inicio_Frames**: Duración en frames de la transición de entrada
   - **Intensidad_Fin**: Intensidad del blur al finalizar el clip
   - **Duracion_Fin_Frames**: Duración en frames de la transición de salida

## Características del Efecto

✅ **Adaptación automática**: Se ajusta a cualquier duración de clip
✅ **Transiciones suaves**: Usa `ease()` para movimientos naturales  
✅ **Controles independientes**: Control separado de inicio y final
✅ **Controles dinámicos**: 4 sliders personalizables en Premiere Pro
✅ **Sin keyframes**: Completamente controlado por expresiones
✅ **Reutilizable**: Funciona en cualquier tipo de media (foto, video, etc.)
✅ **Máxima flexibilidad**: Intensidades y duraciones diferentes para entrada y salida

## Ejemplos de Uso Creativo

### Efecto Asimétrico
- **Intensidad_Inicio**: 60 (blur fuerte al inicio)
- **Intensidad_Fin**: 20 (blur suave al final)
- **Duracion_Inicio_Frames**: 6 (transición lenta de entrada)
- **Duracion_Fin_Frames**: 2 (transición rápida de salida)

### Efecto Solo de Entrada
- **Intensidad_Inicio**: 40
- **Intensidad_Fin**: 0 (sin blur al final)
- **Duracion_Inicio_Frames**: 8
- **Duracion_Fin_Frames**: 1

### Efecto de Enfoque Dramático
- **Intensidad_Inicio**: 80 (muy desenfocado)
- **Intensidad_Fin**: 100 (máximo blur al final)
- **Duracion_Inicio_Frames**: 12 (transición muy lenta)
- **Duracion_Fin_Frames**: 8

## Solución de Problemas

### Error: "Effect is undefined"
- Verifica que los nombres de los sliders coincidan exactamente con los de la expresión
- Asegúrate de haber creado todos los controles en Essential Graphics

### El efecto no se anima
- Confirma que la expresión está aplicada al parámetro correcto (Blurriness)
- Verifica que los valores de duración no sean mayores que la duración total del clip

### Los controles no aparecen en Premiere
- Revisa que los sliders estén correctamente vinculados en Essential Graphics
- Confirma que el .mogrt se exportó correctamente