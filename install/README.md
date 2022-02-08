# K3d

## Install K3d

The easiest way to get k3d running on Windows is with Chocolatey. To install Chocolatey you can run the following from an administrative PowerShell instance:

```shell
Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))
```

Now close PowerShell and open a new administrative instance and run the following to install k3d and a couple other useful tools:

```shell
choco install k3d -y
choco install jq -y
choco install yq -y
choco install kubernetes-helm -y
```

And now let’s configure tab completion for k3d:

```shell
# Create user profile file if it doesn't exist
if ( -not ( Test-Path $Profile ) ) { New-Item -Path $Profile -Type File -Force }
# Append the k3d completion to the end of the user profile
k3d completion powershell | Out-File -Append $Profile
```

## Using k3d

Start a new PowerShell instance (doesn’t need to be administrative this time around). Now that you have the k3d binary on your path, you can create a cluster by running:

```shell
k3d cluster create localk8s
```

try to create cluster for this workshop

```shell
k3d cluster create my-cluster --servers 1 --agents 3 --port "8888:80@loadbalancer" --port "8889:443@loadbalancer"
```
