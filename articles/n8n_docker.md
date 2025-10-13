# Complete Guide to Installing n8n with Docker on Windows 11

![n8n workflow automation platform running in Docker on Windows 11](https://user-gen-media-assets.s3.amazonaws.com/seedream_images/f75595ea-3d51-41f7-8890-0138a751d8a2.png)

n8n workflow automation platform running in Docker on Windows 11

Are you tired of repetitive tasks eating up your productive time? Meet n8n - a powerful, open-source workflow automation tool that can connect your favorite apps and automate complex workflows, all running locally on your Windows 11 machine.

## Why Choose n8n Over Other Automation Tools?

n8n stands out from cloud-based alternatives like Zapier or Microsoft Power Automate because it offers:

- **Complete data privacy** - Everything runs on your local machine
- **Zero monthly costs** - Completely free and open-source
- **Visual workflow builder** - Intuitive drag-and-drop interface
- **200+ integrations** - Connect with popular services and APIs
- **Custom code support** - JavaScript functions for advanced logic
- **Full control** - No vendor lock-in or service limitations


## What You'll Achieve

By the end of this guide, you'll have:

- A production-ready n8n instance running in Docker
- Persistent data storage that survives container restarts
- Basic authentication configured for security
- Knowledge of maintenance and troubleshooting
- A foundation for building powerful automation workflows


## Prerequisites

Before we begin, ensure you have:

- **Windows 11** with WSL 2 enabled
- **Docker Desktop** installed and running with WSL 2 backend
- **Basic PowerShell knowledge** for running commands


### Installing Docker Desktop

If you haven't installed Docker Desktop yet:

1. Download from Docker's official website
2. During installation, ensure **"Use WSL 2 instead of Hyper-V"** is selected
3. After installation, verify in PowerShell:
```powershell
docker --version
docker compose version
```

You should see version information for both commands.

## Step 1: Create Your Project Structure

First, let's create a dedicated directory for our n8n installation:

```powershell
mkdir n8n-docker
cd n8n-docker
```

This folder will contain:

- `docker-compose.yml` - Our container configuration
- `n8n_data/` - Persistent storage for workflows (created automatically)
- `.env` - Environment variables (optional but recommended)


## Step 2: Create the Docker Compose Configuration

Create a `docker-compose.yml` file with this modern, production-ready configuration:

```yaml
services:
  n8n:
    image: n8nio/n8n:latest
    restart: always
    ports:
      - "5678:5678"
    environment:
      - N8N_BASIC_AUTH_ACTIVE=true
      - N8N_BASIC_AUTH_USER=admin
      - N8N_BASIC_AUTH_PASSWORD=change_this_password
      - N8N_HOST=localhost
      - N8N_PORT=5678
      - N8N_PROTOCOL=http
      - NODE_ENV=production
      - WEBHOOK_URL=http://localhost:5678/
    volumes:
      - ./n8n_data:/home/node/.n8n
    healthcheck:
      test: ["CMD", "wget", "--quiet", "--tries=1", "--spider", "http://localhost:5678/healthz"]
      interval: 30s
      timeout: 10s
      retries: 3
```


### Configuration Breakdown

- **`ports`**: Maps your local port 5678 to the container's port 5678
- **`environment`**: Sets up basic authentication and runtime configuration
- **`volumes`**: Persists all n8n data in the local `./n8n_data` folder
- **`restart: always`**: Automatically starts the container when Docker starts
- **`healthcheck`**: Monitors container health for reliability


## Step 3: Launch n8n

From inside your `n8n-docker` folder, run:

```powershell
docker compose up -d
```

This command will:

1. Download the latest n8n Docker image
2. Create the container with your configuration
3. Start the service in background mode
4. Create the `n8n_data` folder for persistent storage

## Step 4: Access Your n8n Instance

Open your web browser and navigate to **http://localhost:5678**.

You'll be prompted to log in with:

- **Username**: `admin`
- **Password**: `change_this_password`

**Important**: Change these default credentials immediately for security!

## Essential Docker Commands

Learn these commands to manage your n8n installation:

```powershell
# View running containers
docker ps

# Stop n8n (keeps data safe)
docker compose down

# Start n8n again
docker compose up -d

# Restart the service
docker compose restart

# View logs for troubleshooting
docker compose logs -f

# Update to the latest version
docker compose pull
docker compose up -d
```


## Security Best Practices

### Use Environment Variables

Create a `.env` file in your `n8n-docker` folder for sensitive data:

```bash
N8N_BASIC_AUTH_USER=your_secure_username
N8N_BASIC_AUTH_PASSWORD=your_very_secure_password
N8N_ENCRYPTION_KEY=a_32_character_encryption_key_here
```

Then update your `docker-compose.yml` to reference the environment file.

### Network Access Control

By default, n8n only accepts connections from localhost. To access from other devices on your network:

```yaml
ports:
  - "0.0.0.0:5678:5678"  # Listen on all network interfaces
```

**Warning**: Only do this on trusted networks and with strong authentication.

## Troubleshooting Common Issues

### Docker Desktop Not Running

**Symptoms**: Commands fail with "Cannot connect to Docker daemon"

**Solution**:

1. Check the system tray for the Docker whale icon
2. If not running, open Docker Desktop from the Start menu
3. Wait until status shows "Docker Engine is running"

### WSL 2 Backend Problems

**Symptoms**: Docker Desktop fails to start or shows WSL errors

Check WSL status:

```powershell
wsl --list --verbose
```

Fix WSL issues:

```powershell
# Restart WSL completely
wsl --shutdown

# Then restart Docker Desktop
```


### Port Already in Use

**Symptoms**: "Port 5678 is already allocated"

**Solution**: Change the port in your `docker-compose.yml`:

```yaml
ports:
  - "5679:5678"  # Use port 5679 instead
```

Then access via http://localhost:5679.

## Backup and Maintenance

### Creating Backups

All your workflows are stored in `./n8n_data/`. Create regular backups:

```powershell
# Create a timestamped backup
$date = Get-Date -Format "yyyy-MM-dd"
Compress-Archive -Path ./n8n_data -DestinationPath "n8n-backup-$date.zip"
```


### Updating n8n

```powershell
# Pull the latest image
docker compose pull

# Restart with the new version
docker compose up -d

# Verify the update
docker compose logs
```


## Getting Started with Workflows

Now that n8n is running, here's how to create your first automation:

### Explore the Interface

- **Workflows tab**: Create and manage your automations
- **Credentials**: Store API keys and authentication details
- **Executions**: Monitor workflow runs and debug issues


### Build Your First Workflow

Try this simple example:

1. Create a new workflow
2. Add a "Schedule Trigger" node (runs on a timer)
3. Add an "HTTP Request" node to fetch data
4. Add a "Set" node to transform the data
5. Save and activate your workflow

### Essential Nodes to Know

- **Trigger nodes**: Webhook, Schedule, Manual
- **Action nodes**: HTTP Request, Email, File operations
- **Logic nodes**: IF, Switch, Set
- **App nodes**: Google Sheets, Slack, GitHub, and 200+ more


## Next Steps and Resources

### What to Explore Next

1. Browse example workflows at n8n.io/workflows
2. Connect your favorite apps using the extensive node library
3. Set up webhooks to trigger workflows from external services
4. Learn JavaScript expressions for advanced data manipulation
5. Join the community for tips and inspiration

### Helpful Resources

- Official n8n Documentation
- Community Forum
- YouTube Tutorials
- Workflow Templates
- Docker Documentation


## Conclusion

With n8n running locally on your Windows 11 machine, you now have the power to automate repetitive tasks, connect different services, and build complex workflows - all without monthly subscription fees or data privacy concerns.

Start small with simple automations like:

- Backing up files to cloud storage
- Monitoring websites for changes
- Synchronizing data between different tools
- Sending automated notifications

As you become more comfortable, you can build sophisticated multi-step workflows that save hours of manual work. Happy automating! If this guide helped you get started with n8n, consider sharing it with others who might benefit from workflow automation.


