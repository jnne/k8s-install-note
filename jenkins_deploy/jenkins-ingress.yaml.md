apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: jenkins
  namespace: jenkins-project
spec:
  rules:
  - host: k8s-jenkins.fengjr.com
    http:
      paths:
      - backend:
          serviceName: jenkins-slb
          servicePort: 80
        path: /
