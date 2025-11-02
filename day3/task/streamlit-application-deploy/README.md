

## Requirements
1. Python
2. Streamlit
3. Docker
4. DockerHub Account
5. Kubernetes Cluster (Minikube)


## Install Required Packages
```sh
sudo apt update && sudo apt upgrade -y
```
```sh
sudo apt install python3-pip -y
```

## Create Python Virtual Environment & Activate it
```sh
python -m venv venv
source venv/bin/activate
```

```sh
pip3 install streamlit scikit-learn pandas matplotlib seaborn reportlab
```

## Run Streamlit App
```sh
streamlit run app.py
```

## On your browser
```sh
http://localhost:8501
```
or 
```sh
http://PublicIP:8501
```

For more information, please refer to the original repository: [iQuantC/Scikit-learn-Streamlit-Docker-Kubernetes](https://github.com/iQuantC/Scikit-learn-Streamlit-Docker-Kubernetes)