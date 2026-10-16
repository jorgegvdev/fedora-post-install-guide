# 08 — Desarrollo de software

## Qué es

Entorno para TypeScript/Node, contenedores, bases de datos y editor.

## Por qué así

- Herramientas de desarrollo **nativas**: necesitan ver todo el sistema (compiladores, Docker, tus proyectos).
- **Node con nvm**: cambias de versión por proyecto sin tocar el sistema.
- **Bases de datos en contenedores**: no ensucian Fedora y reproduces el entorno fácil.

## Pasos

### 1. Herramientas de compilación
```bash
sudo dnf group install development-tools
```
Instala gcc, make y afines. Algunos paquetes de npm compilan código nativo y las necesitan.

### 2. Git
```bash
sudo dnf install git
```
```bash
git config --global user.name "Tu Nombre"
```
```bash
git config --global user.email "tu@email.com"
```
```bash
git config --global init.defaultBranch main
```

### 3. Clave SSH (GitHub/GitLab)
```bash
ssh-keygen -t ed25519 -C "tu@email.com"
```
Genera la clave. Luego muéstrala y pégala en GitHub/GitLab:
```bash
cat ~/.ssh/id_ed25519.pub
```

### 4. Terminal moderna
Una por vez:
```bash
sudo dnf install ripgrep
```
```bash
sudo dnf install fd-find
```
```bash
sudo dnf install bat
```
```bash
sudo dnf install eza
```
```bash
sudo dnf install fzf
```
```bash
sudo dnf install tmux
```
```bash
sudo dnf install neovim
```
Para LazyVim: necesita Neovim reciente, `git`, un compilador (paso 1), `ripgrep`, `fd-find` y una Nerd Font. Verifica con `nvim --version`.

### 5. Node con nvm
nvm **no está en los repos de Fedora**; se instala con su script oficial (versión fijada, no `master`):

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.8/install.sh | bash
```
Descarga y ejecuta el instalador. Agrega unas líneas a `~/.bashrc`.

Cierra y abre la terminal, y:
```bash
nvm install --lts
```
Instala la última versión LTS de Node.

```bash
nvm alias default 'lts/*'
```
La deja como predeterminada en cada terminal nueva.

Verificar versión de nvm antes de pinchar el tag: https://github.com/nvm-sh/nvm/releases

### 6. Docker Engine (repo oficial)
Fuente: [docs.docker.com](https://docs.docker.com/engine/install/fedora/). La página cubre Fedora 44 y 43; si usas 45, revísala por si cambió.

Quitar paquetes no oficiales que puedan chocar:
```bash
sudo dnf remove docker docker-client docker-client-latest docker-common docker-latest docker-latest-logrotate docker-logrotate docker-selinux docker-engine-selinux docker-engine
```
(si dice que no están instalados, está bien).

Agregar el repo:
```bash
sudo dnf config-manager addrepo --from-repofile https://download.docker.com/linux/fedora/docker-ce.repo
```

Instalar (es el set oficial: motor, CLI, runtime, buildx y compose):
```bash
sudo dnf install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```
Al pedir aceptar la clave GPG, verifica que termine en `…621E 9F35` (huella `060A 61C5 1B55 8A7F 742B 77AA C52F EB6B 621E 9F35`).

Activar y arrancar:
```bash
sudo systemctl enable --now docker
```

Usar Docker sin `sudo`:
```bash
sudo usermod -aG docker $USER
```
Cierra sesión y vuelve a entrar. **Aviso de seguridad:** el grupo `docker` equivale a acceso root en tu máquina. Es lo habitual en desarrollo local, pero no lo hagas en un equipo compartido.

> **SELinux:** al montar carpetas en un contenedor agrega `:Z` (`-v ./data:/data:Z`) o el contenedor no podrá escribir. Es una de las causas más comunes de "permission denied" en Fedora. No desactives SELinux.

> Alternativa: Fedora ya trae **Podman**, compatible con comandos Docker y sin demonio. Si no necesitas Docker específicamente, puedes usarlo.

### 7. VS Code (repo de Microsoft)
```bash
sudo rpm --import https://packages.microsoft.com/keys/microsoft.asc
```
Importa la clave de firma.

```bash
echo -e "[code]\nname=Visual Studio Code\nbaseurl=https://packages.microsoft.com/yumrepos/vscode\nenabled=1\nautorefresh=1\ntype=rpm-md\ngpgcheck=1\ngpgkey=https://packages.microsoft.com/keys/microsoft.asc" | sudo tee /etc/yum.repos.d/vscode.repo > /dev/null
```
Crea el archivo del repo. Se actualiza con `dnf`.

```bash
sudo dnf install code
```
Nativo (no Flatpak) para que el terminal integrado vea Node, Docker y git.

### 8. Bases de datos en contenedores
Ejemplo PostgreSQL local:
```bash
docker run -d --name pg -e POSTGRES_PASSWORD=cambiame -p 5432:5432 -v pgdata:/var/lib/postgresql/data postgres:17
```
- `-d`: en segundo plano.
- `-v pgdata:…`: volumen para que no pierdas los datos al borrar el contenedor.
- La contraseña es solo para desarrollo local. En proyectos reales, credenciales por variables de entorno y `.env` fuera de git.

SQL Server y MySQL funcionan igual (imágenes `mcr.microsoft.com/mssql/server` y `mysql`). Conéctate con DBeaver ([06](06-apps.md)).

### 9. Nerd Font (opcional, para terminal/Neovim)
Descarga *JetBrainsMono Nerd Font* desde nerdfonts.com y:
```bash
mkdir -p ~/.local/share/fonts
```
Copia los `.ttf` ahí y:
```bash
fc-cache -f
```

## Verificar

```bash
node -v
```
```bash
docker run --rm hello-world
```
Debe imprimir `Hello from Docker!`.

```bash
code --version
```
