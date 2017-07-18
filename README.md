# kontena

Kontena haproxy with Lets encrypt

haproxy:
  image: kontena/haproxy:acme
  ports:
    - 80:80
    - 443:443
  environment:
    - LE_EMAIL=your@email.com
    - LE_DOMAINS=www.domain.com,api.domain.com
    - BACKENDS=%{project}-lb:80

lb:
  image: kontena/lb:latest

web:
  image: nginx:latest
  environment:
    - KONTENA_LB_VIRTUAL_HOSTS=www.domain.com
  links:
    - lb

api:
  image: my/api:latest
  environment:
    - KONTENA_LB_VIRTUAL_HOSTS=api.domain.com
  links:
    - lb

