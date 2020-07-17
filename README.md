# AIHR Pro-Label
Text annotation tool. Annotation features for text classification, sequence labeling and sequence to sequence. Create labeled data for sentiment analysis, classification models, named entity recognition, and text summarization. 

### Named entity recognition

First demo is one of the sequence labeling tasks, named-entity recognition. You just select text spans and annotate it. App also supports shortcut keys, so you can quickly annotate text spans or highlight with your mouse.

![Named Entity Recognition](./docs/entity.gif)

### Doc Classification / Doc Sentiment

Second demo is one of the text classification tasks, topic classification. Since there may be more than one category, you can annotate multi-labels.

![Text Classification](./docs/classification.gif)

### Machine translation

Final demo is one of the sequence to sequence tasks, machine translation. Since there may be more than one responses in sequence to sequence tasks, you can create multi responses.

## Features

-   Collaborative annotation
-   Create (import) and export datasets
-   View annotation progress statistics

## Requirements

-   Python 3.6+
-   Django 2.1.7+
-   Node.js 8.0+

## Installation

**Option 1: Pull production Docker image**

```bash
docker pull quinnpertuit/prolabel
```
**Option 2: Setup Python environment**

Install dependencies:

```bash
sudo apt-get install libpq-dev
pip install -r requirements.txt
cd app
```

Start the webpack server so the frontend gets compiled continuously:

```bash
cd server/static
npm install
npm run build
# npm start  # for developers
cd ..
```

**Option 3: Pull the development Docker-Compose images**

```bash
docker-compose pull
```

## Usage

### Start the development server

#### Option 1: Docker image as a Container

Run a Docker container:

```bash
docker run -d --rm --name prolabel \
  -e "ADMIN_USERNAME=admin" \
  -e "ADMIN_EMAIL=admin@example.com" \
  -e "ADMIN_PASSWORD=password" \
  -p 8000:8000 quinnpertuit1/prolabel
```

#### Option 2: Django development server

Before running, make the migration. Run the following command:

```bash
python manage.py migrate
```

Create admin user. Run the following command:

```bash
python manage.py create_admin --noinput --username "admin" --email "admin@example.com" --password "password"
```

Developers can validate the project by running the tests:

```bash
python manage.py test server.tests
```

Finally, to start the server:

```bash
python manage.py runserver
```

Optional; change the bind ip and port:

```bash
python manage.py runserver <ip>:<port>
```

#### Option 3: Development Docker-Compose stack

Use docker-compose to set up the webpack server, django server, database, all in one command:

```bash
docker-compose up
```

Open a Web browser and go to <http://127.0.0.1:8000/login/>. 

### Create a project

Log in with the superuser account. 

Chose project type (classification, entity, or sequence-to-sequence.

### Import Data

After creating a project, you will see the "Import Data" page, or click `Import Data` button in the navigation bar. 

You can upload the following types of files (depending on project type):

-   `Text file`: one sentence/document per line
-   `CSV file`: header with `"text"` as the first column or one-column csv file. If using labels the second column must be the labels.
-   `Excel file`: header with `"text"` as the first column or one-column excel file. If using labels the second column must be the labels. Supports multiple sheets as long as format is the same.
-   `JSON file`: each line contains a JSON object with a `text` key. JSON format supports line breaks rendering.

> Notice: Pro-Label won't render line breaks in annotation page for sequence labeling task due to the indent problem, but the exported JSON file still contains line breaks.

Any other columns (for csv/excel) or keys (for json) are preserved and will be exported in the `metadata` column.

### Define labels

Click `Labels` button in left bar to define custom labels. Specify label text, shortcut key, and colors.

### Annotation

Click the `Annotate Data` button in the navigation bar, and start to annotate the documents you uploaded.

### Export Data

After the annotation is complete (partially or in full), you can download the annotated data. Click the `Edit data` button in navigation bar, and then click `Export Data`.

You can export data as CSV file or JSON file by clicking the button. 

Exported documents have a metadata column or key, which will contain additional columns or keys from the imported document. For example:

Input file:
`import.json`

```JSON
{"text": "Working for IBM as a senior software engineer.", "meta": {"external_id": 1}}
```

Exported File:
`output.json`

```JSON
{"doc_id": 2023, "text": "Designing autonomous systems for Skunkworks", "labels": ["Autonomy"], "username": "root", "meta": {"external_id": 1}}
```
