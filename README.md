# Simple Material Query Engine

## Overview

This project is a simple material query engine designed to take free-text input queries and output relevant material information in a table format. The data is sourced from the [Materials Project](https://next-gen.materialsproject.org/) and stored in an Elasticsearch database. The goal is to provide users with a convenient way to search and retrieve information about various materials, including their properties and literature references. I have created two seperated repositories for Frontend React App (https://github.com/dikshya-bhandari/material-explorer-frontend) and Backend Nodejs App (https://github.com/dikshya-bhandari/material-explorer-backend)

Below, I have listed the processes I have followed to complete this project

## Steps

    - First, I went to https://api.materialsproject.org/docs website and searched for API which gives most of the features(fields) mentioned in the assignment task like
         ID: the candidate can choose the method how to generate the ID, must explain why they choose that method.
         pretty_formula
         Energy Above Hull
         Space Group
         Band Gap
         Predicted Formation Energy
         Magnetic Ordering
         Total Magnetization
         Experimentally Observed
    - I found GET /materials/summary/ to be the best and optimal API that gave all our fields. I ran the API call in postman using https://api.materialsproject.org/materials/summary/?deprecated=false&_per_page=200&_skip=0&_limit=200&_all_fields=true&license=BY-C url which was generated from the curl request it gave there in the documentation after I entered the API Key which they gave me after I logged in.
    - I downloaded the response from the postman and saved it in a json file which can be seen in the extract.js file in this repo
    - The literature reference was missing from the response, so I found another API that gives the doi of the material paper when I passed ID of the material. The API was GET /doi/{task_id}/ where task_id it accepted was material ID.
    - I then saved the output file given by the extract.js in my nodejs app(--) and made an endpoint(/uploadBulkData) to upload the data to elasticsearch.
    - I then went to https://www.elastic.co and created an account, project and created an elasticsearch index there and got the neccessary codes to connect it to a javascript application and upload as well as query data from there.
    - I createad a nodejs application with endpoints:
        - /uploadAllData
            It uploads the entire data from output.js file to the elasticsearch
        - /getAllData
            It fetches the entire data from elasticsearch(gets list of only few fields which is to be shown in the explorer page in react app)
        - /getDetail/:id
            It fetches the detail of one material(gets list of all fields present)
        - /searchData/:query
            It searches the entire elasticsearch index for the query in "reference.bibtex", "magnetic_ordering", "pretty_formula", "ID" fields.
    - The elasticsearch setup can be found in (elasticSetup.js file)

    - I then created a reactjs application with home page and explorer page to search and show results.

    - Then I deployed the reactjs and nodejs application to netlify individually. The urls are
        - https://material-explorer-frontend.vercel.app/
        - https://material-explorer-backend.onrender.com/
