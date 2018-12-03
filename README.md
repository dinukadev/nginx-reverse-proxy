# nginx-reverse-proxy
ngnix-reverse-proxy repository holds configuration for the ngnix reverse-proxy server. 

The configuration files comprise of 

<b> nginx.conf </b> : Default configuration file

<b> ssl.conf </b>: Configuration for Https server where ssl is enabled. 

Apart from these the repo consists of 1 more file for Docker configuration to setup the root and configuration for demo setup. 

The main task of the build plan corresponding to this repo is to copy over the ssl configuration to AWS jump host. So to change the 
configuration of any new environment, we create a new branch for the environment and make changes to ssl.conf file and commit it for the
build plan to take over. 

To expose a new web/gw artefact we modify the ssl.conf by adding an entry like below :

#Example : 

```
location ~ ^/messages.* {

             rewrite ^/messages/?(.*)$ /$1 break;

              proxy_pass https://ms-messages:443;
    }
```
### Location
A location can either be defined by a prefix string, or by a regular expression. 
Regular expressions are specified with the preceding "~*" modifier (for case-insensitive matching), or the "~" modifier 
(for case-sensitive matching). To find location matching a given request, nginx first checks locations defined using the prefix strings 
(prefix locations). Among them, the location with the longest matching prefix is selected and remembered. Then regular expressions are 
checked, in the order of their appearance in the configuration file. The search of regular expressions terminates on the first match, 
and the corresponding configuration is used. If no match with a regular expression is found then the configuration of the prefix location 
remembered earlier is used.

### Rewrite directive
A rewrite directive, which usually involves regex and in the below example 
`rewrite ^/messages/?(.*)$ /$1 break; ` means to take everything after /messages/ and append to the proxy-pass url. 

For eg., If we enter the url http://localhost:80/messages/conversations/1
  ngnix replaces it to https://ms-messages:443/conversations/1
  
### A proxy-pass
URL of the microservice. 
    
