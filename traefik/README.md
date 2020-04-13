# Traefik v2.2 deployment

These kubernetes manifests can be used to deploy Traefik v2.2.
With the current configuration a Traefik loadbalancer is deployed on all nodes with equal configurations to the namespace **kube-system**.

## HTTPS and SSL
With the current configuration Traefik requests certificates from Letsencrypt through a TLS challenge. Alternative challenges are described here: https://docs.traefik.io/https/acme/
If possible, a DNS challenge should be the desired option.

All http traffic can be redirected to https directly at the entrypoint by uncommenting the argument in the `deployment.yml`:
```
- --entrypoints.web.http.redirections.entryPoint.to=websecure
```
Alternatively Middleware can be used to do the redirection on a per service selection.
**Make sure to adjust the ACME challenge parameteres to match your configuration**

## Sample service
Find a sample configuration for a service in the **sample-service.yml** file.
Adjust the domain to a proper one and attach it to your service. The following lines are responsible for that:
```
services:
- name: sample
port: 80
```
If a public domain is listed in `- match: Host(`sample-service.com`)` Traefik will automatically get a valid certificate for the domain and ship the certificate on request. 
Also, this configuration should score an A+ on ssllabs.com [Qualys SSL test](https://www.ssllabs.com/ssltest/).