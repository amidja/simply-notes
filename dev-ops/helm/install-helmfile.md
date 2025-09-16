https://medium.com/@kapare.sushant23/use-helmfile-for-managing-helm-charts-4fdddcc7ddb9

curl -LO https://github.com/roboll/helmfile/releases/download/v0.135.0/helmfile_linux_amd64
mv helmfile_linux_amd64 helmfile
chmod 777 helmfile
sudo mv helmfile /usr/local/bin
helmfile --version