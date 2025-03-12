# CTF issue
mitmproxy had a problem that it would sometimes redirect the url to fingerprint.byu.edu, such that fingerprint.byu.edu showed up in the browser, instead of forwarding traffic to it. However, sometimes this would not be the problem, at least at first after restarting the proxy. 

For the CTF we integrated mitmproxy with a custom script to fix this, but then realized that the custom script was now forwarding the traffic instead of mitmproxy.

# Setup
Here are the steps to set up mitmproxy:
1. Set up a python virtual environment.
    - ```python3 -m venv myenv```
    - source myenv/bin/activate
2. pip install mitmproxy
3. Download the certificates using certbot. See [Certificates.md](Certificates.md).
4. nohup mitmdump –mode reverse:https://fingerprint.byu.edu –listen-host 0.0.0.0 –listen-port 443 --set block_global=false --certs mitmproxy.httpcarrierpigeons.xyz=/etc/letsencrypt/live/mitmproxy.httpcarrierpigeons.xyz/combined.pem
    - Note that during the ctf we had a ssl-insecure option as well. It turned out that we did not need it. It is possible that removing it would have prevented the redirects.s

# Testing
Steps to test after setting up with previous steps:
1. source myenv/bin/activate
2. mitmdump –mode reverse:https://fingerprint.byu.edu –listen-host 0.0.0.0 –listen-port 443 --set block_global=false --certs mitmproxy.httpcarrierpigeons.xyz=/etc/letsencrypt/live/mitmproxy.httpcarrierpigeons.xyz/combined.pem

Note that the nohup command has been ommitted from the testing command. Nohup redirects output, such as error messages, to a nohup.out file instead of the terminal, thus making testing a little more tedious. The utility of nohup is that it causes the proxy to continue running after the shell is terminated, rather than terminating together with the shell which is the default behavior.

