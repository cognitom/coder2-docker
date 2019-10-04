# Coder2

[Coder2](https://github.com/cdr/code-server) + some little features:

- [fixuid](https://github.com/boxboat/fixuid)
- Easy SSL setup, kind of
- SSH agent forwarding (only when it runs from terminal, and if the session is still alive)
- Pass your SSH keys to the container
- Pass your Git configs to the container

To build and start Coder, run `bash up`. (If you have not got a SSL certificate yet, check the section below before)

```bash
$ git clone https://github.com/cognitom/coder2-docker.git
$ cd coder2-docker
...you might need some steps in the following sections...
$ bash up
```

## SSL Certificate

Set your own domain name for Coder and your password for authentification:

```bash
$ echo "export CODER_HOST='example.com'" >> ~/.bashrc
$ echo "export CODER_PASS='your-perfect-strong-password'" >> ~/.bashrc
$ source ~/.bashrc
```

Ok, then, let's get your certificate from Let's Encrypt. [lego](https://github.com/go-acme/lego) is exactly the tool for that.

You don't even need to install it. Just run `lego` via Docker:

```bash
$ docker run -it -v "${HOME}/.lego:/.lego" goacme/lego \
    --email="yourname@example.com" \
    --domains=$CODER_HOST \
    --dns=manual \
    run
```

I chose the manual method (`--dns=manual`) in the example command above, but there're many many options. Check it out here: https://go-acme.github.io/lego/dns/

## Git user and email

Before commiting, you need to tell Git your name and email (if you haven't set them yet):

```bash
$ git config --global user.name "John Doh"
$ git config --global user.email "john.doh@example.com"
```

Then, your `.gitconfig` will be shared with the Coder container.
