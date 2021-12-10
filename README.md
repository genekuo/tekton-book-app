# tekton-lab-app

## kind create cluster

cat <<EOF | kind create cluster --config=-
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
- role: control-plane
  kubeadmConfigPatches:
  - |
    kind: InitConfiguration
    nodeRegistration:
      kubeletExtraArgs:
        node-labels: "ingress-ready=true"
  extraPortMappings:
  - containerPort: 80
    hostPort: 80
    protocol: TCP
  - containerPort: 443
    hostPort: 443
    protocol: TCP
EOF

kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/main/deploy/static/provider/kind/deploy.yaml

kubectl wait --namespace ingress-nginx \
  --for=condition=ready pod \
  --selector=app.kubernetes.io/component=controller \
  --timeout=90s

kubectl -n ingress-nginx get pod -o wide

kubectl get all

kubectl apply --filename https://storage.googleapis.com/tekton-releases/pipeline/latest/release.yaml

kubectl apply -f https://storage.googleapis.com/tekton-releases/triggers/latest/release.yaml 
kubectl apply -f https://storage.googleapis.com/tekton-releases/triggers/latest/interceptors.yaml

kubectl apply -f https://raw.githubusercontent.com/tektoncd/triggers/main/examples/rbac.yaml

kubectl create clusterrolebinding serviceaccounts-cluster-admin --clusterrole=cluster-admin --group=system:serviceaccounts

tkn version

npm install
npm start

curl localhost:3000 
{"message":"Hello","change":"here"} 
curl localhost:3000/add/12/10 
{"result":22} 
curl localhost:3000/substract/10/2 
{"result":8} 

npm run lint
npm run test

docker build -t genedocker/tekton-lab-app .

docker login docker.io

docker push genedocker/tekton-lab-app

kubectl apply -f ./deploy.yaml

curl localhost

server.js ln 6 change to 
res.send({ message: "Hello", change: "changed" }).status(200);

git commit -am "Change a server response"
git push origin main

