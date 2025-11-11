# 🎨 Galería Interactiva Z-Masters - Guía de Aprendizaje

## � ¡IMPORTANTE! Cambio Estructura

Hemos restructurado todo para que **cada día sea un overlay anidado** con imágenes dentro.

**Lee esto primero:** [`ESTRUCTURA_NUEVA.md`](./ESTRUCTURA_NUEVA.md)

---

## �📚 Estructura del Proyecto

```
galeria-interactiva/
├── index-simple.html          # ← OPCIÓN A: Galería simple (aprendizaje puro)
├── index.html                 # ← OPCIÓN B: Proyecto completo (hermandades)
├── main.scss                  # ← Punto de entrada (importa los módulos)
├── styles-simple.scss         # ← Estilos para index-simple.html
├── _variables.scss            # ← Variables SASS (colores, fuentes)
├── _gallery.scss              # ← Estilos de la galería (con anidamiento)
├── imagenes/                  # ← Carpeta con imágenes
└── README.md                  # ← Este archivo
```

---

## 🎯 ¿POR QUÉ ESTA ESTRUCTURA?

### **Variables SASS** (`_variables.scss`)
Define valores reutilizables:
- `$overlay-bg-color: rgba(0, 0, 0, 0.7);` - Color del overlay
- `$overlay-text-color: #ffffff;` - Color del texto
- `$transition-speed: 0.3s;` - Velocidad de animaciones

**Ventaja:** Si cambias el overlay a rojo, solo cambias UNA variable y afecta a TODO el proyecto.

### **Módulo Galería** (`_gallery.scss`)
Contiene TODOS los estilos de las tarjetas y overlays, anidados correctamente:
```scss
.gallery-container {
    .gallery-card {
        .card-overlay {
            h3 { /* solo aquí existe h3 */ }
            p  { /* solo aquí existe p */ }
        }
    }
}
```

**Ventaja:** No repites selectores, el código es más limpio y fácil de mantener.

### **Main** (`main.scss`)
Importa los módulos y añade estilos globales:
```scss
@use 'variables' as *;
@use 'gallery';
```

---

## 🔑 CONCEPTOS CLAVE: position: relative vs absolute

### El Problema
Quieres un overlay que **flote encima** de la imagen sin empujar el contenido.

### La Solución

```scss
.gallery-card {
    position: relative;  // ← ESTO es la "ancla"
    
    .card-overlay {
        position: absolute;  // ← Flota respecto al padre
        top: 0;
        left: 0;
        right: 0;
        bottom: 0;  // Llena toda la tarjeta
        opacity: 0; // Oculto por defecto
    }
    
    &:hover .card-overlay {
        opacity: 1; // Aparece al pasar el ratón
    }
}
```

### ¿Por qué funciona?

| Propiedad | Efecto |
|-----------|--------|
| `position: relative` en `.gallery-card` | Establece un "contexto de posicionamiento". Dice: "Yo soy el contenedor padre, los hijos absolutos se posicionan respecto a mí" |
| `position: absolute` en `.card-overlay` | **Saca el elemento del flujo normal** (no empuja hermanos) y lo posiciona respecto al padre con `position: relative` |
| `top/left/right/bottom: 0` | Estira el overlay para llenar el padre |
| `opacity: 0 → 1` | Transición suave (aparece/desaparece) |

---

## 🎓 Autoevaluación: ¿Dónde me encuentro?

### **Nivel 1: "Explorador"** ❌
- [ ] He conseguido crear tarjetas con imágenes
- [ ] He usado SASS, pero todo está en `main.scss`
- [ ] Al hacer hover, el overlay empuja el contenido
- **Diagnóstico:** `position: absolute` sin `position: relative` no funciona

### **Nivel 2: "Aprendiz"** ⚠️
- [ ] El overlay flota encima (con `position: absolute`)
- [ ] He separado `_gallery.scss`, pero los selectores no están anidados
- [ ] El overlay se posiciona en la esquina superior izquierda de la página
- **Diagnóstico:** Me falta `position: relative` en el contenedor padre

### **Nivel 3: "Practicante"** ✅ (OBJETIVO)
- [ ] Trabajar perfectamente: overlay flota encima sin empujar nada
- [ ] Usado `position: relative` en `.gallery-card` y `position: absolute` en `.card-overlay`
- [ ] Separado SASS en `_variables.scss` y `_gallery.scss`
- [ ] Importado todo desde `main.scss`
- [ ] El overlay aparece suavemente con `opacity` al hacer hover
- **Diagnóstico:** He entendido la relación padre-hijo en posicionamiento y modularidad SASS

