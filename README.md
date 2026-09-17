# Mini-TPs — MLOPs2_UBA

Entregas individuales de los mini-TP de **Operaciones de Aprendizaje Automático II** (CEIA, FIUBA), sesiones 1 a 3.

El modelo usado es el de predicción de stroke del TP final de **Aprendizaje de Máquina II** (llamado "MLOPS I" en el programa de esta materia): [`aprendizaje_maquina_II/`](https://github.com/christophcharaf/aprendizaje_maquina_II). En vez de depender del stack completo (Airflow + MLflow + MinIO + Postgres en Docker) para cada entrega semanal, `common/` reentrena localmente el mismo modelo (mismo dataset, mismo preprocesamiento — one-hot + SMOTE + escalado — y mismo `RandomForestClassifier`) y guarda los artefactos que consumen los tres mini-TP. Así cada sesión agrega solo la capa de protocolo (REST → GraphQL → gRPC) sobre el mismo modelo, como pide el enunciado.

## Estructura

```
common/
    train_model.py     # entrena el modelo y genera common/artifacts/{model.pkl, data_dict.json}
    inference.py        # preprocesamiento + predicción, compartido por los 3 mini-TP
mini_tp1_rest/           # Sesión 1 — API REST (FastAPI)
mini_tp2_graphql/        # Sesión 2 — metadatos por GraphQL (Strawberry)
mini_tp3_grpc/           # Sesión 3 — servicio gRPC (unary + streaming)
```

## Setup

```bash
python -m venv .venv
./.venv/Scripts/activate      # Windows (PowerShell: .venv\Scripts\Activate.ps1)
pip install -r requirements.txt
python common/train_model.py   # genera common/artifacts/
```

(Equivalente con `uv`: `uv venv && uv pip install -r requirements.txt`.)

## Mini-TPs

| Sesión | Carpeta | Qué expone |
|---|---|---|
| 1 — REST | [mini_tp1_rest](mini_tp1_rest/README.md) | `/v1/predict`, `/v1/model`, `/health` |
| 2 — GraphQL | [mini_tp2_graphql](mini_tp2_graphql/README.md) | esquema `Model { name, version, metrics }` |
| 3 — gRPC | [mini_tp3_grpc](mini_tp3_grpc/README.md) | `Predict` (unary) y `PredictBatch` (streaming) |

Cada carpeta tiene su propio README con cómo levantar el servicio, el cliente de prueba y, para las sesiones 2 y 3, la comparación pedida contra REST.
