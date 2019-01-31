
# TechU: Platform security 

## Setup

```
git clone https://github.com/sarts3/techU_platform_security.git
cd techU_platform_security
git submodule init
git submodule update

```

## Secret Review

### Truffelhog ( https://github.com/dxa4481/truffleHog )

```
pip install trufflehog
trufflehog --entropy=True https://github.com/dxa4481/truffleHog.git
trufflehog --entropy=True secrets_demo_repo
trufflehog --regex --entropy=False secrets_demo_repo
trufflehog --regex --entropy=False --rules rules_truffle.json secrets_demo_repo
trufflehog --regex --entropy=False --rules rules_truffle.json --json secrets_demo_repo
``` 

### git-secrets ( https://github.com/awslabs/git-secrets )
    
- Setup

```
git clone https://github.com/awslabs/git-secrets.git
cd git-secrets
PATH=$PATH:$PWD
```

- Find secrets

```
cd ../secrets_demo_repo
git config --local --add secrets.patterns "apiKey\\s*=\\s*.+"
git config --local --add secrets.patterns "password\\s*=\\s*.+"
git-secrets --scan -r
git config --local --add secrets.patterns "hash\\s*=\\s*.+"
git config --local --add secrets.patterns "ssh-keygen"
git-secrets --scan -r
```

- Install hooks

```
git-secrets --install
ls
echo "value: \"password=THIS_TEXT_WILL_BE_CHANGED_FOR_ENVIRONMENT_VARIABLE\"" > secret.md
git status
git add secret.md
git config --global user.name "test username"
git commit -m "SECRET"
```

- False Positive

```
git secrets --add --allowed 'THIS_TEXT_WILL_BE_CHANGED_FOR_ENVIRONMENT_VARIABLE'
git commit -m "SECRET"
cd ..
```

## SAST Review

OWASP TOP 10: https://www.owasp.org/images/5/5e/OWASP-Top-10-2017-es.pdf

### Detección de vulnerabilidades en librerías

```
git clone https://github.com/OWASP/NodeGoat.git
cd NodeGoat/; npm install
npm audit
npm audit fix
cd ..
```

**NOTA(Solo en caso de fallo):** Si falla en algún punto el npm intentar actualizar con los siguientes comandos: 

```
echo 'export PATH=$HOME/local/bin:$PATH' >> ~/.bashrc
. ~/.bashrc
mkdir ~/local
mkdir ~/node-latest-install
cd ~/node-latest-install
curl http://nodejs.org/dist/node-latest.tar.gz | tar xz --strip-components=1
./configure --prefix=$HOME/local
make install
curl -L https://www.npmjs.com/install.sh | sh (si da fallos de locale ejecutar antes --> export LC_ALL=en_US.UTF-8)
```

### SAST en PYTHON
```
pip install bandit
git clone https://github.com/Contrast-Security-OSS/DjanGoat.git
cd DjanGoat
bandit -r .
cd ..
```

### SAST en Javascript
```
git clone https://github.com/ajinabraham/NodeJsScan
cd NodeJsScan
docker build -t nodejsscan . (si no hay permisos para ejecutar docker, pedirlos al administrador)
docker run -it -p 9090:9090 nodejsscan
Probar nodejsscan en el navegador con localhost:9090
cd ..
```

## Docker Images Review

### Basic Docker

```
docker run hello-world
docker run -d --name web-test -p 80:8000 crccheck/hello-world
```

### Rachel ( https://github.com/zamarrowski/rachel-resources.git )

```
git clone https://github.com/zamarrowski/rachel-resources.git
cd rachel-resources
docker-compose up (si no funciona se necesitan permisos de administrador para instalar docker-compose)
(press ctrl +c to stop after some seconds)
docker-compose up
```

On other terminal on the same path
```
docker run -v `pwd`/yair/config/:/opt/yair/config/:ro yfoelling/yair nginx | jq (if jq is not installed, just remove '| jq')
docker run -v `pwd`/yair/config/:/opt/yair/config/:ro yfoelling/yair postgres (if jq is not installed, just remove '| jq')
```