### **Nivel 4: "Maestro"** 🚀 (BONUS)
- [ ] Todo el Nivel 3 ✅
- [ ] Añadido `transition` suave a `opacity` para efecto visual
- [ ] Usado un **mixin SASS** para centrar el overlay (ej. Flexbox)
- [ ] Combinado `position`, `opacity` y `transition` para una experiencia pulida
- **Diagnóstico:** Domino SASS avanzado y posicionamiento CSS

---

## 🔧 Cómo Compilar SASS

Tienes dos opciones:

### **Opción 1: Compilar manualmente (VS Code Extension)**
1. Instala "Live Sass Compiler" (extensión VS Code)
2. Click derecho en `main.scss` → "Watch Sass"
3. Se genera `main.css` automáticamente

### **Opción 2: Compilar desde terminal**
```bash
sass main.scss main.css
sass styles-simple.scss styles-simple.css
```

---

## 📝 Ejercicios Propuestos

### **Ejercicio 1: Cambiar el color del overlay**
Edita `_variables.scss`:
```scss
$overlay-bg-color: rgba(255, 100, 100, 0.8);  // Rojo semi-transparente
```
✨ **Sin cambiar nada más, todo el proyecto usa este color**

### **Ejercicio 2: Hacer el overlay más lento**
Edita `_variables.scss`:
```scss
$transition-speed: 1s;  // Ahora la transición dura 1 segundo
```

### **Ejercicio 3: Crear un mixin SASS**
Añade a `_variables.scss`:
```scss
@mixin flex-center {
    display: flex;
    justify-content: center;
    align-items: center;
}
```

Luego en `_gallery.scss`, usa:
```scss
.card-overlay {
    @include flex-center;
}
```

---

## 🎨 OPCIÓN A vs OPCIÓN B

### **OPCIÓN A: `index-simple.html`**
- 4 tarjetas de ejemplo
- HTML limpio y simple
- Perfecta para **entender los conceptos** sin ruido
- Compila: `styles-simple.scss` → `styles-simple.css`

### **OPCIÓN B: `index.html`**
- Proyecto completo (hermandades sevillanas)
- HTML complejo con JavaScript
- Perfecta para ver **aplicación real** del aprendizaje
- Compila: `main.scss` → `main.css`

**Recomendación:** Empieza con **OPCIÓN A**, entiende bien los conceptos, luego aplica a **OPCIÓN B**.

---

## ❓ Preguntas de Autorreflexión

Responde honestamente estas preguntas. Si NO puedes contestar, repasa la teoría.

1. **¿Qué pasa si quito `position: relative` de `.gallery-card`?**
   - Respuesta: El overlay se posiciona respecto a `<body>`, apareciendo en la esquina superior izquierda.

2. **¿Por qué usar `position: absolute` en el overlay?**
   - Respuesta: Porque saca el elemento del flujo normal, evitando que empuje el contenido.

3. **¿Qué diferencia hay entre `opacity: 0` y `display: none`?**
   - Respuesta: `opacity: 0` = invisible pero ocupa espacio. `display: none` = no ocupa espacio (mejor para animaciones suaves).

4. **¿Por qué separar `_variables.scss`?**
   - Respuesta: Para reutilizar valores en múltiples archivos. Si cambio el color, cambia en TODO el proyecto.

5. **¿Cuál es la ventaja de anidar en SASS?**
   - Respuesta: No repito selectores, código más limpio y fácil de leer.

---

## 🚀 Próximos Pasos

Una vez domines esto:
- [ ] Aprende **Grid y Flexbox** (maquetación avanzada)
- [ ] Aprende **Media Queries** (diseño responsivo)
- [ ] Aprende **Z-index** (capas superpuestas)
- [ ] Aprende **Transform y keyframes** (animaciones)

---

## 📚 Recursos

- [MDN: position CSS](https://developer.mozilla.org/es/docs/Web/CSS/position)
- [MDN: SASS Nesting](https://developer.mozilla.org/es/docs/Web/CSS/Nesting)
- [SASS Oficial](https://sass-lang.com/)

---

**Creado para aprender, no para copiar. ¡A entender se ha dicho!** 💪
