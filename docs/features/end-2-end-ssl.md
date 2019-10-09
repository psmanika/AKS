## End-to-End SSL

In order to secure both connection between Clients and App Gateway and App Gateway and Pods, you need to setup SSL on both App Gateway and Pod.

To enable SSL on the App Gateway, just provide a tls secret in the ingress resource. This will enable HTTPS on the App Gateway endpoint.

To enable App Gateway to use HTTPS while connecting to the Pods, you need specify the protocol explicitly by using `appgw.ingress.k8s.io/backend-protocol: "https"`.

Here is an example for the same:

### Example
```yaml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: go-server-ingress-timeout
  namespace: test-ag
  annotations:
    kubernetes.io/ingress.class: azure/application-gateway
    appgw.ingress.kubernetes.io/backend-protocol: "https"
spec:
  tls:
  - secretName: go-server-secret
  rules:
  - http:
      paths:
      - path: /hello/
        backend:
          serviceName: go-server-service
          servicePort: 443
```