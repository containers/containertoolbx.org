Toolbx website
==============

To set up Hugo locally:

1) Set up Toolbx container and clone the repo
```bash
toolbox create toolbx-website --release 42
toolbox enter toolbx-website
mkdir -p src && cd src
git clone git@github.com:containers/containertoolbx.org.git
cd containertoolbx.org
```

2) Install Hugo and run the server
```bash
sudo dnf install -y hugo
hugo server -D
```

The CI should deploy the site automatically to [https://containertoolbx.org](https://containertoolbx.org).
