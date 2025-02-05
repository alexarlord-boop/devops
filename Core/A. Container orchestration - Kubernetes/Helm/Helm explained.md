# Helm
* package manager for Kubernetes
* helps you define, install, and upgrade complex Kubernetes applications and distribute them in public or private repositories



## Helm charts
* Helm uses a packaging format called charts -- bundle of  YAML config files
* Create and Share / Get and Use

Many complex Database apps, Monitoring apps have helm charts available.

Helm chart directory structure:
```
mychart/
    Chart.yaml   # chart metadata
    values.yaml  # values for the template files
    charts/      # other chart dependencies
    templates/   # actual template files
```

## Usage

Setup:
```bash
brew install helm

helm search <keyword>  # search for charts or use Helm hub
helm install <chartname>
```

Value injection (overwriting defaut values.yaml with custom my-values.yaml):
```bash
helm install --values=my-values.yaml <chartname>
```
| values.yaml | my-values.yaml | result (merged) |
--- | --- | ---
| name: my-app |  | name: my-app |
| image: my-app-image | image: my-app-image-2 | image: my-app-image-2 |
| port: 9001 |  | port: 9001 |


or use a set flag (not preferred):
```bash
helm install --set image=my-app-image-2 <chartname>
```

## Use cases
1. Sharing across teams or public
2. Templating engine 
```yaml
# config_template.yaml
apiVersion: v1
kind: Pod
metadata: 
    name: {{ .Values.name }}
spec:
    containers:
    - name: {{ .Values.name }}
      image: {{ .Values.image }}
      port: {{ .Values.port }}
```

```yaml
# values.yaml
name: my-app
container:
    name: my-app-container
    image: my-app-image
    port: 9001
```
-- practical for CI/CD pipelines: you can have a single template and multiple values files for Build.

3. Deploying same app across different environments
* same chart for dev, staging, prod

4. Release management

## Tiller
Legacy of Helm v2, removed in Helm v3
