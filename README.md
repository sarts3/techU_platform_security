
# TechU: Platform security 

## Setup

```
    git clone https://github.com/sarts3/techU_platform_security.git
    cd techU_platform_security/secrets_demo_repo
    git submodule init
    git submodule update
    cd ..
```

## Secret Review

### Truffelhog ( https://github.com/dxa4481/truffleHog )

    git clone https://github.com/dxa4481/truffleHog.git
    cd truffleHog    
    truffleHog --entropy=True https://github.com/dxa4481/truffleHog.git
    truffleHog --entropy=True ../secrets_demo_repo
    truffleHog --regex --entropy=False ../secrets_demo_repo
    trufflehog --regex --entropy=False --rules ../rules_truffle.json ../secrets_demo_repo
    trufflehog --regex --entropy=False --rules ../rules_truffle.json --json ../secrets_demo_repo
    

### git-secrets ( https://github.com/awslabs/git-secrets )
    
- Installation

```
    git clone https://github.com/awslabs/git-secrets.git
    cd git-secrets
    make install
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
    git commit -m "SECRET"
```

- False Positive

```
    git secrets --add --allowed 'THIS_TEXT_WILL_BE_CHANGED_FOR_ENVIRONMENT_VARIABLE'
    git commit -m "SECRET"
```

## SAST Review

OWASP TOP 10: https://www.owasp.org/images/5/5e/OWASP-Top-10-2017-es.pdf

### Detección de vulnerabilidades en librerías

```
git clone https://github.com/OWASP/NodeGoat.git
cd NodeGoat/; npm install
npm audit
npm audit fix
```

### SAST en PYTHON
```
pip install bandit
git clone https://github.com/Contrast-Security-OSS/DjanGoat.git
cd DjanGoat
bandit -r .
```

### SAST en Javascript
```
git clone https://github.com/ajinabraham/NodeJsScan
cd NodeJsScan
docker build -t nodejsscan .
docker run -it -p 9090:9090 nodejsscan
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
    docker-compose up
    (on other terminal)
    docker run -v `pwd`/yair/config/:/opt/yair/config/:ro yfoelling/yair nginx | jq
    docker run -v `pwd`/yair/config/:/opt/yair/config/:ro yfoelling/yair postgres | jq
```