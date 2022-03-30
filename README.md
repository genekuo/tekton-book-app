# tekton-lab-app

## kind create cluster
<code>
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
<code>

## Deploy nginx ingress controller
`kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/main/deploy/static/provider/kind/deploy.yaml`

`kubectl wait --namespace ingress-nginx \
  --for=condition=ready pod \
  --selector=app.kubernetes.io/component=controller \
  --timeout=100s`

`kubectl -n ingress-nginx get pod -o wide`

`kubectl get all`

`kubectl apply --filename https://storage.googleapis.com/tekton-releases/pipeline/latest/release.yaml`

`kubectl apply -f https://storage.googleapis.com/tekton-releases/triggers/latest/release.yaml`
`kubectl apply -f https://storage.googleapis.com/tekton-releases/triggers/latest/interceptors.yaml`

`kubectl apply -f https://raw.githubusercontent.com/tektoncd/triggers/main/examples/rbac.yaml`

`kubectl create clusterrolebinding serviceaccounts-cluster-admin --clusterrole=cluster-admin --group=system:serviceaccounts`

`tkn version`

`npm install`
`npm start`

`curl localhost:3000`

`curl localhost:3000/add/12/10`

`curl localhost:3000/substract/10/2`


`npm run lint`
`npm run test`

`docker build -t <YOUR_USERNAME>/tekton-lab-app .`

`docker login docker.io`

`docker push <YOUR_USERNAME>/tekton-lab-app`

`kubectl apply -f ./deploy.yaml`

`curl localhost`

`server.js` ln 6 change to 
`res.send({ message: "Hello", change: "changed" }).status(200);`

`git commit -am "change a server response three"`
`git push origin main`

`npm run test`
`npm run lint` 

`docker build -t <YOUR_USERNAME>/tekton-lab-app .`
`docker push <YOUR_USERNAME>/tekton-lab-app`

`kubectl rollout restart deployment/tekton-deployment`

`curl localhost`

## Pipeline

`tkn hub install task git-clone`
 
`tkn hub install task npm`

`tkn hub install task kubernetes-actions`

`kubectl apply -f ./task.yaml`

`kubectl apply -f ./pipeline.yaml`

`export TEKTON_SECRET=$(head -c 24 /dev/random | base64)`
`kubectl create secret generic git-secret --from-literal=secretToken=$TEKTON_SECRET`
`echo $TEKTON_SECRET`

`kubectl apply -f ./trigger.yaml`

`kubectl port-forward svc/el-listener 8080`

`brew install ngrok/ngrok/ngrok`
`ngrok http 8080`

## Go to GitHub and add a new webhook to your repository
    Payload URL: This is your public ngrok URL.
    Content Type: This should be changed to application/json.
    Secret: This is the secret you've stored in the $TEKTON_SECRET environment variables.

`server.js` ln 6 change to 
`res.send({ message: "Hello", change: "changed one" }).status(200);`

`git commit -am "Change a server response one"`
`git push origin main`

`tkn pipelineruns ls`

`curl localhost`

`kubectl apply --filename https://github.com/tektoncd/dashboard/releases/latest/download/tekton-dashboard-release.yaml`
`kubectl --namespace tekton-pipelines port-forward svc/tekton-dashboard 9097:9097`

Go to `http://localhost:9097`

 

