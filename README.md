# Simple Material Query Engine

## Overview

This project is a simple material query engine designed to take free-text input queries and output relevant material information in a table format. The data is sourced from the [Materials Project](https://next-gen.materialsproject.org/) and stored in an Elasticsearch database. The goal is to provide users with a convenient way to search and retrieve information about various materials, including their properties and literature references. I have created two separated repositories for the [Frontend React App](https://github.com/dikshya-bhandari/material-explorer-frontend) and [Backend Nodejs App](https://github.com/dikshya-bhandari/material-explorer-backend).

Below, I have listed the processes I have followed to complete this project.

## Steps

- First, I went to [Materials Project API Documentation](https://api.materialsproject.org/docs) website and searched for an API that gives most of the features (fields) mentioned in the assignment task like:
  - ID: the candidate can choose the method how to generate the ID, must explain why they choose that method.
  - pretty_formula
  - Energy Above Hull
  - Space Group
  - Band Gap
  - Predicted Formation Energy
  - Magnetic Ordering
  - Total Magnetization
  - Experimentally Observed
- I found `GET /materials/summary/` to be the best and optimal API that gave all our fields. I ran the API call in Postman using [this URL](https://api.materialsproject.org/materials/summary/?deprecated=false&_per_page=200&_skip=0&_limit=200&_all_fields=true&license=BY-C) which was generated from the curl request in the documentation after entering the API Key.
- I downloaded the response from Postman and saved it in a JSON file (see `extract.js` file in this repo).
- The literature reference was missing from the response, so I found another API that gives the DOI of the material paper when I passed the ID of the material. The API was `GET /doi/{task_id}/` where task_id it accepted was the material ID.
- I then saved the output file given by `extract.js` in my Nodejs app and created an endpoint (`/uploadBulkData`) to upload the data to Elasticsearch.
- I then went to [Elasticsearch](https://www.elastic.co) and created an account, project, and index there. I got the necessary codes to connect it to a JavaScript application and upload as well as query data from there (see `elasticSetup.js` file).
- I created a Nodejs application with endpoints:
  - `/uploadAllData`: Uploads the entire data from `output.js` file to Elasticsearch.
  - `/getAllData`: Fetches the entire data from Elasticsearch (gets a list of only a few fields which are to be shown in the explorer page in the React app).
  - `/getDetail/:id`: Fetches the detail of one material (gets a list of all fields present).
  - `/searchData/:query`: Searches the entire Elasticsearch index for the query in "reference.bibtex", "magnetic_ordering", "pretty_formula", "ID" fields.
- I then created a Reactjs application with a home page and explorer page to search and show results.
- Then I deployed the Reactjs and Nodejs application to Netlify individually. The URLs are:
  - [Material Explorer Frontend](https://material-explorer-frontend.vercel.app/)
  - [Material Explorer Backend](https://material-explorer-backend.onrender.com/)

If you have any specific questions or if there's anything specific you'd like feedback on, feel free to ask!
