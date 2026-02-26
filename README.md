# 🎮 Proyecto Integrador – Escenario Procedural con Animación en Blender

## Información General

**Materia:** Graficación / Modelado Procedural  
**Unidad:** 1  
**Software utilizado:** Blender  
**Lenguaje:** Python (bpy)

---

## Descripción del Proyecto

Este proyecto consiste en la generación automática de una pista procedural dentro de Blender utilizando programación en Python mediante la API `bpy`.

El escenario está compuesto por:

- ✔ Dos tramos rectos  
- ✔ Dos curvas de 90 grados  
- ✔ Paredes con materiales alternados  
- ✔ Piso generado como malla personalizada  
- ✔ Cámara animada recorriendo todo el trayecto  
- ✔ Movimiento suavizado mediante interpolación Bézier  

El objetivo principal fue aplicar conceptos de:

- Trigonometría
- Vectores
- Generación procedural
- Animación con keyframes
- Automatización de escenas 3D

---

# ⚙️ Funcionamiento del Código

---

## 1. Limpieza de la Escena

Antes de generar la pista, el script elimina todos los objetos existentes para evitar duplicaciones.

```python
bpy.ops.object.select_all(action='SELECT')
bpy.ops.object.delete()
```

Esto garantiza que cada vez que se ejecute el script, el escenario se cree desde cero.

---

## 2. Creación de Materiales

Se definen tres materiales:

- ParedOscura
- ParedDetalle
- SueloBlanco

```python
def crear_material(nombre, color_rgb):
    if nombre in bpy.data.materials:
        return bpy.data.materials[nombre]
    mat = bpy.data.materials.new(name=nombre)
    mat.diffuse_color = (*color_rgb, 1.0)
    return mat
```

Permite reutilizar materiales si ya existen y evita crear duplicados innecesarios.

---

## 3. Parámetros Geométricos

El comportamiento de la pista se controla mediante variables:

```python
ancho_pasillo = 4
separacion = 2
largo_recto = 10
radio = 8
segmentos_curva = 25
```

Estas variables determinan:

- El ancho total del pasillo
- La longitud de los tramos rectos
- El tamaño de las curvas
- La suavidad del arco

Entre mayor sea `segmentos_curva`, más suave será la curva.

---

## 4. Generación del Primer Tramo Recto

Se utiliza un ciclo `for` para generar bloques en línea recta.

```python
for i in range(largo_recto):
    y = i * separacion
    puntos_centro.append((0, y))
```

Se crean dos cubos por cada punto central (izquierda y derecha).  
Los materiales se alternan para dar un efecto visual dinámico.

---

## 5. Generación de Curvas (Trigonometría)

Las curvas se generan usando funciones trigonométricas:

```python
ang = (i / segmentos_curva) * (math.pi / 2)
x = centro_x + radio * math.cos(ang)
y = centro_y + radio * math.sin(ang)
```

Se recorre un cuarto de circunferencia (π/2), calculando posiciones con seno y coseno.  
Los cubos se rotan para alinearse con la dirección del arco.

---

## 6. Segundo Recto y Segunda Curva

Se repite el proceso anterior cambiando la dirección del eje.

Esto genera una pista con forma tipo "S", almacenando todos los puntos en la lista:

```python
puntos_centro
```

Esta lista es fundamental para generar el piso y animar la cámara.

---

## 7. Generación Procedural del Piso

El piso se crea como una sola malla personalizada.

```python
mesh.from_pydata(verts, [], faces)
```

Proceso matemático:

1. Se toman dos puntos consecutivos.
2. Se calcula el vector dirección.
3. Se obtiene el vector normal (perpendicular).
4. Se crean 4 vértices.
5. Se forma una cara cuadrilateral.

Ventajas:

- Mayor rendimiento
- Menor consumo de memoria
- Sin uniones visibles

---

## 8. Creación de la Cámara

```python
cam_data = bpy.data.cameras.new("CamaraPista")
cam_data.lens = 50
```

Configuración:

```python
fps = 24
duracion = 4
total_frames = fps * duracion
```

Esto garantiza que la animación dure exactamente 4 segundos.

---

## 9. Animación de la Cámara

La cámara recorre todos los puntos del centro del camino.

```python
ang = math.atan2(nx - x, ny - y)
cam.location = (x, y, cam_z)
cam.rotation_euler = (math.radians(90), 0, ang)
```

`atan2` permite que la cámara siempre mire hacia el siguiente punto.

Se insertan los keyframes:

```python
cam.keyframe_insert("location", frame=frame)
cam.keyframe_insert("rotation_euler", frame=frame)
```

---

## 10. Suavizado de Movimiento

Para evitar movimiento robótico:

```python
kp.interpolation = 'BEZIER'
```

Esto genera:

- Aceleración progresiva
- Desaceleración suave
- Movimiento más natural

---

# 🖥️ Resultado Final

El script genera automáticamente:

- Pista con rectas y curvas
- Paredes alternando colores
- Piso continuo procedural
- Cámara animada recorriendo todo el trayecto

---

# Requisitos

- Blender 3.x o superior
- Ejecutar el script desde el editor de texto de Blender

---

# Cómo Ejecutar el Proyecto

1. Abrir Blender.
2. Ir a la pestaña "Scripting".
3. Crear un nuevo archivo de texto.
4. Pegar el código completo.
5. Presionar **Run Script**.

La pista y la animación se generarán automáticamente.

---

# Conclusión

Este proyecto demuestra cómo la programación puede automatizar la creación de entornos 3D complejos.

En lugar de modelar manualmente cada elemento, el sistema genera:

- Geometría
- Materiales
- Trayectorias
- Animación

Todo mediante reglas matemáticas y estructuras repetitivas.

Se integraron conceptos de:

- Trigonometría
- Vectores
- Modelado procedural
- Cinemática básica
- Interpolación Bézier

Mostrando así el potencial de la generación procedural dentro de Blender.
<img width="659" height="256" alt="image" src="https://github.com/user-attachments/assets/faf8faa8-f807-43af-b31d-f06f83b64d1c" />
<img width="1242" height="606" alt="image" src="https://github.com/user-attachments/assets/f4fc8afc-6925-47f8-bf33-ddbb8e4b8112" />
