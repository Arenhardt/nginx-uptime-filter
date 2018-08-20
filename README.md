# Nginx uptime filter

It is an serie of Nginx configuration files that allow you filter requests made from the major uptime platforms. You can use that to allow only a country access (using GeoIP) to your application and the Pingdom too, for example. It's preatty easy to use and very helpfull.

We will maintain the IPv4 and IPv6 address range for all the providers with the option to use each one or both.

# Installation

`git clone https://github.com/Arenhardt/nginx-uptime-filter.git`

Pingdom IPv4 install on Debian based SO (Debian, Ubuntu, Kubuntu, etc):

`sudo cp -r nginx-uptime-filter/nginx/conf.d/pingdomv4.conf /etc/nginx/conf.d/pingdom.conf`

For IPv6 filter:

`sudo cp -r nginx-uptime-filter/nginx/conf.d/pingdomv6.conf /etc/nginx/conf.d/pingdom.conf`

And for both (IPv4 and IPv6):

`sudo cp -r nginx-uptime-filter/nginx/conf.d/pingdomv4_v6.conf /etc/nginx/conf.d/pingdom.conf`

Than follow the examples to usage in the Nginx global environment or inside a single vhost.

# Examples

See some usage examples

Deny example if the requester is a Pingdom agent:
```
if ($is_pingdom = 1) {
    # your stufs here if the request is made from Pingdom
    ex: Deny
    return 403 "You shaw not pass!";
}
```

Allow example if the requester is an UptimeRobot agent:
```
if ($is_uptimerobot = 1) {
    # your stufs here if the request is made from Pingdom
    ex: Allow
    return 200 "Ok, i'm alive";
}
```

Complex flow:
```
# verify if is a Pingdom request
if ($is_pingdom = 1) {
    set $rule 1$rule;
}
# verify if request method is GET
if ($request_method = GET ) {
    set $rule 2$rule;
}
# if is Pingdom and the request method is GET, so rewrite
if ($rule = "21"){
    rewrite . https://domain.com/ permanent;
}
```

Other one:
```
# verify if is a Pingdom request
if ($is_pingdom = 1) {
    set $rule 1$rule;
}
# verify if is a UptimeRobot request
if ($is_uptimerobot = 1) {
    set $rule 2$rule;
}
# verify if request method is POST
if ($request_method = GET ) {
    set $rule 3$rule;
}
# if is UptimeRobot, the request method is GET and is not Pingdom, so rewrite
if ($rule = "32"){
    rewrite . https://domain.com/no_pingdom.html permanent;
}
```

# TODO

- Installation system
- Add more providers
- Script to update the providers base
