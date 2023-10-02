Experimento utilizando Docker para limitar el uso de CPU en un contenedor y analizar
las consecuencias en el tiempo de ejecucion de un benchmark de CPU.


## Pasos

1. Creamos carpeta para este experimento

2. Creamos el archivo llamado Dockerfile con el contenido:
FROM ubuntu:latest
RUN apt-get update && apt-get install -y sysbench
COPY run-benchmark.sh /usr/local/bin/
ENTRYPOINT ["run-benchmark.sh"]

3. Creamos un script llamado run-benchmark.sh con el contenido:
#!/bin/bash
echo "Running sysbench..."
sysbench --test=cpu --cpu-max-prime=20000 run

4. Le damos permiso de ejecucion con chmod +x

5. Construimos imagen Docker con docker build

6. Ejecutamos el contenedor limitando el uso de CPU al 50% con docker run -it
--cpus=".5" experimento-cpu-docker

7. Realizamos el mismo paso anterior pero para un uso de CPU del 25%



## Resultados

- CPU al 50%:

Running the test with following options:
Number of threads: 1
Initializing random number generator from current time


Prime numbers limit: 20000

Initializing worker threads...

Threads started!

CPU speed:
    events per second:   150.22

General statistics:
    total time:                          10.1239s
    total number of events:              1521

Latency (ms):
         min:                                    2.03
         avg:                                    6.62
         max:                                  435.87
         95th percentile:                       31.94
         sum:                                10062.77

Threads fairness:
    events (avg/stddev):           1521.0000/0.00
    execution time (avg/stddev):   10.0628/0.00
    
    
- CPU al 25%:

Running the test with following options:
Number of threads: 1
Initializing random number generator from current time


Prime numbers limit: 20000

Initializing worker threads...

Threads started!

CPU speed:
    events per second:    90.27

General statistics:
    total time:                          10.0020s
    total number of events:              903

Latency (ms):
         min:                                    2.07
                  avg:                                   10.99
         max:                                   83.43
         95th percentile:                       78.60
         sum:                                 9922.29
    
Threads fairness:
    events (avg/stddev):           903.0000/0.00
    execution time (avg/stddev):   9.9223/0.00


## Conclusiones

Podemos observar como el uso de CPU afecta al tiempo de ejecucion. Cuando se limita
al 50% se logra una velocidad mayor que usando un 25% de la CPU. De esta forma, la
limitacion de la CPU al 25% reduce la velocidad de la CPU y aumenta el tiempo de
ejecucion del benchmark.

## Autor

Cristina Vilella Grindlay

    

