# Website Certificates blocked by BYU's firewall
Some proxies were not able to retrieve website certificates from within BYU's infrastructure.
Evilginx is supposed to automatically retrieve a certificate when enabling a phishlet. It seemed to sometimes work and other times not.

When setting up Muraena and using the command-line tool certbot, the error messages showed that certain temporary urls on the server had their connection reset (even though you could copy and paste those same urls into the browser or curl them and they worked fine). We believe that BYU's firewall blocks bots and detected Let's Encrypt (the certificate issuer) as a bot.

# Get Certificates
## certbot certificate
To get a certificate using certbot, start an apache server, then follow instructions from https://muraena.phishing.click/infra/run. This URL recommends using certbot with the --manual option, which means manually creating files on the apache server. The --manual can be omitted to automate this process.

Here are the full steps:
1. Make sure there are no proxies or websites running, since certbot needs to use the website ports.
2. certbot certonly -v --server https://acme-v02.api.letsencrypt.org/directory --agree-tos -d httpcarrierpigeons.xyz
3. Select option 1 for having certbot do the steps automatically.
    - The output will show the location of the stored certificates.
4. cat fullchain.pem privkey.pem > combined.pem
    - Some proxies need a combined.pem file that has both the private key and the full chain in it.


## How to get certificates using zerossl 
The zerossl certificate authority lets us use the website directly to get a certificate without worrying about firewall rules on our server.

Here is how to get a certificate using zerossl:

We got the certificates using zerossl. Our domain registrar is godaddy. Someone will need to be given access to the godaddy account. 

Zerossl will have us set up a temporary cname in godaddy. Zerossl has a glitch that when you click the copy button for the cname value, there is a space at the end of the cname url. Make sure to remove this before submitting the cname to godaddy or an error will come up and it will fail. 

# Workaround, use /etc/hosts
For some reason, muraena still would not work with the valid certificates from Zerossl that worked in Apache. For testing purposes, we had to change the /etc/hosts to map a url to the ip of the proxy server. Since our proxy server had the domain name httpcarrierpigeons.xyz, for some reason that made it so that we could not use httpcarrierpigeons.xyz in our /etc/hosts, it just would not work. We could use any other domain name though. For testing purposes I used phishing.click. I did not test this on the phone since editing /etc/hosts on a phone is more tricky.