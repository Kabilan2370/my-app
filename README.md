### A Dockerized API that serves a model
`#0969DA`
# What we are developing ?

Backend API: FastAPI

Model: A simple ML model (e.g., sklearn)

Database : postgres:15

Containerization:

Dockerfile (required)

Docker Compose

Install python 3 and pip
sudo apt install python3 python3-pip -y

Create a virtual environment
python3 -m venv myenv
source myenv/bin/activate

Install neccessary dependies using pip
pip install fastapi uvicorn scikit-learn pandas

Create a file name as model.py add below these lines

from sklearn.dummy import DummyClassifier
import pickle

# Create a dummy model
X_train = [[0], [1], [2], [3]]
y_train = [0, 1, 1, 0]

model = DummyClassifier(strategy="most_frequent")
model.fit(X_train, y_train)

# Save the model
with open("model.pkl", "wb") as f:
    pickle.dump(model, f)

Once you create the model.py file, Run the below command it will give the model.pkl file
python3 model.py

For FastAPI app create a app.py file

# app.py
from fastapi import FastAPI
import pickle

app = FastAPI()

# Load the model
with open("model.pkl", "rb") as f:
    model = pickle.load(f)

@app.get("/")
def read_root():
    return {"message": "Hello! ML API is running."}

@app.get("/predict")
def predict(x: int):
    prediction = model.predict([[x]])
    return {"input": x, "prediction": int(prediction[0])}

Once created these file, Using unicorn to start the application

uvicorn app:app --reload

Install the docker engine 

sudo apt update
sudo apt install docker-ce docker-ce-cli containerd.io -y

Create a Dockerfile to create a docker image

Before create a Dockerfile create another requirements.txt
this file should contain

fastapi
uvicorn[standard]
scikit-learn
pandas

This is the Dockerfile

# Use official Python slim image for smaller size
FROM python:3.11-slim

# Set working directory
WORKDIR /app

# Copy only requirements
COPY requirements.txt .

# Install dependencies
RUN pip install --no-cache-dir -r requirements.txt

# Copy the codes
COPY . .

# Expose port
EXPOSE 8000

# Command to run the API
CMD ["uvicorn", "app:app", "--host", "0.0.0.0", "--port", "8000"]

Build the docker image using this Dockerfile

sudo docker build -t api-ml .

Now create a docker container using this What we created docker image

sudo docker run -d --name api-app -p 8000:8000 api-ml(image-name)

http://public-ip/  = We can see the API
http://public-ip:8000/docs  = We can see the interactive Swagger docs

Once you done with this image upload this image on your dockerhub, This is my docker image stored on dockerhub
sudo docker pull kabilan2003/api-ml:latest

To Launch a multiple docker container use the docker compose file. Here I'm going to use the postgres for database.
So, pull the postgres:15 image from dockerhub

Create a docker-compose.yml file

version: "3.9"

services:
  api:
    build: .
    ports:
      - "8000:8000"
    depends_on:
      - db

  db:
    image: postgres:15
    environment:
      POSTGRES_USER: kabilan
      POSTGRES_PASSWORD: kabilan123
      POSTGRES_DB: mydb
    ports:
      - "5432:5432"

The finlly buiild the docker-compose file
sudo docker-compose up --build

For public url access any cloud ec2 machine, Here I used AWS EC2 machine.
then pulled the images from my dockerhub what I build before.
