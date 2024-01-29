# image-background-removal-api-end-to-end-pipeline

[This repository](https://github.com/danielgatis/rembg) is selected as the image background removal algorithm.

Implementation is straightforward, the first step is to install it as a Python package.

    pip install rembg
    
Then, this snippet produces an image with its background removed.

    from rembg import remove

    input_path = 'input.png'
    output_path = 'output.png'

    with open(input_path, 'rb') as i:
        with open(output_path, 'wb') as o:
            input = i.read()
            output = remove(input)
            o.write(output)

#### API

The API is deployed into a t2.2xlarge instance from AWS. 

Steps:

Log into AWS account and create an EC2 instance, using the latest stable Ubuntu image.

SSH into the instance and run these commands to update the software repository and install the dependencies.

    sudo apt-get update
    sudo apt install -y python3-pip nginx
    
    sudo nano /etc/nginx/sites-enabled/fastapi_nginx
    
    
And put this config into the file (replace the IP address with your EC2 instance's public IP):

    server {
        listen 80;   
        server_name <YOUR_EC2_IP>;    
        location / {        
            proxy_pass http://127.0.0.1:8000;    
        }
    }

Start NGINX.

    sudo service nginx restart
    
Update EC2 security-group settings for your instance to allow HTTP traffic to port 80.

#### image background removal served through an API

    git clone https://github.com/computervisiondeveloper/image-background-removal-api-end-to-end-pipeline.git
    
    cd image-background-removal-api-end-to-end-pipeline

Create a virtual environment and install requirements.

    sudo apt install python3-virtualenv

    virtualenv venv --python=python3
    
    source venv/bin/activate

    pip install -r requirements.txt

Launch app.
    
    python3 -m uvicorn main:app


### deliverable

[This](https://docs.google.com/document/d/1zwRmXQDsDvB9vg2lHc3zYwjIxJ4b-YNu/edit?usp=drive_link&ouid=107960887514237623929&rtpof=true&sd=true).
