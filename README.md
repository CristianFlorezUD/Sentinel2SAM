<div align="center">
  <img src="assets/OsGeolab.png" alt="Logo OsGeoLab" width="200"/>
  <img src="assets/Ud.png" alt="Logo Universidad Distrital" width="100"/>

  # Estudio de viabilidad en la delimitación de coberturas con imágenes satelitales mediante el modelo Segment Anything

  **Comparación con la metodología CORINE Land Cover en el municipio de Guatavita**

  [![Python](https://img.shields.io/badge/Python-3.8+-blue.svg)](https://www.python.org/)
  [![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-orange.svg)](https://jupyter.org/)

  **Autores:**
Cristian Stiven Florez Macias, Sergio Andres Escobar Eslava

  *Universidad Distrital Francisco José de Caldas*
</div>

---

## Tabla de Contenidos

- [Descripción](#-descripción)
- [Zona de Estudio](#-zona-de-estudio)
- [Estructura del Proyecto](#-estructura-del-proyecto)
- [Requisitos](#-requisitos)
- [Instalación](#-instalación)
- [Uso](#-uso)
- [Notebooks](#-notebooks)
- [Metodología](#-metodología)
- [Resultados](#-resultados)
- [Tecnologías](#-tecnologías)
- [Contribuciones](#-contribuciones)
- [Referencias](#-referencias)

---

## Descripción

Este proyecto evalúa la **viabilidad del modelo Segment Anything (SAM)** de Meta AI para la delimitación automática de coberturas terrestres utilizando imágenes satelitales Sentinel-2, comparando los resultados con la metodología tradicional **CORINE Land Cover** en el municipio de Guatavita, Cundinamarca, Colombia.

### Objetivos

- Implementar el modelo SAM para segmentación automática de coberturas terrestres
- Comparar resultados con la metodología CORINE Land Cover
- Evaluar diferentes estrategias de segmentación (automática, por puntos, por texto, por cuadrados)
- Generar mapas de cobertura terrestre de alta precisión

---

## Zona de Estudio

**Municipio de Guatavita, Cundinamarca, Colombia**

- **Ubicación:** Noreste de Bogotá
- **Superficie:** 252 km²
- **Altitud promedio:** 2700 msnm
- **Límites:**
  - Norte: Sesquilé y Machetá
  - Este: Gachetá
  - Sur: Guasca y Junín
  - Occidente: Gachancipá y Tocancipá

---

## Estructura del Proyecto

```
DEFINITIVO-Proyecto de grado/
│
├── NoteBooks/
│   ├── 1.descarga_imagen.ipynb          # Descarga y procesamiento de imágenes Sentinel-2
│   ├── 2.generacion_teselas.ipynb       # Generación de teselas para procesamiento
│   ├── 3.segmentacion_automatica.ipynb  # Segmentación automática con SAM
│   ├── 4.segmentacion_puntos.ipynb      # Segmentación guiada por puntos
│   ├── 5.segmentacion_texto.ipynb       # Segmentación guiada por texto
│   └── 6.segmentacion_cuadrado.ipynb    # Segmentación por cuadros delimitadores
│
├── assets/
│   ├── OsGeolab.png                     # Logo del semillero
│   ├── Ud.png                           # Logo Universidad Distrital
│   └── imageCollectionm.png             # Diagramas y visualizaciones
│
├── geojson/
│   ├── geoboundary.geojson              # Límites administrativos Colombia
│   ├── guatavita.geojson                # Área de estudio
│   └── landcoverLevel1.geojson          # Cobertura CORINE Land Cover
│
├── tiff/
│   ├── sentinel_guatavita.tif           # Imagen Sentinel-2 original
│   └── sentinel_guatavita_realzada.tif  # Imagen procesada
│
├── tiles/
│   ├── Sentinel/                        # Teselas de imágenes satelitales
│   └── LandUse/                         # Teselas de coberturas CORINE
│
└── README.md
```

---

## 🔧 Requisitos

### Software

- Python 3.8+
- Jupyter Notebook / JupyterLab
- Cuenta de Google Earth Engine

### Hardware Recomendado

- RAM: 16 GB (mínimo 8 GB)
- GPU: NVIDIA con CUDA (opcional, mejora rendimiento SAM)
- Almacenamiento: 10 GB libres

---

## 📦 Instalación

### 1. Clonar el repositorio

```bash
git clone https://github.com/tu-usuario/proyecto-sam-landcover.git
cd proyecto-sam-landcover
```

### 2. Crear entorno virtual

```bash
python -m venv venv
source venv/bin/activate  # En Windows: venv\Scripts\activate
```

### 3. Instalar dependencias

```bash
pip install -q geemap geopandas rasterio eeconvert tqdm pandas numpy shapely matplotlib scikit-image
```

### 4. Configurar Google Earth Engine

```python
import ee
ee.Authenticate()
ee.Initialize(project='tu-proyecto-gee')
```

---

## Uso

### Flujo de trabajo completo

1. **Descarga de imágenes Sentinel-2**
   ```bash
   jupyter notebook NoteBooks/1.descarga_imagen.ipynb
   ```

2. **Generación de teselas**
   ```bash
   jupyter notebook NoteBooks/2.generacion_teselas.ipynb
   ```

3. **Segmentación con SAM** (elegir método)
   ```bash
   jupyter notebook NoteBooks/3.segmentacion_automatica.ipynb
   ```

4. **Análisis de resultados**

---

## Notebooks

### 1. Descarga y Procesamiento de Imágenes

**`1.descarga_imagen.ipynb`**

- Autenticación en Google Earth Engine
- Descarga de imágenes Sentinel-2 (2018)
- Filtrado por cobertura de nubes (<10%)
- Generación de composición mediana
- Análisis de histogramas
- Realce de imágenes (opcional)

**Salidas:**
- `sentinel_guatavita.tif` - Imagen RGB original
- `sentinel_guatavita_realzada.tif` - Imagen procesada

---

### 2. Generación de Teselas

**`2.generacion_teselas.ipynb`**

- División de imagen en teselas de 256×256 px
- Generación de malla georreferenciada (GeoJSON)
- Recorte de imágenes raster por tesela
- Recorte de coberturas CORINE por tesela
- Eliminación de bordes vacíos con NumPy

**Salidas:**
- `gautavita.geojson` - Malla de teselas
- `tiles/Sentinel/Guatavita_*.tif` - Teselas de imagen
- `tiles/LandUse/Guatavita_*.geojson` - Teselas de cobertura

---

### 3. Segmentación Automática

**`3.segmentacion_automatica.ipynb`**

- Carga del modelo SAM
- Generación automática de máscaras
- Extracción de características geométricas
- Comparación con CORINE Land Cover

---

### 4. Segmentación por Puntos

**`4.segmentacion_puntos.ipynb`**

- Selección manual de puntos de interés
- Segmentación guiada por coordenadas
- Refinamiento de máscaras

---

### 5. Segmentación por Texto

**`5.segmentacion_texto.ipynb`**

- Integración con modelos de lenguaje
- Segmentación mediante descripciones textuales
- Clasificación semántica de coberturas

---

### 6. Segmentación por Cuadros

**`6.segmentacion_cuadrado.ipynb`**

- Definición de bounding boxes
- Segmentación por regiones de interés
- Análisis de precisión por área

---

## Metodología

### 1. Adquisición de Datos

- **Fuente:** Sentinel-2 Level-2A (Google Earth Engine)
- **Periodo:** 2018-01-01 a 2018-12-31
- **Resolución espacial:** 10 m
- **Bandas utilizadas:** B4 (Rojo), B3 (Verde), B2 (Azul)
- **Filtros:** Cobertura de nubes < 10%

### 2. Preprocesamiento

```python
# Composición mediana para reducir nubes
image = ee.ImageCollection('COPERNICUS/S2_SR_HARMONIZED')
    .filterBounds(region)
    .filterDate('2018-01-01', '2018-12-31')
    .filter(ee.Filter.lt("CLOUDY_PIXEL_PERCENTAGE", 10))
    .median()
```

### 3. Teselado

- Ventanas deslizantes de 256×256 px
- Solapamiento: 0 px (sin overlap)
- Total de teselas: Variable según área

### 4. Segmentación SAM

**Modelo:** Segment Anything (Meta AI) y SAM HQ

**Estrategias evaluadas:**
1. **Automática:** Generación completa de máscaras
2. **Por puntos:** Prompts de coordenadas
3. **Por texto:** Descripciones semánticas
4. **Por cuadros:** Prompts por cajas

### 5. Evaluación

**Métricas:**
- IoU (Intersection over Union)
- DICE
- TPR
- FPR
- Pixel Accuracy

---

## Resultados


### Productos Generados

- Mapa de coberturas terrestres segmentado automáticamente
- Comparación lado a lado: SAM vs. CORINE Land Cover
- Análisis de precisión por clase de cobertura
- Visualizaciones interactivas en Jupyter

---

## Tecnologías

### Librerías Python

| Librería | Versión | Uso |
|----------|---------|-----|
| **geemap** | Ultima | Integración Google Earth Engine |
| **geopandas** | Ultima | Manipulación de geometrías vectoriales |
| **rasterio** | Ultima | Lectura/escritura de datos raster |
| **numpy** | Ultima | Operaciones con arrays multidimensionales |
| **shapely** | Ultima | Geometrías y operaciones espaciales |
| **matplotlib** | Ultima | Visualización de datos |
| **tqdm** | Ultima | Barras de progreso |
| **scikit-image** | Ultima | Procesamiento de imágenes |

### Plataformas

- **Google Earth Engine:** Procesamiento en la nube de imágenes satelitales
- **Jupyter Notebook:** Entorno de desarrollo interactivo, con la capacidad de combinar ejecución por partes
- **Segment Anything (SAM):** Modelo de segmentación de Meta AI

---

## Características Destacadas

 **Procesamiento escalable** mediante teselado  
 **Múltiples estrategias de segmentación**  
**Integración con Google Earth Engine**  
**Comparación con metodología estándar (CORINE)**  
**Visualizaciones interactivas**  
**Código modular y reutilizable**  
**Documentación completa en notebooks**

---

## Contribuciones

Proyecto abierto a contribuciones. Por favor:

1. Clonar el proyecto
2. Crea una rama para el cambio (`git checkout -b feature/AmazingFeature`)
3. Commit con los cambios (`git commit -m 'Add: AmazingFeature'`)
4. Push a la rama (`git push origin feature/AmazingFeature`)
5. En gitHub abrir un pull request

---

## Referencias

### Datasets

- **Sentinel-2:** [Copernicus Open Access Hub](https://scihub.copernicus.eu/)
- **CORINE Land Cover:** [IDEAM Colombia](http://www.ideam.gov.co/)
- **geoBoundaries:** [geoBoundaries API](https://www.geoboundaries.org/)

### Modelos y Frameworks

- **Segment Anything (SAM):** [Meta AI Research](https://segment-anything.com/)
- **Google Earth Engine:** [Earth Engine Documentation](https://developers.google.com/earth-engine)

### Publicaciones Relacionadas

- Kirillov, A., et al. (2023). *Segment Anything*. arXiv:2304.02643
- Gorelick, N., et al. (2017). *Google Earth Engine: Planetary-scale geospatial analysis for everyone*. Remote Sensing of Environment

---

## Contacto

**Universidad Distrital Francisco José de Caldas**

- Bogotá, Colombia
- [www.udistrital.edu.co](https://www.udistrital.edu.co)

**Autores:**
Cristian Stiven Florez Macias
Sergio Andres Escobar Eslava

---

<div align="center">
  <p>Desarrollado en el semillero OsGeoLab</p>
  <p>Tutor Paulo Coronado</p>
  <p>Universidad Distrital Francisco José de Caldas | 2025</p>
</div>
