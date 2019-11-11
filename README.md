# cheyyali

cheyyali is a fork of [doccano](https://github.com/chakki-works/doccano) project.


### [Named entity recognition](https://doccano.herokuapp.com/demo/named-entity-recognition/)

The first demo is a sequence labeling task: named-entity recognition. You just select text spans and annotate them. Doccano supports shortcut keys, so you can quickly annotate text spans.

![Named Entity Recognition](./docs/named_entity_annotation.gif)

### [Sentiment analysis](https://doccano.herokuapp.com/demo/text-classification/)

The second demo is a text classification task: sentiment analysis. Since there may be more than one category, you can annotate with multiple labels.

![Text Classification](./docs/text_classification.gif)

### [Machine translation](https://doccano.herokuapp.com/demo/translation/)

The final demo is a sequence to sequence task: machine translation. Since there may be more than one response in sequence to sequence tasks, you can create multiple responses.

![Machine Translation](./docs/translation.gif)

## Features

-   Collaborative annotation
-   Multi-language support
-   Mobile support
-   Emoji :smile: support
-   (future) Auto labeling

## Requirements _kavalasinavi_

-   Python 3.6+
-   Django 2.1.7+
-   Node.js 8.0+
-   Google Chrome (highly recommended)

## Install _Cheyyadanaki_

### Clone repository

First of all, you have to clone the repository:

```bash
git clone https://github.com/CivicDatalab/cheyyali.git
cd cheyyali
```

_Note for Windows developers: Be sure to configure git to correctly handle line endings or you may encounter `status code 127` errors while running the services in future steps. Running with the git config options below will ensure your git directory correctly handles line endings._

```bash
git clone https://github.com/CivicDatalab/cheyyali.git --config core.autocrlf=input
```

### Installation

First we need to install the dependencies. Run the following commands:

```bash
sudo apt-get install libpq-dev unixodbc unixodbc-dev npm
pip install -r requirements.txt
cd app
```

> Note: If you want to add annotators, see [Frequently Asked Questions](https://github.com/doccano/doccano/wiki/Frequently-Asked-Questions#i-want-to-add-annotators)

## Usage

### Start the development server

Let’s start the development server and explore it.

#### Running Django development server

Before running, we need to make migration. Run the following command:

```bash
python manage.py migrate
```

Next we need to create a user who can login to the admin site. Run the following command:

```bash
python manage.py create_admin --noinput --username "admin" --email "admin@example.com" --password "password"
```

Create the admin, annotator, and annotation approver roles to assign to users. Run the following command:

```bash
python manage.py create_roles
```

Developers can also validate that the project works as expected by running the tests:

```bash
python manage.py test server.tests
```

Finally, to start the server, run the following command:

```bash
python manage.py runserver
```

Optionally, you can change the bind ip and port using the command

```bash
python manage.py runserver <ip>:<port>
```


### Confirm all cheyalli services are running
Open a Web browser and go to <http://127.0.0.1:8000/login/>. You should see the login screen:

<img src="./docs/login_form.png" alt="Login Form" width=400>

### Create a project

Now, try logging in with the superuser account you created in the previous step. You should see the cheyalli project list page:

<img src="./docs/projects.png" alt="Projects page" width=600>

There is no project created yet. To create your project, make sure you’re in the project list page and select `Create Project` button. You should see the following screen:

<img src="./docs/create_project.png" alt="Project Creation" width=400>

In this step, you can select three project types: text classification, sequence labeling and sequence to sequence. You should select a type with your purpose.

### Import Data

After creating a project, you will see the "Import Data" page, or click `Import Data` button in the navigation bar. You should see the following screen:

<img src="./docs/upload.png" alt="Upload project" width=600>

You can upload the following types of files (depending on project type):

-   `Text file`: file must contain one sentence/document per line separated by new lines.
-   `CSV file`: file must contain a header with `"text"` as the first column or be one-column csv file. If using labels the second column must be the labels.
-   `Excel file`: file must contain a header with `"text"` as the first column or be one-column excel file. If using labels the second column must be the labels. Supports multiple sheets as long as format is the same.
-   `JSON file`: each line contains a JSON object with a `text` key. JSON format supports line breaks rendering.

> Notice: Doccano won't render line breaks in annotation page for sequence labeling task due to the indent problem, but the exported JSON file still contains line breaks.

`example.txt/csv/xlsx`

```txt
EU rejects German call to boycott British lamb.
President Obama is speaking at the White House.
He lives in Newark, Ohio.
...
```

`example.json`

```JSON
{"text": "EU rejects German call to boycott British lamb."}
{"text": "President Obama is speaking at the White House."}
{"text": "He lives in Newark, Ohio."}
...
```

To stop the container, run `docker container stop doccano -t 5`.
All data created in the container will persist across restarts.

Access <http://127.0.0.1:8000/>.

## One-click Deployment

| Service | Button |
|---------|---|
| AWS[^1]   | [![AWS CloudFormation Launch Stack SVG Button](https://cdn.rawgit.com/buildkite/cloudformation-launch-stack-button-svg/master/launch-stack.svg)](https://console.aws.amazon.com/cloudformation/home?#/stacks/create/review?stackName=doccano&templateURL=https://s3-external-1.amazonaws.com/cf-templates-10vry9l3mp71r-us-east-1/2019290i9t-AppSGl1poo4j8qpq)  |
| Azure | [![Deploy to Azure](https://azuredeploy.net/deploybutton.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fdoccano%2Fdoccano%2Fmaster%2Fazuredeploy.json)  |
| GCP[^2] | [![GCP Cloud Run PNG Button](https://storage.googleapis.com/gweb-cloudblog-publish/images/run_on_google_cloud.max-300x300.png)](https://console.cloud.google.com/cloudshell/editor?shellonly=true&cloudshell_image=gcr.io/cloudrun/doccano&cloudshell_git_repo=https://github.com/doccano/doccano.git)  |
| Heroku  | [![Deploy](https://www.herokucdn.com/deploy/button.svg)](https://heroku.com/deploy)  |

> [^1]: (1) EC2 KeyPair cannot be created automatically, so make sure you have an existing EC2 KeyPair in one region. Or [create one yourself](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html#having-ec2-create-your-key-pair). (2) If you want to access doccano via HTTPS in AWS, here is an [instruction](https://github.com/doccano/doccano/wiki/HTTPS-setting-for-doccano-in-AWS).
> [^2]: Although this is a very cheap option, it is only suitable for very small teams (up to 80 concurrent requests). Read more on [Cloud Run docs](https://cloud.google.com/run/docs/concepts).

## Contribution

You can export data as CSV file or JSON file by clicking the button. As for the export file format, you can check it here: [Export File Formats](https://github.com/CivicDatalab/cheyyali/wiki/Export-File-Formats).

Here are some tips might be helpful. [How to Contribute to Doccano Project](https://github.com/doccano/doccano/wiki/How-to-Contribute-to-Doccano-Project)

## Citation

```
@misc{doccano,
  title={{doccano}: Text Annotation Tool for Human},
  url={https://github.com/doccano/doccano},
  note={Software available from https://github.com/doccano/doccano},
  author={
    Hiroki Nakayama and
    Takahiro Kubo and
    Junya Kamura and
    Yasufumi Taniguchi and
    Xu Liang},
  year={2018},
}
```
## Contact

For help and feedback, please feel free to contact [Samantar Team](https://samantar@civicdatalab.in)
