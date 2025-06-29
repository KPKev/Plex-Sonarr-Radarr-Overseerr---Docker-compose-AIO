# Automated Media Server Docker Stack

A Docker Compose file for deploying a comprehensive, automated media server stack, including Plex, Sonarr, Radarr, SABnzbd, and Overseer. It is pre-configured for use on a NAS and designed to integrate seamlessly with a remote transcoding solution for optimal performance.

## Services Included

*   **Plex:** The media server for organizing and streaming your content.
*   **Sonarr:** For automatically downloading, sorting, and managing TV series.
*   **Radarr:** For automatically downloading, sorting, and managing movies.
*   **SABnzbd:** The Usenet download client that handles the downloads from Sonarr and Radarr.
*   **Overseer:** A request management and media discovery tool for your users.

## Prerequisites

*   A host machine with Docker and Docker Compose installed (e.g., a Synology NAS, Unraid, or any Linux server).
*   A directory structure created on your host for storing configuration files and media.

## Setup Instructions

1.  **Clone or Download:**
    *   Clone this repository to a location on your host machine:
        ```bash
        git clone https://github.com/KPKev/--Automated-Workflow-Integrates-directly-with-SABnzbd-as-a-post-processing-script.git
        cd --Automated-Workflow-Integrates-directly-with-SABnzbd-as-a-post-processing-script
        ```

2.  **Review Directory Mappings:**
    *   Open the `docker-compose.yml` file.
    *   Carefully review the `volumes` section for each service. The left side of the colon (`:`) is the path on your **host machine (your NAS)**. The right side is the path *inside* the Docker container (you usually don't need to change this).
    *   **You must update the host paths** to match the directory structure on your NAS where you want to store configuration files and media. For example, change `./plex/configs:/config` to `/volume1/docker/plex:/config`.

3.  **Set User and Group IDs:**
    *   In the `environment` section for each service, you will see `PUID` (User ID) and `PGID` (Group ID).
    *   You should change these to match the user on your NAS that should own the files. This is critical for avoiding permission errors. You can find these IDs by SSHing into your NAS and running the command `id your_username`.

4.  **Deploy the Stack:**
    *   Once you have updated the volumes and user IDs, run the following command from the same directory as the `docker-compose.yml` file:
        ```bash
        docker-compose up -d
        ```
    *   This will download the container images and start all the services in the background.

## Integration with Remote Transcoding

This Docker stack is designed to work with the [Remote-Transcode-Bridge](https://github.com/KPKev/Remote-Transcode-Bridge) script. To complete the integration:

1.  Ensure your SABnzbd container has the necessary tools (`openssh-client`, `ffmpeg`, `ffprobe`). You may need to build a custom Docker image using the provided `sabnzbd.Dockerfile`.
2.  Follow the setup instructions in the [Remote-Transcode-Bridge repository](https://github.com/KPKev/Remote-Transcode-Bridge) to configure the script and link it to SABnzbd.

## License

This project is licensed under the MIT License. 