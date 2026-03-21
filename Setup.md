# Install Jenkins First Ubuntu

EC2 Server ----> Ubuntu 

# Install Docker 

sudo apt update
sudo apt install -y docker.io

sudo systemctl start docker
sudo systemctl enable docker

sudo usermod -aG docker Jenkins

sudo systemctl restart jenkins
sudo systemctl restart docker

# Install Java First 

sudo apt install -y openjdk-17-jdk

java -version

# Install Jenkins From Officail Site 

# Install Maven

sudo apt install -y maven

mvn -version

# Install GitHub In Server 

apt-get install git -y


# Install Sonarcube On Another Server 

docker run -d \
  --name sonarqube \
  -p 9000:9000 \
  sonarqube:lts-community

  Open Port No 9000

  Take Access Using Public Ip 

  Login = admin / admin


# Configure Jenkins Project




