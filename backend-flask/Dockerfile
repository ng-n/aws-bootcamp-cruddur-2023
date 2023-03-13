FROM python:3.10-slim-buster

# Inside Container
# Create a new directory named backend-flask inside container
WORKDIR /backend-flask

# Outside Container => Inside Container 
# requirements.txt contains libraries required to install to run the app
COPY requirements.txt requirements.txt

# Install python libraries required for the app
RUN pip3 install -r requirements.txt

# Copy from Outside Container to Inside Container
# first peropd = /backend-flask (outside container)
# second period = /backend-flask (inside container)
COPY . .

# Set Environment Variables
# Insdie the Container and will remain set when the container is running
ENV FLASK_ENV=development

EXPOSE ${PORT}

# CMD (Command) ==> python3 -m flask run --host=0.0.0.0 --port 4567
#CMD ["python3", "-m", "flask", "run", "--host=0.0.0.0", "--port=4567"]
CMD ["./script.sh"]