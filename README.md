# NGINX Reverse-proxy Maintenance Configuration

## Description
This directory contains a special reverse-proxy maintenance configuration for NGINX. It allows you to enable or disable maintenance mode by using a semaphore. 

Rules when maintenance_mode is enabled:

|                | not_allowed_endpoint | allowed_endpoint |
|----------------|----------------------|------------------|
| not_allowed_ip |    response is 503   |  normal operation |
| allowed_ip     |    normal operation   |  normal operation |
|                |                      |                  |

When maintenance_mode is disabled, all IPs can access all the endpoints (normal operation

## Installation
Copy the contents of this repo into `/etc/nginx/maintenance`

```bash
git clone https://github.com/rtisma/nginx-maintenace ~/nginx-maintenance
sudo mv ~/nginx-maintenace /etc/nginx/maintenance
sudo ln -s /etc/nginx/maintenance/reverse-proxy.conf /etc/nginx/sites-enabled/reverse-proxy.conf
```

## Configuration:
### Not allowed IPs
By adding IPs and their value (0 = allowed, 1=not_allowed) to the `not_allowed.conf` file, upon NGINX reload (`sudo ./maintenance-reload`)
those IPs that have the value 1 will return a 503 response for not_allowed_endpoints, while all others IPs will function
as normal (refer the Rules table in the description).

### Allowed Endpoints
You can configure the allowed endpoints in a long regex. This means that any IP can use these endpoints. 
Any other endpoint is considered a `not_allowed_endpoint`. Upon updating this, nginx needs to be reloaded

## Usage:

`sudo service nginx status`: Checks if nginx service is running
`sudo service nginx start`: Starts nginx service
`sudo ./maintenance-start`: Enables maintenance mode
`sudo ./maintenance-stop`: Disables maintenance mode
`sudo ./maintenance-status`: Check is maintenance mode is enabled
`sudo ./maintenance-reload`: Reloads NGINX. This needs to be called after updating `not_allowed.conf` or `reverse-proxy.conf`




