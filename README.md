# Dokimion
Gerenciador de testes em kubernetes. Testando a autenticidade dos sistemas pelo fogo.


# Objetivos

Criação de um operator, que fará a gestão das operações no kubernetes para criação de ambientes de teste e cenários de teste de carga, inicialmente será fornecido suporte ao jMeter.

A operação é para ser feita pela interface gráfica e via custom resource.

Teremos um CR que definirá um ambiente de teste:
```yaml
apiVersion: dokimon.mycloudlab.github.io/v1
kind: TestEnvironment
metadata:
  name: my-jmeter-environment 
spec:
  jmeter:
    master:
     spec:
       containers:
       - image: quay.io/mycloudlab/dokimon-jmeter-master:latest
         name: jmeter-master
    slave:
        replicas:
        containers:
        - image: quay.io/mycloudlab/dokimon-jmeter-slave:latest
        name: jmeter-slave
```
    
```yaml
apiVersion: dokimon.mycloudlab.github.io/v1
kind: TestScenario
metadata:
  name: test-rhsso-cenario-1 
spec:
  environment: my-jmeter-environment
  jmeter:
    git: git@github.com:mycloudlab/dokimion.git
    branch: HEAD
    credentials:
      pat-secret: pat-secret
    script: load-test/cenario-1/test.jmx 
  current-state: error
  elapsed-time: "1 hour 24 min 13 seg"
```