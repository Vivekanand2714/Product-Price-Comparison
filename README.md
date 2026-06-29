# Product Price Comparison Application

A microservices-based product price comparison application deployed on IBM Cloud Code Engine.

## Architecture

The application consists of three microservices:

| Microservice | Language | Port | Source |
|---|---|---|---|
| Product Details (`prodlist`) | Python | 5000 | `products_list/` in backend repo |
| Dealer Details (`dealerdetails`) | Node.js | 8080 | `dealer_details/` in backend repo |
| Frontend | Python (Flask) | 5001 | `dealer_evaluation_frontend/` |

## Backend Source
- GitHub: https://github.com/ibm-developer-skills-network/dealer_evaluation_backend.git

## Deployment (IBM Cloud Code Engine)

### Task 1 – Deploy Product Details Microservice

```bash
ibmcloud ce application create \
  --name prodlist \
  --image us.icr.io/${SN_ICR_NAMESPACE}/prodlist \
  --registry-secret icr-secret \
  --port 5000 \
  --build-context-dir products_list \
  --build-source https://github.com/ibm-developer-skills-network/dealer_evaluation_backend.git
```

### Task 2 – Deploy Dealer Details Microservice

```bash
ibmcloud ce application create \
  --name dealerdetails \
  --image us.icr.io/${SN_ICR_NAMESPACE}/dealerdetails \
  --registry-secret icr-secret \
  --port 8080 \
  --build-context-dir dealer_details \
  --build-source https://github.com/ibm-developer-skills-network/dealer_evaluation_backend.git
```

### Task 3 – Clone Frontend

```bash
git clone https://github.com/ibm-developer-skills-network/dealer_evaluation_frontend.git
```

### Task 4 – Update API URLs in index.html

In `dealer_evaluation_frontend/html/index.html`, replace the placeholders with your deployed URLs:

```js
let produrl = "<PRODUCT_DETAILS_DEPLOYMENT_URL>/"
let dealerurl = "<DEALER_DETAILS_DEPLOYMENT_URL>/"
```

### Task 5 – Deploy Frontend Microservice

```bash
ibmcloud ce application create \
  --name dealerevaluation \
  --image us.icr.io/${SN_ICR_NAMESPACE}/dealerevaluation \
  --registry-secret icr-secret \
  --port 5001 \
  --build-context-dir dealer_evaluation_frontend \
  --build-source https://github.com/Vivekanand2714/Product-Price-Comparison.git
```

## Frontend Structure

```
dealer_evaluation_frontend/
├── html/
│   └── index.html       # Main UI - update produrl and dealerurl here
├── app.py               # Flask server
├── Dockerfile           # Container config
├── products.json        # Product data
└── requirements.txt     # Python dependencies
```

## Features

- Dropdown showing all available products
- Dealers listed per selected product
- Price displayed for selected dealer + product
- "All Dealers" option shows a table of all dealer prices

## Author
Project based on IBM Developer Skills Network Final Project – Option A (Python)
