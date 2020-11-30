# 표준프레임워크 샘플 프로젝트
## 1. /k8s/deployment.yaml
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: sample-boot2
      labels:
        app: sample-boot2
    spec:
      replicas: 1
      revisionHistoryLimit: 1
      selector:
        matchLabels:
          app: sample-boot2
      template:
        metadata:
          labels:
            app: sample-boot2
        spec:
          containers:
            - name: sample-boot2
              image: dasomel/sample-boot2:version
              ports:
                - containerPort: 8080
              imagePullPolicy: Always
              resources:
                requests:
                  cpu: 0.5
                  memory: 0.5Gi
                limits:
                  cpu: 0.5
                  memory: 0.5Gi
## 2. /k8s/service.yml
    apiVersion: v1
    kind: Service
    metadata:
      name: sample-boot2
    spec:
      type: NodePort
      selector:
        app: sample-boot2
      ports:
      - protocol: TCP
        port: 8080
        targetPort: 8080
        nodePort: 30102
## 3. build & dockerizing
    ./gradlew clean war bootRepackage
    docker build . -t dasomel/sample-boot2-keycloak:1.0.0
    docker push dasomel/sample-boot2-keycloak:1.0.0
## 4. kubernetes build & deploy
    kubectl apply -f ./k8s/deployment.yaml
    kubectl apply -f ./k8s/service.yaml
## 5. Confirm
    
    
    
    