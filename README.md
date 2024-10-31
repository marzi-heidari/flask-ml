
# Flask Azure App Deployment with CI/CD

This project demonstrates deploying a Flask app to Azure App Services and setting up CI/CD with Azure Pipelines. 

## Prerequisites
- Azure account: [Sign up for free](https://azure.microsoft.com)
- Azure CLI installed: [Azure CLI Installation Guide](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli)

## Project Setup

### 1. Create a Virtual Environment and Install Dependencies
Create and activate a virtual environment, then install Flask:
```bash
python3 -m venv ~/.flask-ml-azure
source ~/.flask-ml-azure/bin/activate
pip install flask
```

Create a `requirements.txt` file:
```plaintext
flask
```

### 2. Write a Simple Flask Application
In the project root, create a file named `app.py`:
```python
from flask import Flask

app = Flask(__name__)

@app.route('/')
def home():
    return "Hello, Azure!"

if __name__ == "__main__":
    app.run(host='0.0.0.0', port=5000)
```

Run the app locally to test it:
```bash
python app.py
```

Access the app at [http://127.0.0.1:5000](http://127.0.0.1:5000).

## Deploy to Azure App Services

### 3. Log in to Azure
```bash
az login
```

### 4. Deploy the App to Azure
Use the Azure CLI to create a resource group and deploy the Flask app:
```bash
az group create --name flask-rg --location eastus
az webapp up --name <your-app-name> --resource-group flask-rg --location eastus --sku B1
```

- Replace `<your-app-name>` with a unique name for your app.
- `B1` is the SKU for the Basic tier. Adjust it as needed.

### 5. Verify the Deployment
Visit `https://<your-app-name>.azurewebsites.net/` to confirm the app is live.

## Set Up CI/CD with Azure Pipelines

### 6. Create a GitHub Repository
Initialize a Git repository:
```bash
git init
git add .
git commit -m "Initial commit"
git remote add origin <your-github-repo-url>
git push -u origin main
```

### 7. Set Up an Azure DevOps Project
1. Go to [Azure DevOps](https://dev.azure.com/) and create a new project.
2. In **Pipelines**, create a new pipeline and connect it to your GitHub repository.

### 8. Create Azure Pipeline YAML File
In your project, create a `.github/workflows/azure-pipelines.yml` file with the following:


### 9. Commit and Push to GitHub
```bash
git add .github/workflows/azure-pipelines.yml
git commit -m "Set up Azure Pipelines"
git push
```

Azure Pipelines will automatically run on each push, deploying any updates to your Azure App Services instance.

### Quota Issue Resolution
If you encounter a quota error for **PremiumV2 instances**, use one of the following options:

1. **Change SKU**: Use a lower SKU (e.g., `B1` or `S1`) when running the `az webapp up` command:
   ```bash
   az webapp up --name <your-app-name> --resource-group flask-rg --location eastus --sku B1
   ```
2. **Change Region**: Choose a region with available PremiumV2 capacity (e.g., `centralus` or `westus`):
   ```bash
   az webapp up --name <your-app-name> --resource-group flask-rg --location centralus --sku P1v2
   ```
3. **Request Quota Increase**: Submit a request through the Azure Portal under **Help + Support** for a PremiumV2 quota increase in the desired region.

## Testing the Deployed Application
Test the deployed application by visiting `https://<your-app-name>.azurewebsites.net/`. Update any additional scripts for testing or prediction to point to the deployed URL.

