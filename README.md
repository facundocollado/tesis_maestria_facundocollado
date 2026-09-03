# Algoritmos automatizados para el análisis textual de búsquedas laborales en la Web

Repositorio de código y datos asociados a la tesis de **Maestría en Estadística Aplicada** de **Facundo José Collado**.

Se comparan cuatro enfoques de extracción automática de frases clave (*keyphrases*) aplicados a descripciones de empleo publicadas en [Indeed.com](https://www.indeed.com), con el objetivo de identificar competencias, herramientas y requisitos recurrentes en el mercado laboral del área de datos.

## Contenido del repositorio

| Carpeta / archivo | Descripción |
|---|---|
| [`data/indeed_job_dataset.csv`](data/indeed_job_dataset.csv) | Dataset original de avisos laborales de Indeed (~5.700 registros) |
| [`notebooks/01_preprocesamiento/`](notebooks/01_preprocesamiento/) | Limpieza y normalización del texto de las descripciones (incluye `SmartStoplist.txt`) |
| [`notebooks/02_estadistica_descriptiva/`](notebooks/02_estadistica_descriptiva/) | Análisis estadístico descriptivo del dataset |
| [`notebooks/03_extraccion_keyphrases/`](notebooks/03_extraccion_keyphrases/) | Implementación y evaluación de los cuatro algoritmos |
| [`requirements.txt`](requirements.txt) | Dependencias de Python |
| [`LICENSE`](LICENSE) | Licencia GNU GPL v3 |

## Estructura del proyecto

```
tesis_maestria_facundocollado/
├── README.md
├── LICENSE
├── requirements.txt
├── .gitignore
├── data/
│   └── indeed_job_dataset.csv
└── notebooks/
    ├── 01_preprocesamiento/
    │   ├── depuracion_datos.ipynb
    │   └── SmartStoplist.txt
    ├── 02_estadistica_descriptiva/
    │   └── estadistica_descriptiva.ipynb
    └── 03_extraccion_keyphrases/
        ├── extraccion_keybert.ipynb
        ├── extraccion_rake.ipynb
        ├── extraccion_ngram_ranking.ipynb
        └── extraccion_bertopic.ipynb
```

## Flujo de trabajo

Los notebooks siguen el orden numérico de las carpetas:

1. **Preprocesamiento** — Se depura el campo `Description`: eliminación de HTML, URLs, stopwords (NLTK y SmartStoplist), lematización y filtrado de textos con menos de 30 palabras. El resultado es `indeed_job_dataset_clean.csv` (~5.332 avisos).
2. **Estadística descriptiva** — Caracterización del dataset: perfiles laborales, habilidades declaradas, distribuciones geográficas e industriales.
3. **Extracción de keyphrases** — Aplicación comparativa de cuatro métodos sobre el corpus depurado.

## Algoritmos comparados

| Algoritmo | Enfoque | Notebook |
|---|---|---|
| **RAKE** | Estadístico: co-ocurrencia de palabras y grado de separación | [`extraccion_rake.ipynb`](notebooks/03_extraccion_keyphrases/extraccion_rake.ipynb) |
| **N-gram ranking** | Frecuencia de n-gramas con ranking y traducción opcional | [`extraccion_ngram_ranking.ipynb`](notebooks/03_extraccion_keyphrases/extraccion_ngram_ranking.ipynb) |
| **KeyBERT** | Embeddings BERT para extracción semántica de frases | [`extraccion_keybert.ipynb`](notebooks/03_extraccion_keyphrases/extraccion_keybert.ipynb) |
| **BERTopic** | Modelado de tópicos con embeddings y reducción dimensional | [`extraccion_bertopic.ipynb`](notebooks/03_extraccion_keyphrases/extraccion_bertopic.ipynb) |

## Dataset

El archivo [`data/indeed_job_dataset.csv`](data/indeed_job_dataset.csv) contiene avisos de empleo del sector de datos (data scientist, data engineer, data analyst, etc.) con variables como:

- `Job_Title`, `Description`, `Company`, `Location`
- `Skill`, `No_of_Skills`, `Job_Type`, `Queried_Salary`
- Indicadores binarios de habilidades (`python`, `sql`, `machine learning`, `r`, `hadoop`, etc.)
- Indicadores geográficos (`CA`, `NY`, `TX`, …) e industriales

> **Nota:** El dataset fue recopilado desde Indeed.com con fines académicos. Su uso está sujeto a los términos de servicio de la plataforma.

## Requisitos y ejecución

Los notebooks fueron desarrollados originalmente en **Google Colab**. Para ejecutarlos localmente o en Colab:

```bash
pip install -r requirements.txt
python -m nltk.downloader stopwords wordnet averaged_perceptron_tagger punkt
```

### Ajuste de rutas

Los notebooks referencian rutas de Google Drive (`/content/drive/MyDrive/Colab Notebooks/...`). Al ejecutarlos localmente, actualizar las rutas de lectura del CSV, por ejemplo:

```python
df = pd.read_csv("../../data/indeed_job_dataset.csv")
```

### Archivos auxiliares

El notebook de preprocesamiento utiliza [`SmartStoplist.txt`](notebooks/01_preprocesamiento/SmartStoplist.txt), lista de stopwords del proyecto SMART (Salton, 1971) usada también por RAKE. Está incluida en `notebooks/01_preprocesamiento/`. Al ejecutar localmente, ajustar la ruta en el notebook:

```python
my_file = open("SmartStoplist.txt", "r")  # misma carpeta que el notebook
```

## Observaciones técnicas

- **KeyBERT** presenta limitaciones de memoria RAM al procesar corpus extensos (>1.000 documentos); se documentan en el notebook correspondiente.
- **RAKE** y **n-gram ranking** son más eficientes en tiempo y recursos para el volumen completo del dataset.
- **BERTopic** requiere GPU para tiempos de ejecución razonables en Colab.

## Autor

**Facundo José Collado**  
Maestría en Estadística Aplicada

## Licencia

Este proyecto se distribuye bajo la [GNU General Public License v3.0](LICENSE).
