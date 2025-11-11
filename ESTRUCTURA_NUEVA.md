# 🎨 Galería Interactiva - Estructura Nueva con Overlays Anidados

## 📋 Cambios Realizados

Has pedido que **cada día de la Semana Santa sea un overlay** que contenga sus imágenes con textos. Así que hemos restructurado todo:

---

## 🏗️ Estructura HTML Antigua vs Nueva

### **ANTES (Estructura Plana)**
```html
<div class="domingo de ramos">
    <h1>Domingo de Ramos</h1>
    <h2>La Borriquita</h2>
    <img src="...">
    <h2>La Santa Cena</h2>
    <img src="...">
    <!-- Etc... -->
</div>
```
❌ Imágenes sueltas, sin estructura clara

---

### **AHORA (Estructura Jerárquica con Overlays)**
```html
<div class="day-card">
    <!-- Encabezado visible siempre -->
    <div class="day-header">
        <h3>Domingo de Ramos</h3>
        <p>Hermandades procesionantes</p>
    </div>
    
    <!-- OVERLAY DEL DÍA: Aparece al pasar el ratón -->
    <div class="day-overlay">
        <!-- Galería interna -->
        <div class="gallery-grid">
            <!-- ITEMS DE IMAGEN: Cada una con su overlay -->
            <div class="image-item">
                <img src="imagenes/borriquita.jpg">
                <!-- OVERLAY DE LA IMAGEN: Aparece al pasar ratón -->
                <div class="image-overlay">
                    <p>La Borriquita</p>
                </div>
            </div>
            <!-- Más items... -->
        </div>
    </div>
</div>
```

✅ **Dos niveles de overlays anidados:**
1. **Day Overlay** → Aparece al pasar ratón sobre el día
2. **Image Overlay** → Aparece al pasar ratón sobre la imagen

---

## 🎯 El Patrón: Dos Niveles de Position Relative/Absolute

### **Nivel 1: Day Card**
```scss
.day-card {
    position: relative;  // ← Ancla para .day-overlay
    
    .day-overlay {
        position: absolute;
        top: 0; left: 0; right: 0; bottom: 0;
        opacity: 0;  // Oculto
    }
    
    &:hover .day-overlay {
        opacity: 1;  // Aparece al hover
    }
}
```

**¿Por qué `position: relative` en `.day-card`?**
- Sin ella, el `position: absolute` se posiciona respecto a `<body>`
- Con ella, el overlay aparece dentro de la tarjeta, no en la esquina de la página

---

### **Nivel 2: Image Item**
```scss
.image-item {
    position: relative;  // ← Ancla para .image-overlay
    
    img { }
    
    .image-overlay {
        position: absolute;
        top: 0; left: 0; right: 0; bottom: 0;
        opacity: 0;  // Oculto
    }
    
    &:hover .image-overlay {
        opacity: 1;  // Aparece al hover
    }
}
```

**¿Por qué otro `position: relative`?**
- Las imágenes dentro del `.day-overlay` necesitan su propio contexto
- Así cada overlay de imagen se posiciona respecto a su imagen, no a toda la página

---

## 📊 Comparación: Antes vs Después

| Aspecto | ANTES | AHORA |
|---------|-------|-------|
| **Estructura HTML** | Plana, divs con clases extrañas | Jerárquica, clara |
| **Overlays** | Ninguno al nivel del día | Day overlay + Image overlay |
| **Responsividad** | JavaScript complexo | CSS puro, simpler |
| **Usabilidad** | Difícil de entender | Intuitiva: hover → ve las imágenes |

---

## 🎓 Conceptos Clave de Este Patrón

### **1. Position Relative "Ancla" el Absolute**
```scss
// ❌ MALO: Sin position: relative
.container {
    // Sin position: relative
    .overlay {
        position: absolute;  // ¿Respecto a quién? ¡Respecto a <body>!
    }
}

// ✅ BIEN: Con position: relative
.container {
    position: relative;      // ← El "contexto de posicionamiento"
    .overlay {
        position: absolute;  // ← Se posiciona respecto a .container
    }
}
```

### **2. Overlay Oculto por Defecto**
```scss
.day-overlay {
    opacity: 0;              // Invisible
    visibility: hidden;      // (Opcional) No ocupa espacio del flujo
    pointer-events: none;    // (Opcional) No interfiere con clics
}

.day-card:hover .day-overlay {
    opacity: 1;              // Visible
    visibility: visible;
    pointer-events: auto;
}
```

### **3. Transition Suave**
```scss
.day-overlay {
    opacity: 0;
    transition: opacity 0.3s ease-in-out;  // ← La magia
}

.day-card:hover .day-overlay {
    opacity: 1;  // No aparece de golpe, sino suavemente
}
```

