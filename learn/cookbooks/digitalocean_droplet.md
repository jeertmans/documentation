# Deploy a Meilisearch instance on DigitalOcean

[[toc]]

## Deploy Meilisearch on a DigitalOcean droplet

### 1. Create a new "droplet"

A "droplet" is a set of resources, as a Virtual Machine, or a Server, in which you can run your own applications.
In any DigitalOcean page, when you are logged in, you will find a menu in the upper-right corner. Click on "Create" -> "Droplets".

![Selecting "Droplets Create cloud servers" from the "Create" dropdown](/digitalocean/01.create.png)

### 2. Select Meilisearch snapshot

By default, DigitalOcean will display the "Distributions" tab. Select the "Marketplace" tab and search for "meili". Select it.

![Searching for 'meili' in Marketplace](/digitalocean/02.marketplace.png)

### 3. Select your plan

Select your plan. Plans start at $5 (click on "See all plans" for more options). Memory-optimized options will give you better results for a production environment on big datasets.

![Selecting the plan based on your usage](/digitalocean/03.select-plan.png)

### 4. Select a region for your droplet

Select the region where you want to deploy your droplet. Remember, the closer you are to your users or customers, the better will be their search experience with Meilisearch.

![Selecting the London data center region](/digitalocean/04.select-region.png)

### 5. Add your SSH key

Select your SSH key in order to be able to connect to your droplet later. If you don't see your SSH key add yours to your account.

If you need help with this, visit [this link](https://www.digitalocean.com/docs/droplets/how-to/add-ssh-keys/to-account/)

You can also set a password for `root` user if you prefer this authentication method, but it is less secure.

![Selecting SSH keys for authentication](/digitalocean/05.add-ssh-key.png)

### 6. Choose your droplet name and tags

Here you can select the name that will be visible everywhere in your DigitalOcean account. Choose wisely! Droplets can only contain alphanumeric characters, dashes, and periods.

![Adding 'meilisearch-droplet-name' as the hostname](/digitalocean/06.droplet-name.png)

Tags are a very good method to know who created resources, and for organizing resources or projects. Tags can contain letters, numbers, colons, dashes, and underscores. Try to always add some tags to make clear what are the server purposes.

![The search bar, meilisearch, and search-team tags](/digitalocean/06.add-tags.png)

### 7. Click on "Create Droplet"

![The "Create Droplet" button](/digitalocean/07.create-droplet.png)

### 8. Your Meilisearch is running (in **development** environment)

Instance creation in progress...

![Progress bar for the meilisearch-droplet-name instance](/digitalocean/08.creating.png)

... done!

![meilisearch-droplet-name instance created successfully](/digitalocean/08.created-ip.png)

### 9. Test Meilisearch

Copy the public IP address:

![meilisearch-droplet-name instance's IP: 165.227.56.77](/digitalocean/09.copy-ip.png)

Paste it in your browser. If this screen is shown, your Meilisearch is now ready!

![Meilisearch search preview](/digitalocean/09.test-meili.png)

## Configure production settings in your Meilisearch droplet

Configuring your Meilisearch in a **production** environment on DigitalOcean droplet is very straightforward. Establish an SSH connection with your droplet and a script will guide you through the process.

### 1. Make your domain name point to your droplet

If you want to use your own domain name (or sub-domain), add `A record` in your domain name provider account.

![An interface for editing DNS records with "Type": A, "Name": droplet, "IPv4 address": 165.227.56.77, and "TTL": Auto](/digitalocean/11.domain-a-record.png)

This should work out of the box. Your domain name should now be linked to your Meilisearch instance. You can now do a health check to verify that your instance is running and your DNS is well configured:

```bash
curl -v http://<your-domain-name>/health
```

The server should answer with a `200 OK` status code and the following body `{"status": "available"}` as shown in the example below:

```bash
...
HTTP/1.1 200 OK
...
{"status": "available"}
...
```

![Meilisearch interface](/digitalocean/11.working-domain.png)

### 2. Set API key and SSL (HTTPS)

Meilisearch is currently running in a **development** environment. You haven't set up an API key, meaning that anyone can read/write from your Meilisearch, and you aren't using HTTPS yet, which makes this configuration unsafe for **production**.
To start the configuration process, connect via SSH to your new Meilisearch Droplet and follow the instructions:

### 2.1. Run the configuration script

Open a terminal and start a new SSH connection with the IP you got from DigitalOcean.

Write in your terminal `ssh root@<your-ip-address>` and press Enter to establish connection:

```bash
ssh root@42.42.42.42
```

Write `yes` and press Enter to accept the authentication process.

A script will run automatically, asking for your settings and desired configuration. If you want to run this script again anytime, you can do so by using the following command:

```bash
meilisearch-setup
```

### 3. Enjoy your ready-to-use Meilisearch droplet

Your Meilisearch Droplet is ready to be used in **production**.

To check if everything is running smoothly, do an HTTP call to the `/health` route:

```bash
curl -v https://<your-domain-name>/health
```

The server should answer with a `200 OK` status code and the following body `{"status": "available"}` as shown in the example below:

```bash
...
HTTP/1.1 200 OK
...
{"status": "available"}
...
```

**Enjoy**!
