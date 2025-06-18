# TFM: XGBoost con Anonimización y Privacidad Diferencial para Detección de Fraude

## 👨‍💼 Información del Autor
**Desarrollador**: David González  
**Email**: dalexx07.28@gmail.com  
**Rol**: Data Engineer & XGBoost Specialist  
**Especialización**: XGBoost, Gradient Boosting, Optimización de Modelos, Privacidad Diferencial  
**Programa**: Máster en Análisis de Datos Masivos e Inteligencia Empresarial - UNIE  
**Fecha**: Abril 2025

## 🔗 Proyecto Grupal
Este repositorio forma parte del **TFM grupal sobre Anonimización de Datos y Cumplimiento GDPR en LLMs**.

**👥 Colaboradores del TFM**:
- **Armando Ita**: Random Forest - [🔗 Repositorio](https://github.com/iansilva2305/tfm_anonimizacion)
- **Alexis Mendoza**: Regresión Logística - [🔗 Repositorio](https://github.com/alexismendozac/TFM_Anonimizacion_RegresionLogistica/tree/main/notebooks)
- **David González**: XGBoost (este repositorio)

## 🎯 Enfoque Específico: XGBoost

### ¿Por qué XGBoost?
XGBoost (eXtreme Gradient Boosting) fue seleccionado por su **rendimiento superior en competiciones de machine learning** y su capacidad para detectar patrones complejos en datos financieros. Sin embargo, planteamos la hipótesis de que su dependencia en gradientes específicos podría hacerlo más vulnerable a las transformaciones de anonimización.

### Hipótesis de Trabajo
**"XGBoost, aunque potente, puede experimentar degradación significativa tras la anonimización debido a su sensibilidad a las relaciones específicas entre variables"**

### Resultados Obtenidos 📊
- **F1-Score Original**: 86.33%
- **F1-Score Anonimizado**: 66.43%
- **Impacto**: -19.90% (Degradación significativa)
- **Ranking**: 🥈 **2º lugar** entre los 3 modelos evaluados (post-anonimización)

### Lecciones Aprendidas
- XGBoost requiere **ajustes específicos** para trabajar con datos anonimizados
- La **privacidad diferencial** tiene mayor impacto en modelos de gradient boosting
- Es necesario **rebalancear hiperparámetros** tras aplicar k-anonimato

## 🏗️ Arquitectura de la Solución

### Pipeline de Procesamiento
```
📁 Dataset PaySim1 
    ↓
🔒 Seudonimización (SHA-256)
    ↓  
📊 K-Anonimato (k=10)
    ↓
🎯 L-Diversidad (l=2)
    ↓
🛡️ Privacidad Diferencial (ε=2.0)
    ↓
⚡ XGBoost Training
    ↓
📈 Evaluación GDPR
```

### Técnicas de Anonimización Implementadas

| Técnica | Parámetros | Aplicado a | Impacto en XGBoost |
|---------|------------|------------|-------------------|
| **Seudonimización** | SHA-256 | nameOrig, nameDest | ✅ Sin impacto directo |
| **K-Anonimato** | k=10 | amount, step | ⚠️ Afecta gradientes específicos |
| **L-Diversidad** | l=2 | type | ⚠️ Reduce patrones de boosting |
| **Privacidad Diferencial** | ε=2.0 | Proceso de entrenamiento | 🔴 Mayor impacto que en Random Forest |

## 📁 Estructura del Proyecto

```
TFM-XGBoost-Anonimization-GDPR/
├── src/                                    # Código fuente principal
│   ├── comparativa_modelos_XGBOOST.py     # Script principal de comparación
│   ├── eda_anonimizacion_fraud_detection.py # EDA específico para fraude
│   └── eda_anonimizacion_modular.py       # Análisis modular de anonimización
├── notebooks/                              # Análisis interactivos
│   ├── notebook_analisis_privacidad-utilidad.ipynb    # Trade-off privacidad-utilidad
│   ├── notebook_deteccion_fraude_basico.ipynb         # Detección básica de fraude
│   └── notebook_EDA_anonimizacion_LLMs.ipynb          # EDA con integración LLMs
├── dataset/                               #Datos 
│   ├── sample_paysim1.csv                 #Conjunto de datos 
├── results/                              #Resultados y métricas
│   ├── xgboost_metrics.json              #Métricas detalladas del modelo
│   ├── comparison_results.csv            #Comparación con otros modelos
│   └── privacy_impact_analysis.xlsx      #Análisis de impacto de privacidad
├── config/                               #Configuraciones
│   ├── xgboost_params.yaml            #Hiperparámetros optimizados
│   └── anonymization_config.json      #Configuración de anonimización
├── requirements.txt                   #Dependencias específicas
├── .gitignore                           #Archivos a ignorar
└── README.md                           #Este archivo
```

## 🚀 Instalación y Uso

### Prerrequisitos
- Python 3.10+
- XGBoost 1.7.0+
- Scikit-learn 1.3.0+

### Instalación

1. **Clonar el repositorio:**
```bash
git clone https://https://github.com/gonzalezvdavid/TFM-XGBoost-Anonimization-GDPR
cd TFM-XGBoost-Anonimization-GDPR
```

2. **Crear entorno virtual:**
```bash
python -m venv xgboost_env
source xgboost_env/bin/activate  # En Windows: xgboost_env\Scripts\activate
```

3. **Instalar dependencias:**
```bash
pip install -r requirements.txt
```

### Ejecución Rápida

```bash
# Ejecutar comparativa completa de modelos XGBoost
python src/comparativa_modelos_XGBOOST.py

# Análisis exploratorio para detección de fraude
python src/eda_anonimizacion_fraud_detection.py

# Análisis modular de técnicas de anonimización
python src/eda_anonimizacion_modular.py
```

### Ejecutar Notebooks

```bash
jupyter notebook notebooks/
```

**Orden recomendado de ejecución:**
1. `notebook_EDA_anonimizacion_LLMs.ipynb` - Análisis exploratorio inicial
2. `notebook_deteccion_fraude_basico.ipynb` - Modelo baseline
3. `notebook_analisis_privacidad-utilidad.ipynb` - Evaluación final

## 📊 Resultados Principales

### Comparación Detallada: XGBoost

| Métrica | Original | Anonimizado | Impacto | Interpretación |
|---------|----------|-------------|---------|----------------|
| **Precisión** | 99.97% | 99.92% | -0.05% | ✅ Excelente |
| **Sensibilidad** | 80.60% | 59.66% | -20.94% | 🔴 Degradación significativa |
| **F1-Score** | 86.33% | 66.43% | -19.90% | 🔴 Requiere optimización |

### Análisis del Impacto

**✅ Fortalezas Mantenidas:**
- **Precisión alta**: 99.92% - Evita falsos positivos
- **Estructura del modelo**: XGBoost sigue siendo interpretable
- **Velocidad de inferencia**: No se ve afectada

**⚠️ Áreas de Degradación:**
- **Sensibilidad**: Pérdida significativa en detección de fraudes reales
- **Patrones complejos**: Dificultad para capturar relaciones sutiles post-anonimización
- **Gradientes**: Los árboles secuenciales son más sensibles a transformaciones

### Recomendaciones Específicas para XGBoost

1. **Ajustar hiperparámetros** post-anonimización:
   - Reducir `learning_rate` a 0.05
   - Aumentar `n_estimators` a 200
   - Usar `reg_alpha` y `reg_lambda` más altos

2. **Técnicas de anonimización específicas**:
   - Considerar rangos más amplios en k-anonimato
   - Explorar diferent values de ε en privacidad diferencial

3. **Ensamble con otros modelos**:
   - Combinar con Random Forest para mayor robustez

## 🛠️ Tecnologías Utilizadas

### Core ML Stack
```python
xgboost==1.7.4
scikit-learn==1.3.0
pandas==2.0.3
numpy==1.24.3
```

### Privacidad y Anonimización
```python
opacus==1.4.0              # Privacidad diferencial
diffprivlib==0.6.0         # Biblioteca de IBM para DP
faker==19.6.2              # Generación de datos sintéticos
```

### Análisis y Visualización
```python
matplotlib==3.7.2
seaborn==0.12.2
plotly==5.15.0
jupyter==1.0.0
```

## 📈 Impacto y Contribuciones

### Contribuciones Técnicas
- **📊 Análisis exhaustivo** del impacto de anonimización en gradient boosting
- **⚙️ Optimización específica** de hiperparámetros para datos anonimizados
- **🔍 Identificación** de limitaciones de XGBoost en contextos GDPR
- **📋 Framework** replicable para evaluación de modelos de boosting

### Implicaciones para la Industria
- **Bancos**: Consideraciones especiales para modelos XGBoost con datos anonimizados
- **Reguladores**: Evidencia de que algunos algoritmos requieren mayor cuidado
- **Desarrolladores ML**: Guía práctica para optimización post-anonimización

## 🔬 Metodología de Investigación

### Hipótesis Evaluadas
1. **H1**: XGBoost mantendrá rendimiento superior tras anonimización ❌
2. **H2**: Privacidad diferencial afectará más a XGBoost que a Random Forest ✅
3. **H3**: K-anonimato impactará gradientes de boosting ✅

### Experimentos Realizados
- **Baseline**: XGBoost sin anonimización
- **Anonimización básica**: Solo seudonimización
- **Anonimización completa**: Pipeline completo (k-anonimato + l-diversidad + DP)
- **Optimización**: Ajuste de hiperparámetros post-anonimización

## 📝 Documentación Adicional

### Informes Detallados
- [📊 Análisis de Impacto XGBoost](results/xgboost_impact_analysis.pdf)
- [⚙️ Guía de Optimización](docs/xgboost_optimization_guide.md)
- [🔒 Reporte de Cumplimiento GDPR](docs/gdpr_compliance_report.pdf)

### Notebooks de Apoyo
- [🧮 Calibración de Hiperparámetros](notebooks/hyperparameter_tuning.ipynb)
- [📈 Análisis de Sensibilidad](notebooks/sensitivity_analysis.ipynb)
- [🎯 Optimización Post-Anonimización](notebooks/post_anonymization_optimization.ipynb)

## 🎓 Contexto Académico

**Trabajo Final de Máster**: Anonimización de Datos Personales y Cumplimiento del GDPR en Datos Generados por Modelos de Lenguaje de Gran Escala (LLMs)

**Tutor**: Desirée Delgado  
**Universidad**: UNIE  
**Máster**: Análisis de Datos Masivos e Inteligencia Empresarial

## 🤝 Colaboración y Contribuciones

### Cómo Contribuir
1. Fork el repositorio
2. Crea una rama para tu feature (`git checkout -b feature/nueva-optimizacion`)
3. Commit tus cambios (`git commit -am 'Añadir nueva optimización XGBoost'`)
4. Push a la rama (`git push origin feature/nueva-optimizacion`)
5. Crea un Pull Request

### Issues y Mejoras
- 🐛 Reportar bugs en la implementación
- 💡 Sugerir nuevas técnicas de optimización
- 📚 Mejorar documentación
- 🧪 Añadir nuevos experimentos

## 📞 Contacto

**David González**  
📧 Email: dalexx07.28@gmail.com  
💼 LinkedIn: [David González - Data Engineer](https://www.linkedin.com/in/davidgnzlez/)  
🐙 GitHub: [@davidgonzalez](https://github.com/gonzalezvdavid)

## 📄 Licencia

Este proyecto está bajo la Licencia MIT - ver el archivo [LICENSE](LICENSE) para detalles.

## 🙏 Agradecimientos

- **Desirée Delgado**: Supervisión académica y orientación metodológica
- **Armando Ita**: Colaboración en Random Forest e intercambio de insights
- **Alexis Mendoza**: Trabajo en Regresión Logística y análisis comparativo
- **Universidad UNIE**: Marco académico y recursos de investigación
- **Comunidad XGBoost**: Documentación y best practices

---

## 📊 Estadísticas del Proyecto

![GitHub last commit](https://img.shields.io/github/last-commit/davidgonzalez/TFM-XGBoost-Anonimization-GDPR)
![GitHub repo size](https://img.shields.io/github/repo-size/davidgonzalez/TFM-XGBoost-Anonimization-GDPR)
![Python](https://img.shields.io/badge/python-v3.10+-blue.svg)
![XGBoost](https://img.shields.io/badge/XGBoost-v1.7+-orange.svg)
![GDPR](https://img.shields.io/badge/GDPR-Compliant-green.svg)
![License](https://img.shields.io/badge/license-MIT-blue.svg)

---

*"Optimizando XGBoost para un mundo donde la privacidad y el rendimiento coexisten"*
