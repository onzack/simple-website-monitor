# simple-website-monitor

A simple website monitor based on the Telegraf http_response plugin.  
The idea is to run Telegraf as deployment on Kubernetes for hight availability.    
This project contains a Helm Chart and some other files and scripts for the best usability.  

Used technonlogies:
* Docker
* Telegraf
* InfluxDB
* Grafana

Official [Telegraf/http_response Docs](https://github.com/influxdata/telegraf/tree/master/plugins/inputs/http_response  )

We created a custom Docker Image for Telegraf because we wanted some security optimizations.  
You can find the Docker Image on [Quay.io](https://quay.io/repository/onzack/telegraf-swm?tab=tags)

# Architecture
![A Sample Graph for visualization ](https://github.com/onzack/simple-website-monitor/blob/main/Docs/simple-website-monitor-architecture.png)

## Quick Guide for simple-website-monitor

### Prerequisites
On your client:
- Install Helm on your client: https://helm.sh/docs/intro/quickstart/
- Install kubectl on your client: https://kubernetes.io/docs/tasks/tools/install-kubectl/  
- Clone this project to your client  
- Make sure you have a kubeconfig for the target Kubernetes cluster and save it to ```"~./kube/config```.

Infrastructure:
- An InfluxDB instance
- A Grafana instance

### Install Telegraf with http_response
```
kubectl apply -f ./simple-website-monitor/simple-website-monitor-namespace.yaml
cp ./simple-website-monotor/custom-values.yaml ./custom-values.yaml
vim ./custom-values.yaml
helm install -n simple-website-monitor simple-website-monitor -f custom-values.yaml --set swmDb.influxDbUser=<user> --set swmDb.influxDbPW=<password> ./simple-website-monitor/Helm/simple-website-monitor
```
You can find a Grafana Dashboard in this project. Add it to your Grafana instance to get started and monitor your websites.

### Rancher specific
Add the simple-website-monitor namespace to a project if you want.

## Clean up
```
helm uninstall -n simple-website-monitor simple-website-monitor
kubectl delete namespace simple-website-monitor
```

# Licence
Copyright 2021 ONZACK AG

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