---

## 🔧 Archivos Modificados

### **`index.html`** 
- ✅ Nueva estructura con `.day-card` y `.day-overlay`
- ✅ Grid interno con `.gallery-grid` y `.image-item`
- ✅ Cada imagen con su `.image-overlay`

### **`_gallery.scss`**
- ✅ Estilos para `.day-card` y `.day-overlay`
- ✅ Estilos para `.image-item` y `.image-overlay`
- ✅ Dos niveles de overlays anidados
- ✅ Comentarios explicativos en cada sección

### **`main.scss`**
- ✅ Importa módulos SASS limpios
- ✅ Estilos globales (header, footer, body)
- ✅ Media queries para responsividad

### **`_variables.scss`**
- ✅ Variables SASS reutilizables
- ✅ Colores overlay, fuentes, transiciones

---

## 🎨 Cómo Funciona Visualmente

```
┌─────────────────────────────────────┐
│  .day-card (position: relative)     │
├─────────────────────────────────────┤
│  .day-header (siempre visible)      │ ← Título y descripción del día
│  "Domingo de Ramos"                 │
├─────────────────────────────────────┤
│  .day-overlay (position: absolute)  │ ← APARECE al hover
│  (opacity: 0 → 1)                   │
│  ┌─────────────────────────────────┐│
│  │ .gallery-grid                   ││ ← Grid de imágenes
│  │ ┌──────┬──────┬──────┬──────┐  ││
│  │ │Img   │Img   │Img   │Img   │  ││ ← .image-item (pos: rel)
│  │ │┌────┐│┌────┐│┌────┐│┌────┐│  ││
│  │ ││Ovr ││ │Ovr ││ │Ovr ││ │Ovr ││  ││ ← .image-overlay (pos: abs)
│  │ ││    ││ │    ││ │    ││ │    ││  ││
│  │ │└────┘│└────┘│└────┘│└────┘│  ││
│  │ └──────┴──────┴──────┴──────┘  ││
│  └─────────────────────────────────┘│
└─────────────────────────────────────┘
```

---

## 🚀 Testing: Qué deberías ver

1. **Abre `index.html` en el navegador**
2. **Pasa el ratón sobre un día**
   - ✅ El encabezado se oscurece
   - ✅ Aparece un overlay semitransparente
   - ✅ Dentro se ven las imágenes del día
3. **Pasa el ratón sobre una imagen**
   - ✅ La imagen se oscurece un poco
   - ✅ Aparece el nombre de la hermandad
4. **Alejas el ratón**
   - ✅ Todo vuelve a la normalidad suavemente

---

## 💡 Aprendizaje: Preguntas de Reflexión

1. **¿Por qué hay `position: relative` en `.day-card` Y en `.image-item`?**
   - Porque cada uno necesita servir de "contexto" para su overlay absoluto

2. **¿Qué pasa si quito `position: relative` de `.day-card`?**
   - El `.day-overlay` se posicionaría respecto a `<body>`, apareciendo en la esquina superior izquierda

3. **¿Por qué usar `opacity` en vez de `display: none`?**
   - `opacity` permite transiciones suaves. `display: none` aparece/desaparece bruscamente

4. **¿Podría hacer un tercer nivel de overlay (dentro de `.image-overlay`)?**
   - Sí, el patrón es repetible. Solo necesitarías un `position: relative` más

---

## 📚 Conceptos Avanzados (Bonus)

### **Mixin SASS para Centrar Contenido**
```scss
@mixin flex-center {
    display: flex;
    align-items: center;
    justify-content: center;
}

// Uso:
.image-overlay {
    @include flex-center;
}
```

### **Variable para Z-Index (si hay capas)**
```scss
$z-overlay-image: 10;
$z-overlay-day: 20;

.image-overlay {
    z-index: $z-overlay-image;
}

.day-overlay {
    z-index: $z-overlay-day;
}
```

### **Pseudo-elemento para Degradado**
```scss
.day-overlay::before {
    content: "";
    position: absolute;
    inset: 0;
    background: linear-gradient(180deg, rgba(0,0,0,0.5) 0%, rgba(0,0,0,0.8) 100%);
    z-index: -1;
}
```

---

## 🎯 Resumen del Aprendizaje

✅ **Has aprendido:**
- Dos niveles de overlays anidados
- Cómo `position: relative` "ancla" `position: absolute`
- Transiciones suaves con `opacity`
- Estructura HTML jerárquica y clara
- Modularidad SASS (variables, anidamiento)

❓ **Pregunta clave:** Sin `position: relative` en el padre, ¿dónde aparecería el overlay?

---

**¡Ahora compila SASS y disfruta la galería!** 🎨✨
