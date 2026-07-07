# Avance - Proyecto Integrador - Extracción del Conocimiento en BD

Trabajos de la materia **Extracción del conocimiento en Bases de datos** (UTEZ, grupo 9C).

## Actividad Integradora (entrega en equipo)

Carpeta principal: [`Integradora_IA_Estudiantes/`](Integradora_IA_Estudiantes/)

Estudio bajo la metodología **KDD** sobre el dataset *AI Student Impact* (50,000 estudiantes,
Kaggle). Se construye un modelo supervisado que predice el **nivel de burnout** (Low / Medium /
High) a partir de los hábitos de estudio y de uso de IA generativa, comparando tres algoritmos:
**árbol de decisión, KNN y SVM**.

**Integrantes:**
- Arias Hernández José
- Bertadillo Villalobos Leobardo Daniel
- Paredes Domínguez Jassiel
- Aguilar García Ángel Gabriel
- Torres Galván Alejandro Aldahir

### Contenido de la carpeta

| Archivo | Descripción |
|---|---|
| `Integradora_IA_Estudiantes.ipynb` | Notebook con todo el desarrollo (ya ejecutado, con resultados). |
| `ai_student_impact_dataset.csv` | Dataset utilizado. |
| `latex/main.pdf` | **Reporte final en PDF.** |
| `latex/` | Código fuente LaTeX del reporte. |

### Cómo reproducirlo

Requiere Python con `scikit-learn`, `pandas`, `numpy`, `matplotlib` y `seaborn`.

```sh
cd Integradora_IA_Estudiantes
jupyter lab   # abrir y ejecutar Integradora_IA_Estudiantes.ipynb de arriba a abajo
```

El notebook se ejecuta de principio a fin sin cambios. La celda del SVM tarda unos minutos
porque hace una búsqueda de parámetros sobre una muestra de 10,000 ejemplos.

> Resultado principal: la exactitud ronda el 52% (frente al 33% del azar con tres clases). El
> burnout solo es parcialmente predecible con estas variables; el factor más determinante son
> las **horas semanales de uso de IA**.

## Ejercicios previos (Unidad 3, individuales)

Carpeta [`KNN_SVM/`](KNN_SVM/): actividades T2 (KNN y SVM sobre siluetas de vehículos) y T4
(optimización de modelos sobre Iris), cada una con su notebook y su reporte en PDF.
