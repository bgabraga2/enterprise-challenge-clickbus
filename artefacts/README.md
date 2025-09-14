# Artefatos de Machine Learning - Estrutura Versionada

Esta pasta contém os artefatos dos modelos de Machine Learning organizados por versão.

## Estrutura

```
artefacts/
├── v1/                                    # Versão 1 dos modelos
│   ├── classification/                    # Modelo de classificação
│   │   ├── modelo_recompra_30dias.pkl    # Modelo treinado
│   │   └── feature_importance.csv        # Importância das features
│   ├── clusterization/                    # Modelo de clusterização
│   │   ├── modelo_clusterizacao.pkl      # Modelo K-Means
│   │   └── perfil_clusters.csv           # Perfis dos clusters
│   └── recommendation/                    # Modelo de recomendação
│       ├── modelo_recomendacao.pkl       # Modelo XGBoost
│       ├── label_encoder.pkl             # Encoder para labels
│       ├── feature_encoders.pkl          # Encoders para features
│       └── feature_importance.csv        # Importância das features
└── README.md                             # Esta documentação
```

## Versionamento

- **v1**: Versão inicial dos modelos
- Novas versões devem ser criadas em pastas separadas (v2, v3, etc.)
- A API pode ser configurada para usar diferentes versões através da variável `MODEL_VERSION`

## Uso na API

A API FastAPI (`new_api/main.py`) está configurada para usar esta estrutura:

```python
MODEL_VERSION = "v1"  # Alterar aqui para usar outra versão
MODEL_PATHS = {
    "clusterization": f"../artefacts/{MODEL_VERSION}/clusterization/modelo_clusterizacao.pkl",
    "classification": f"../artefacts/{MODEL_VERSION}/classification/modelo_recompra_30dias.pkl",
    "recommendation": {
        "model": f"../artefacts/{MODEL_VERSION}/recommendation/modelo_recomendacao.pkl",
        "label_encoder": f"../artefacts/{MODEL_VERSION}/recommendation/label_encoder.pkl",
        "feature_encoders": f"../artefacts/{MODEL_VERSION}/recommendation/feature_encoders.pkl"
    }
}
```

## Docker

O Dockerfile foi otimizado para copiar apenas os artefatos necessários:

- Não copia datasets completos
- Copia apenas modelos treinados (.pkl) e metadados essenciais
- Estrutura versionada permite fácil atualização de modelos

## Migração

Os artefatos foram migrados de `dist/*/artifacts/` para `artefacts/v1/*/` mantendo compatibilidade com a API.
