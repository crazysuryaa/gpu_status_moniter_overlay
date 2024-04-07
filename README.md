# gpu_status_moniter_overlay

This project sets up a Conky-based overlay to display GPU status information directly on the desktop. It leverages conky, a lightweight, scriptable system monitor, to fetch and display GPU metrics such as temperature, power usage, memory usage, and utilization.

# Conky Configuration (gpu_status_conky.conf)
The Conky configuration file specifies the GPU metrics to monitor, along with Conky's appearance and behavior. Key settings include:

update_interval: How often Conky updates the data.

own_window_type: Defines how Conky's window is treated by the window manager. We explored using override to make it an overlay but settled on alternatives like dock for compatibility.

own_window_hints: Additional window properties like undecorated, above, and sticky to control the decoration and placement.

own_window_argb_visual and own_window_argb_value: Enable transparent backgrounds for the overlay.

This file also uses nvidia-smi commands within template blocks to fetch real-time GPU statistics.

# Systemd Service (gpu_status_conky.service)

The systemd service file allows us to manage the Conky overlay as a service, offering commands to start, stop, and restart the overlay, and ensuring it runs automatically on system startup.

Key Components:

[Unit]: Describes the service, dependencies, and when it should start.

[Service]: Specifies how the service is run, including the user context, environment variables (like DISPLAY for GUI applications), and the command to start Conky with our configuration file.

[Install]: Determines when the service should be automatically started. WantedBy=graphical.target ensures it starts with the graphical desktop.

The service file explicitly sets DISPLAY=:0 to address issues where Conky cannot find the display when started by systemd, a common scenario for services trying to run graphical applications.

Modify this by  accordingly to your display id, (echo $DISPLAY)

Make sure you also replace USERNAME with your actual username



# Setup Instructions
Install Conky: Ensure Conky is installed on your system.

Configure Conky: Create the gpu_status_conky.conf file with the desired GPU metrics and appearance settings.

Set Up the Service: Create the gpu_status_conky.service file in /etc/systemd/system/, specifying the user and pointing to your Conky configuration.

Enable and Start the Service: Use systemctl commands to enable the service to start at boot and to start it immediately.

# Managing the Service
Reload: sudo systemctl daemon-reload
Start: sudo systemctl start gpu_status_conky.service
Stop: sudo systemctl stop gpu_status_conky.service
Status: sudo systemctl status gpu_status_conky.service
Enable on Boot: sudo systemctl enable gpu_status_conky.service

# Security Considerations
The xhost +SI:localuser:YOUR_USERNAME command grants the specified user permission to access your X session. Use this command judiciously, especially on multi-user systems.