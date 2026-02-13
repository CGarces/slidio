# Slidio : The Google Slides Python Client

**A Python library to dynamically generate Google Slides from templates using text, charts, and tables.**

## 🚀 Features

- 🔤 Replace text in placeholder text boxes using `update(text_id, text_value)`
- 📊 Insert matplotlib figures in place of text boxes using `insert(text_id, matplotlib_figure)`
- 📋 Insert tables using `insert(text_id, data)`
- 🪄 Designed for templating: works with alt-text-based placeholders (e.g. `{{ TITLE }}`, `{{ BODY1 }}`)
- 📄 Create new presentations from templates with `from_template()`
- 👥 Share presentations with viewers and contributors
- 🔗 Get presentation URL with `url` property

## 🔧 Setup

1. Enable **Google Slides API** and **Google Drive API** on Google Cloud
2. Create a **service account**, download the `credentials.json`
3. Share your template Google Slides file with the service account

## 🧱 Usage

### Basic Usage

First install the package:

```bash
pip install slidio
```

Then, authenticate on Google Slides and build your "recipe":

```python
from slidio import SlidioClient
from google.oauth2 import service_account
import matplotlib.pyplot as plt

# Setup credentials and client
creds = service_account.Credentials.from_service_account_file(
    "credentials.json",
    scopes=[
        "https://www.googleapis.com/auth/presentations",
        "https://www.googleapis.com/auth/drive"
    ]
)

client = SlidioClient(creds, "your-presentation-id")

# Replace text
client.text.update("TITLE", "Quarterly Results")

# Insert graph
fig, ax = plt.subplots()
ax.plot([1, 2, 3], [4, 6, 5])
client.graph.insert("BODY1", fig)

# Insert table
data = [
    ["Name", "Score"],
    ["John", "85"],
    ["Jane", "92"]
]
client.table.insert("TABLE1", data)
```

### Creating from Template

```python
# Create a new presentation from a template
client = SlidioClient.from_template(
    credentials=creds,
    template_id="template-presentation-id",
    new_title="My New Presentation",
    viewers_emails=["viewer@example.com"],
    contributors_emails=["editor@example.com"]
)

# Get the presentation URL
print(f"Presentation URL: {client.url}")
```

## 📦 Installation

### Installation du package

```bash
pip install -e .
```

### Installation de l'environnement de développement

1. Installer uv (si ce n'est pas déjà fait) :
```bash
pip install uv
```

2. Installer les dépendances de développement :
```bash
uv pip install -e ".[dev]"
```

3. Installer pre-commit :
```bash
pip install pre-commit
pre-commit install
```

4. Installer les hooks Git :
```bash
pre-commit install
```

## 🛠️ Développement

### Exécuter les tests
```bash
pytest
```

### Vérifier le code
```bash
ruff check .
ruff format .
```

### Vérifier les types
```bash
mypy .
```

## 📁 Directory Structure

```md
py_slidio/
├── __init__.py
├── client.py
├── components/
│   ├── __init__.py
│   ├── text.py
│   ├── graph.py
│   └── table.py
└── utils.py
```

## 💡 Placeholder Design

Use `{{ PLACEHOLDER_ID }}` as the alt text in your Google Slides text boxes. 

For example:
- `{{ TITLE }}` for the presentation title
- `{{ BODY1 }}` for the first body text
- `{{ GRAPH1 }}` for the first graph
- `{{ TABLE1 }}` for the first table


## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.
