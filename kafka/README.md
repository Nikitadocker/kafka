### Step 1
```
kubectl create namespace kafka-demo
kubectl create -f 'https://strimzi.io/install/latest?namespace=kafka-demo' -n kafka-demo
kubectl apply -f kafka-single-node.yaml -n kafka-demo
kubectl apply -f kafka-user.yaml -n kafka-demo
kubectl apply -f kafka-topic.yaml -n kafka-demo
```

### Step 2
```
kubectl -n kafka-demo get secret my-cluster-cluster-ca -o jsonpath='{.data.ca\.crt}'  | base64 -d > /tmp/ca.crt
kubectl -n kafka-demo get secret akhq -o jsonpath='{.data.user\.password}' | base64 -d > /tmp/user.password 
kubectl -n kafka-demo get secret akhq -o jsonpath='{.data.user\.p12}'  | base64 -d > /tmp/user.p12  
```

### Step 3
```
helm repo add akhq https://akhq.io/
helm upgrade --install my-akhq akhq/akhq \
--version 0.25.1 \
--values akhq-values.yaml \
-n kafka-demo
```