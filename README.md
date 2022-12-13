# GraphQL-Microprofile-Quarkus
Sample code of SmallRye GraphQL which is an implementation of MicroProfile GraphQL specification using Quarkus framework

## POC:
<li>
MicroProfile GraphQL
</li>

## Prerequisite

<li>
Kubernetes Cluster
 </li>
 <li>
Istio Instalation (tested on istio-1.16.1)
</li>


## Download the source code

```
Git clone https://github.com/mosesalphonse/GraphQL-Microprofile-Quarkus.git

cd GraphQL-Microprofile-Quarkus

```

Note: verify the quarkus.container-image.group name in application.properties file
quarkus.container-image.group=<<gcp project id>>

## Build, push the image into rep and deploy in kubernetes and istio

```
mvn clean package -Dnative -Dquarkus.container-image.push=true

Kubectl apply -f yamls/manifest.yaml
Kubectl apply -f yamls/network.yaml

```
## Verify

Note: get Istio Ingress Gateway external IP
In browser, 

```
http://{ingressIP}/graphql/schema.graphql

```
Using Curl

```
curl 'http://{ingressIP}/graphql' --data-raw '{"query":"{\n  allFilms {\n    director\n    episodeID\n    releaseDate\n    title\n  }\n}","operationName":null}'

curl 'http://{ingressIP}/graphql' --data-raw '{"query":"query getFilm {\n  film(filmId: 1) {\n    title\n    director\n    releaseDate\n    episodeID\n  }\n}","operationName":"getFilm"}'

curl 'http://{ingressIP}/graphql' --data-raw '{"query":"query getFilmHeroes {\n  film(filmId: 1) {\n    title\n    director\n    releaseDate\n    episodeID\n    heroes {\n      name\n      height\n      mass\n      darkSide\n      lightSaber\n    }\n  }\n}","operationName":"getFilmHeroes"}'

curl 'http://{ingressIP}/graphql' --data-raw '{"query":"{\n  allFilms {\n    director\n    episodeID\n    releaseDate\n    title\n    heroes {\n      darkSide\n      height\n      lightSaber\n      mass\n      name\n      surname\n    }\n  }\n}","operationName":null}'

```
## Cleanup

```
Kubectl delete -f yamls/manifest.yaml
Kubectl delete -f yamls/network.yaml

```
