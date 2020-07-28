

1. Download istio  (https://istio.io/latest/docs/setup/getting-started/)

 -----> curl -L https://istio.io/downloadIstio | sh -

 ----->	cd istio-1.6.5
 
 -----> export PATH=$PWD/bin:$PATH  (move istoctl to /usr/local/bin i.e sudo mv bin/istioctl /usr/local/bin ) 

2. Install istioctl

 -----> istioctl install --set profile=demo

 -----> Enable sidecar injection  i.e  ---> kubectl label namespace default istio-injection=enabled

3. Deploy springboot-demo-istio.yml inside k8s of this repo

 -----> cd k8s
 
 -----> kubectl apply -f springboot-demo-istio.yml

 -----> kubectl get pods

 -----> kubectl get svc

4. Determining the ingress IP and ports (https://istio.io/latest/docs/tasks/traffic-management/ingress/ingress-control/#determining-the-ingress-ip-and-ports)

-----> kubectl get svc istio-ingressgateway -n istio-system

5. access the gateway using the service’s node port. Set the ingress ports: 

----> export INGRESS_PORT=$(kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.spec.ports[?(@.name=="http2")].nodePort}')
----> export SECURE_INGRESS_PORT=$(kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.spec.ports[?(@.name=="https")].nodePort}')
----> export TCP_INGRESS_PORT=$(kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.spec.ports[?(@.name=="tcp")].nodePort}')

Setting the ingress IP depends on the cluster provider (minikube here)

----- export INGRESS_HOST=$(minikube ip)

6. Define the ingress gateway for the application:     eg: (https://istio.io/latest/docs/examples/bookinfo/)

 -----> cd k8s
 
 -----> kubectl apply -f springboot-gateway.yml

 -----> kubectl get gateway

 -----> export GATEWAY_URL=$INGRESS_HOST:$INGRESS_PORT

 ------> echo ${GATEWAY_URL}

7. Verify , open browser and search for 

 ----> http://${GATEWAY_URL}/api/v1/hello  eg: http://192.168.99.100:30908/api/v1/hello

  output : Welcome to Service App!!


	
 
  


