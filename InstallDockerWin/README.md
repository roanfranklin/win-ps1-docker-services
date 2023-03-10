## win-ps1-docker-services

### Instalar o serviço de containers pelo PowerShell
```ps1
Install-WindowsFeature -Name Containers
```

### Resetar o Servidor
```ps1
Restart-Computer -Force
```

### Install Docker
- Instalando repositório
- Instalando o docker
- Instalando o docker-compose
```ps1
Install-Module -Name DockerMsftProvider -Repository PSGallery -Force
Install-Package -Name docker -ProviderName DockerMsftProvider -Force -RequiredVersion 19.03
Invoke-WebRequest "https://github.com/docker/compose/releases/download/v2.15.0/docker-compose-windows-x86_64.exe" -UseBasicParsing -OutFile $Env:ProgramFiles\Docker\docker-compose.exe
```

### Habilita o Docker para iniciar automaticamente
```ps1
Start-Service docker
```

### Commands docker
```ps1
docker.exe image ls
docker.exe ps -a
```

### Reconfigura o Docker para aceitar API
```ps1
Stop-service docker

dockerd --unregister-service
dockerd --register-service -H npipe:// -H 0.0.0.0:2375

Start-Service docker

New-NetFirewallRule -Name "Dockerinsecure2375" -DisplayName "Docker insecure on TCP/2375" -Protocol tcp -LocalPort 2375 -Action Allow -Enabled True
```

### Liberando no Firewall as portas 80 (HTTP) e 443 (HTTPS)
```ps1
New-NetFirewallRule -Name "TraefikHTTP80" -DisplayName "Traefik insecure on TCP/80" -Protocol tcp -LocalPort 80 -Action Allow -Enabled True
New-NetFirewallRule -Name "TraefikHTTPS443" -DisplayName "Traefik secure on TCP/443" -Protocol tcp -LocalPort 443 -Action Allow -Enabled True
```