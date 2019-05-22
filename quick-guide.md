Istio Setup Guide

All contents based on https://istio.io/docs/setup/kubernetes/

1. Download and Install

  curl https://raw.githubusercontent.com/kubernetes/helm/master/script/get > get_helm.sh
  chmod 700 get_helm.sh
  ./get_helm.sh
  
  cd ~
  curl -L https://git.io/getLatestIstio | sh -
  cd istio-{version}
 

2. Add istioctl PATH registration
  
  export PATH=$PWD/bin:$PATH

3. Helm init
  
  kubectl create -f install/kubernetes/helm/helm-service-account.yaml
  helm init --service-account tiller
  
  (Then, if there is "no resources " when you type kubectl get --all-namespace, you must create namespace)

4. Create namespace for the istio-system components
  
  kubectl create namespace istio-system
  helm template install/kubernetes/helm/istio-init --name istio-init --namespace istio-system | kubectl apply -f -
  
 5. Install Istio via Helm
 (when installing, add installing kiali, servicegraph, grafana)
 
  helm install install/kubernetes/helm/istio \
  --name istio \
  --namesapce istio-system \
  --set tracing.enabled=true \
  --set global.mtls.enabled=true \
  --set grafana.enabled=true \
  --set kiali.enabled=true \
  --set servicegraph.enabled=true
  
  6. Check whether installation is valid
    kubectl get po -n istio-system
    
