# Grafana Unity Digital Egiz

This project contains a complete Grafana setup with a Unity WebGL panel plugin for digital twin visualization and interaction.

## Project Structure

```
grafana/
├── docker-compose.yml              # Docker Compose configuration for Grafana
├── unity-plugin-for-grafana/       # Unity panel plugin source code
│   ├── src/                        # Plugin source files
│   │   ├── components/            # React components
│   │   ├── utils/                 # Utility functions and types
│   │   ├── module.ts              # Plugin entry point
│   │   └── plugin.json            # Plugin metadata
│   ├── package.json               # Node.js dependencies
│   ├── tsconfig.json              # TypeScript configuration
│   └── README.md                  # Plugin-specific documentation
└── README.md                      # This file
```

## Quick Start

### Prerequisites

- [Docker](https://www.docker.com/) and Docker Compose
- [Node.js](https://nodejs.org/) >= 14
- [Yarn](https://yarnpkg.com/) package manager

### 1. Build the Unity Plugin

First, navigate to the plugin directory and build it:

```bash
cd unity-plugin-for-grafana
yarn install
yarn build
```

### 2. Start Grafana with Docker

Create the external network (if it doesn't exist):

```bash
docker network create digital-egiz
```

Start Grafana:

```bash
docker compose up -d
```

### 3. Access Grafana

Open your browser and go to: http://localhost:3000

- **Username**: admin
- **Password**: admin (you'll be prompted to change this on first login)

### 4. Using the Unity Plugin

1. Create a new dashboard
2. Add a new panel
3. Select "Unity" as the visualization type
4. Configure your Unity WebGL build in the panel settings

## Development

### Plugin Development

For active development of the Unity plugin:

```bash
cd unity-plugin-for-grafana
yarn dev  # Starts webpack in watch mode
```

The plugin will be automatically rebuilt when you make changes.

### Plugin Development with Docker

You can also use the plugin's built-in Docker development environment:

```bash
cd unity-plugin-for-grafana
yarn server  # Starts Grafana in Docker with the plugin
```

## Features

### Unity Panel Plugin

- **Unity WebGL Integration**: Render Unity builds directly in Grafana panels
- **Bi-directional Communication**: 
  - Send Grafana data to Unity GameObjects
  - Receive events from Unity and store in Grafana variables
- **Multiple Loading Options**: External links, Grafana public folder, or file upload
- **Data Filtering**: Filter data using dashboard variables and column selection

### Docker Setup

- **Grafana OSS**: Latest version with custom configuration
- **Plugin Auto-loading**: Automatically loads the unsigned Unity plugin
- **Persistent Storage**: Grafana data and dashboards are preserved
- **Network Integration**: Connected to `digital-egiz` network for multi-service setups

## Unity Build Requirements

To use Unity builds with this plugin, your Unity project must:

1. **Build for WebGL**: Use `File > Build Settings...` and select WebGL platform
2. **Disable Compression**: In `Player Settings...`, disable compression format
3. **Include Input Script**: Add a script to disable Unity's keyboard input capture (see plugin README for details)

The build will generate 4 files that need to be loaded in the panel:
- `.loader.js` - Loader script
- `.data` - Game data
- `.framework.js` - Unity framework
- `.wasm` - WebAssembly code

## Configuration

### Docker Compose

The `docker-compose.yml` configures:
- **Port Mapping**: Grafana accessible on port 3000
- **Plugin Volume**: Mounts the built plugin
- **Storage Volume**: Persistent Grafana data
- **Environment**: Allows unsigned plugin loading

### Plugin Configuration

See `unity-plugin-for-grafana/README.md` for detailed plugin configuration options.

## Troubleshooting

### Plugin Not Loading

1. Ensure the plugin is built: `cd unity-plugin-for-grafana && yarn build`
2. Restart Grafana: `docker compose restart`
3. Check Grafana logs: `docker compose logs grafana`

### Unity Build Issues

1. Verify WebGL build settings
2. Check console for Unity-specific errors
3. Ensure all 4 Unity build files are accessible

### Network Issues

1. Ensure the `digital-egiz` network exists: `docker network ls`
2. Create it if missing: `docker network create digital-egiz`

## Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Test with the Docker setup
5. Submit a pull request

## License

This project contains components with different licenses:
- Unity Plugin: Apache-2.0 (see `unity-plugin-for-grafana/LICENSE`)
- Docker Configuration: [Your License]

## Support

For issues and questions:
- Plugin Issues: Check the Unity plugin README and GitHub issues
- Docker Issues: Verify Docker Compose configuration
- Unity Integration: Refer to Unity WebGL documentation
