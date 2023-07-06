Lemmy-Easy-Deploy
---

Deploy Lemmy the easy way!

Quick Start
---

```
# Install Docker if you don't already have it
# https://docs.docker.com/engine/install/#server

# Clone the repo
git clone https://github.com/ubergeek77/Lemmy-Easy-Deploy

# Change into the directory
cd ./Lemmy-Easy-Deploy

# Check out the latest tag
git checkout $(git describe --tags `git rev-list --tags --max-count=1`)

# Copy config.env.example to config.env
cp ./config.env.example ./config.env

# Make sure the DNS records for your domain point to your server
# Edit the config.env file, and at minimum, change LEMMY_HOSTNAME to be your domain. Then...

# Deploy!
./deploy.sh
```

The default deployment as outlined above will get you running in ***about 1 minute!***

What is this?
---
This repo provides an "out of the box" installer and updater for [Lemmy](https://join-lemmy.org/) that sets up everything for you automatically using Docker Compose. Unlike the Ansible installer, this does not require a Debian-based distribution, and can be run on pretty much any system with Docker installed. By default, [multiarch images built by me](https://github.com/ubergeek77/lemmy-docker-multiarch) will be used, and if those are not available for a given tag, the official images will be used. Since my multiarch images are used by default, users on ARM systems can deploy Lemmy quickly and easily.

Features:

- Beginner friendly
- Near-zero config required
- ARM and ARM64 supported
- Quick and easy Lemmy updates
- Automatic and hands-off HTTPS certificate management
- Post-deployment health checks
- Optionally deploy an SMTP server or use your own external one
- Plenty of configuration for advanced users

Updating
---

After you've deployed Lemmy via Lemmy-Easy-Deploy, simply run `./deploy.sh` again to detect any Lemmy updates and deploy them.

Lemmy-Easy-Deploy will notify you if a Lemmy-Easy-Deploy update is available. I regularly update Lemmy-Easy-Deploy based on feedback to address common issues people have.

Usage & Configuration
---

```
Usage:
  ./deploy.sh [options]

Run with no options to check for Lemmy updates and deploy them

Options:
  -s|--shutdown          Shut down a running Lemmy-Easy-Deploy deployment (does not delete data)
  -l|--lemmy-tag <tag>   Install a specific version of the Lemmy Backend
  -w|--webui-tag <tag>   Install a specific version of the Lemmy WebUI (will use value from --lemmy-tag if missing)
  -f|--force-deploy      Skip the update checks and force (re)deploy the latest/specified version (must use this for rc versions!)
  -r|--rebuild           Deploy from source, don't update the Git repos, and deploy them as-is, implies -f and ignores -l/-w
  -y|--yes               Answer Yes to any prompts asking for confirmation
  -v|--version           Prints the current version of Lemmy-Easy-Deploy
  -u|--update            Update Lemmy-Easy-Deploy
  -d|--diag              Dump diagnostic information for issue reporting, then exit
  -h|--help              Show this help message
```

All deployment files will be placed in the `./live` directory relative to `deploy.sh`. This directory will contain a Docker Compose stack with a **stack name** of `lemmy-easy-deploy`.

**Your Lemmy data will be stored in named Docker volumes in the Docker system directory,** ***not in the `./live` directory.***

This deployment is not "locked into" Lemmy-Easy-Deploy in any way. You can use manage this deployment manually if you wish, but if you continue to use Lemmy-Easy-Deploy, *all autogenerated files in `./live` will be replaced by Lemmy-Easy-Deploy upon re-deployments*.

The `.env` files in `./live` contain ***important passwords/secrets.*** *Do not delete these!*

---

Configuration of Lemmy-Easy-Deploy is done via `config.env`. Check `config.env.example` for detailed info about what each configuration option does.

If you are an advanced user, and need to specify a certain environment variable for a given service in this deployment, you can define them in any of the below files. If one of thoese files exist, they will be passed to each respective service as an `env_file`. This allows you to specify any environment variables you want for any service. These files follow the [Docker environment file syntax](https://docs.docker.com/compose/environment-variables/env-file/) (basically just `VAR=VAL`).

```
# Will be loaded by the 'proxy` service (Caddy)
./custom/customCaddy.env

# Will be loaded by the 'lemmy' service
./custom/customLemmy.env

# Will be loaded by the 'lemmy-ui' service
./custom/customLemmy-ui.env

# Will be loaded by the 'pictrs' ervice
./custom/customPictrs.env

# Will be loaded by the 'postgres' service
./custom/customPostgres.env

# Will be loaded by the 'postfix' service
./custom/customPostfix.env
```

If you want to use a **custom Postgres configuration**, as mentioned by the [database tweaks section of the Lemmy documentation](https://join-lemmy.org/docs/administration/install_docker.html#database-tweaks), place your custom configuration at this path:

```
./custom/customPostgresql.conf
```

It will be passed to the Postgres container and override the default config.

**If you make any changes to `config.env` or any of the custom files as listed above, redeploy your instance with `./deploy.sh -f` to apply the changes.**

FAQ & Troubleshooting
---

[Please see the troubleshooting page for answers to common questions and solutions to common problems.](TROUBLESHOOTING.md)

Credits
---

- The Lemmy project maintainers, for making Lemmy: https://github.com/LemmyNet/lemmy
- [@QPixel](https://github.com/QPixel), for helping me QA test this

Support me
---

I ***am not*** a maintainer or contributor to the Lemmy project. Lemmy does not belong to me and I did not make it. But, if my script helped you, and you would like to support me, I have crypto addresses:

- Bitcoin: `bc1qekqn4ek0dkuzp8mau3z5h2y3mz64tj22tuqycg`
- Monero/Ethereum: `0xdAe4F90E4350bcDf5945e6Fe5ceFE4772c3B9c9e`

