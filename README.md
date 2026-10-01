## mlops-production-pipeline

A small end-to-end MLOps project built to practice the engineering around a Machine Learning model.

The model is simple on purpose. The main focus is the pipeline around it:

- training
- model artifacts
- inference API
- tests
- Docker
- CI
- deployment

I used scikit-learn because model complexity is not the focus of this repository.

The [document-ai-pipeline](https://github.com/celsomsilva/document-ai-pipeline) repository was later derived from this project, reusing part of this structure for a Document AI pipeline.

---

## Live demo

[User-facing demo](https://mlops-mini-prod.onrender.com)

[Developer API docs](https://mlops-mini-prod.onrender.com/docs)

---

## Workflow

1. Train the model
2. Save the model artifact
3. Load the model through the API
4. Run the application in Docker
5. Run tests and CI checks
6. Deploy

---

## Project structure

```text
mlops-production-pipeline/

  src/
    mlops_api/
      __init__.py
      api.py          # FastAPI application (/health, /predict)
      train.py        # training script
      predict.py      # inference logic

  templates/
    index.html        # home page

  static/
    style.css         # CSS file

  models/
    model.joblib
    metadata.json

  tests/              # pytest tests

  Dockerfile
  compose.yaml
  .github/workflows/ci.yaml
  Makefile
  requirements.txt
  pyproject.toml
  README.md
  .gitignore
```

The project uses the `src-layout` packaging pattern. Application code is installed as the Python package `mlops_api`.

---

## Model

The API exposes a Ridge regression model that predicts weekly retail sales.

### Target

`weekly_sales`

Total weekly revenue.

### Features

- `price`: product price
- `promotion`: promotion active (`0` or `1`)
- `temperature`: environmental temperature

The model itself is deliberately small because this repository is mainly about the MLOps structure around it.

---

## Running locally

### Create a virtual environment

```bash
python3 -m venv .venv
source .venv/bin/activate
```

### Install dependencies

```bash
pip install -r requirements.txt
pip install -e .
```

### Train the model

```bash
make train
```

### Run tests

```bash
make test
```

### Start the API

```bash
make run
```

Swagger UI:

```text
http://localhost:8000/docs
```

---

## Docker

```bash
make docker
```

or:

```bash
docker compose up --build
```

Run tests with:

```bash
make test
```

---

## CI

GitHub Actions runs on pushes and pull requests.

The pipeline:

- checks out the repository
- creates a clean Python environment
- installs dependencies
- runs the tests
- builds the Docker image

A failed test or Docker build causes the workflow to fail.

---

## API endpoints

### Health check

```http
GET /health
```

Response:

```json
{
  "status": "ok"
}
```

### Prediction

Use:

```http
POST /predict
```

Example input:

```json
{
  "price": 12.5,
  "promotion": 1,
  "temperature": 25
}
```

Example output:

```json
{
  "prediction": 180.3,
  "model_version": "2026-02-05T18:12:00",
  "rmse": 10.63
}
```

The endpoint can also be tested through Swagger using the `/docs` page.

---

## Tech stack

- Python 3.11
- FastAPI
- scikit-learn
- Docker
- pytest
- GitHub Actions

---

## Author

This project was developed by an engineer and data scientist with a background in:

* Postgraduate degree in **Data Science and Analytics (USP)**
* Bachelor of **Science in Electrical and Computer Engineering (UERJ)**
* Special interest in statistical models, interpretability, and applied AI
* Strong interest in algorithmic reasoning, correctness, and performance evaluation

---

## Contact

* [LinkedIn](https://linkedin.com/in/celso-m-silva)
* Or open an [issue](https://github.com/celsomsilva/mlops-production-pipeline/issues)
