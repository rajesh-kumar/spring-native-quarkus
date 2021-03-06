# spring-native-vs-quarkus
This work is extension to what Szilárd Antal" and "Ivan Franchin" did for comparing spring boot, quarkus and micronaut. We have combined both's work and tried to compare spring native and quarkus. 

The goal is to compare Quarkus and Spring Native by measuring start-up times and memory footprint.

## Prerequisite 
 
* Python3
* [Docker SDK for Python](https://docker-py.readthedocs.io/en/stable/)
* [Pandas](https://pypi.org/project/pandas/)
* [PyYaml](https://pypi.org/project/PyYAML/)
* [Kubernetes Python Client](https://github.com/kubernetes-client/python/)

```shell script
$ pip install docker
$ pip install pandas
$ pip install pyaml
$ pip install kubernetes
```

### Kubernetes with Docker For Mac

1. Enable Kubernetes
2. Setup Kubernetes Metrics Server
    1. Download the latest components.yaml
        ```shell script
        wget https://github.com/kubernetes-sigs/metrics-server/releases/download/v0.3.6/components.yaml
        ```
    2. Open components.yaml and add the –kubelet-insecure-tls argument into the existing args section.    
        ```yaml
            args:
              - --cert-dir=/tmp
              - --secure-port=4443
              - --kubelet-insecure-tls
        ```
    3. Run the following command 
        ```shell script
        kubectl apply -f components.yaml
        ```
## Run  
To compare all kind of Todo apps in docker just execute
```shell script
./build_and_monitor.py
```

You can specify the app-type and/or build-type to build and run only a set of application 
* app-type: type of the application (spring or quarkus or all)
* build-type: type of the build (jvm or native or all)

```shell script
./build_and_monitor.py -t {app-type} {build-type}
```

The applications can also be run on Docker or Kubernetes. The default platform is the Docker, 
but you can switch to Kubernetes with the -p flag.  
 
```shell script
./build_and_monitor.py -t {app-type} -p {platform} {build-type}
```

### Python Scripts

1. infra.py - sets up the environment and starts/stops postgres-db, prometheus and grafana services
    ```shell script
    ./infra.py -p {platform} start|stop
    ```
2. builder.py - builds and creates docker image for the specified Todo app(s) 
    ```shell script
    ./builder.py -t {app-type} {build-type}
    ```
3. monitor.py - starts and monitors the Todo app(s) on the specified platform
    ```shell script
    ./monitor.py -t {app-type} -b {build-type} -p {platform} start|stop
    ```


