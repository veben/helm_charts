# Mario Bros Helm Chart

<img src="mario_bros_helm_chart.drawio.svg" alt="mario_bros_helm_chart.drawio.svg" style="width:500px;height:auto;">

## I. Configuration
Modify `values.yaml` file as needed

## II. Installation
> release=mario-bros; chart=helm_chart_mario
```sh
helm install mario-bros ./
```

## III. Test
After installation, an output like that is displayed:
```text
NAME: mario-bros
LAST DEPLOYED: Sun Apr 14 15:49:50 2024
NAMESPACE: default
STATUS: deployed
REVISION: 1
TEST SUITE: None
NOTES:
1. Get the application URL by running these commands:
    NOTE: It may take a few minutes for the LoadBalancer IP to be available.
    You can watch the status of by running 'kubectl get --namespace default svc -w mario-bros'
  export SERVICE_IP=$(kubectl get svc --namespace default mario-bros -o=jsonpath='{.status.loadBalancer.ingress[0].ip}')
  export HOST_NAME=$(kubectl get svc --namespace default mario-bros -o=jsonpath='{.status.loadBalancer.ingress[0].hostname}')
  echo http://$SERVICE_IP:80
  echo http://$HOST_NAME:80
```

Connect to the game via IP or hostname

## IV. Uninstallation
```sh
helm uninstall mario-bros
```