
# TechU: Platform security 

## Secret Review

### Truffelhog ( https://github.com/dxa4481/truffleHog )


    git clone https://github.com/dxa4481/truffleHog.git
    cd truffleHog    
    truffleHog --entropy=True https://github.com/dxa4481/truffleHog.git
    truffleHog --regex --entropy=False https://github.com/dxa4481/truffleHog.git
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

## Docker Images Review

### Basic Docker

```
    docker run hello-world
    docker run -d --name web-test -p 80:8000 crccheck/hello-world
```

### Rachel (https://github.com/zamarrowski/rachel-resources.git)

```
    docker-compose up
    docker run -v `pwd`/yair/config/:/opt/yair/config/:ro yfoelling/yair nginx
    docker run -v `pwd`/yair/config/:/opt/yair/config/:ro yfoelling/yair postgres
```