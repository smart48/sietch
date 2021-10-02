## Step 1: Get the information needed

Decide on the domain that you would like to use for your server. e.g. www.example.com.

Decide on a name for your server. You'll use this as a DNS entry to point to the server.

The server name is more important for later steps where you'll have more than one server.

Naming servers can follow any naming system you want.

For this example, we are going to be boring and use the simple ```srv01```will then have a domain name
of ```srv01.example.com```.

## Step 2: Create a virtual server

Log into your [DigitalOcean](https://m.do.co/c/179a47e69ec8) and select the droplets tab.

![DO Droplets Tab](images/DO_droplets_btn.png)

Then click the create button and select Droplets - Create cloud servers.

![DO Create Button](images/DO_create_btn.png)

You should now see the virtual server creation page.

For the example, we are just going to create the smallest server possible, though you may need to select a larger one if
you need more performance.

We'll be using Ubuntu 20.04 mainly as it's the long term release version.

Please pick the distribution you know.

Pick the region that is closest to your clients or meets your requirements the best.

To follow [POPIA](https://popia.co.za), I'm using a region that also follows
[GDPR](https://gdpr-info.eu/).

So I've picked Frankfurt.

Under additional options, select IPv6 and Monitoring.

![DO Create droplet additional options](images/DO_droplet_aditional_options.png)

Under authentication, select or add your SSH key.

Now add the hostname with the domain that you chose above as the server hostname. e.g. ```srv01.example.com```

![DO Create droplet hostname](images/DO_droplet_hostname.png)

DigitalOcean will create PTR records pointing back to the servers IP's. Some service use this to validate your server,
so it's a good idea to get it correct.

Now click the create button to finalise.

![DO Create droplet final create](images/DO_droplet_final_create.png)

DigitalOcean will start creating your virtual server and take you to a page showing the creation progress.

Wait for the server to finish being created, then continue with the next step.

## Step 3: Setup DNS

After your server has finished creating, open up the new server page.

You should have both an IPv4 and IPv6 address for the server at the top of the page.

If you don't have the IPv6 IP, you can click the 'Enable now button'.

![DO Create droplet final create](images/DO_droplet_ips.png)

Grab these two IP's and head over to your DNS provider.

I'll be going over how to do this with [Cloudflare](https://www.cloudflare.com/), but the steps should be the same for
most other providers.

Now using the IP's create both A records, using the IPv4, and AAAA records, using the IPv6, for the following entries.

If you are using [Cloudflare](https://www.cloudflare.com/), make sure the proxy is disabled for now.

Click on the orange cloud when adding the entry to make it grey.

![CF Proxy enabled](images/CF_proxy_enabled.png)

![CF Proxy disabled](images/CF_proxy_disabled.png)

* root domain
  *, e.g. ```example.com```
* ```www.example.com```
* server name that you picked previously
  *, e.g. ```srv01.example.com```

These will allow your users to get to your site on the two most common names.

Later, once you have more than one server, it is easier to connect to the specific server.

Once you have done this, you can test that everything is working by doing a lookup of the domain name.

```dig example.com```

Also, confirm that the PTR record exists. (This should match the server name you set)

```dig -x <IP>```

## Step 4: Setup the server

Ok, first ssh to the server as the root user.

```ssh root@example.com```

As you have used a key, there should be no password required.

**For any commands you use going forward. Please copy them into a separate file, so you have a history.**

You'll use this file at a later stage when we start automating the server setup.

It's also good to have a record of exactly your server is set up.

An example can be found at this link:

[setupCommands.sh](./setupCommands.sh)