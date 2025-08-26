# Remote Desktop Setup Guide

A step-by-step guide to configure your Windows PC for remote access using Windows Remote Desktop Protocol (RDP).

## Prerequisites

-   Windows 10/11 **Pro, Enterprise, or Education** edition. (Home edition does not support being the host).
-   Physical access to the PC you want to access remotely.
-   Access to your wireless router's admin panel.
-   The PC must have a **password set** for the user account.

---

## Step 1: Verify Your Windows Edition

1.  Press `Win + R`.
2.  Type `winver` and press `Enter`.
3.  A window will appear. If it says **"Windows 11/10 Home,"** you cannot use native RDP. Use a third-party tool like TeamViewer or AnyDesk.
4.  If it says **"Pro"** or **"Enterprise,"** you can proceed.

## Step 2: Enable Remote Desktop & Get PC Name

1.  Press `Win + S` and search for **"Remote Desktop Settings"**. Open it.
2.  Toggle **"Enable Remote Desktop"** to **On**. Confirm the change.
3.  **Get the PC Name:**
    -   Press `Win + R`, type `cmd`, and press `Enter`.
    -   In the command prompt, type `hostname` and press `Enter`.
    -   **Note down** the computer name that appears.

## Step 3: Find IP and MAC Address

In the same command prompt window (`cmd`):
1.  Type `ipconfig /all` and press `Enter`.
2.  Look for your active network adapter (e.g., **Wi-Fi** or **Ethernet**).
3.  Find and note these two pieces of information:
    -   **IPv4 Address:** (e.g., `192.168.0.101`) - This is your PC's current IP.
    -   **Physical Address:** (e.g., `XX-XX-XX-XX-XX-XX`) - This is your MAC Address.

## Step 4: Configure Your Router

### Part A: Assign a Static IP via DHCP Reservation

1.  Open a web browser on the same PC.
2.  Go to your router's admin address (common ones: `192.168.0.1`, `192.168.1.1`).
3.  Log in (check the router's sticker for default username/password).
4.  Find the section: **"DHCP Reservation," "Static IP Assignment,"** or **"Address Reservation."**
5.  **Add a new rule:**
    -   **MAC Address:** Enter the Physical Address you noted down.
    -   **IP Address:** Assign a static IP **outside** your router's DHCP range (e.g., if the range is `192.168.0.100` - `200`, use `192.168.0.50`).
    -   **Status:** Enable.
6.  **Save** the settings. The PC will now always get this IP address.

### Part B: Set Up Port Forwarding

1.  In your router's admin panel, find the **"Port Forwarding"** or **"Virtual Server"** section.
2.  **Create a new rule** with the following details:
    | Setting | Value to Enter |
    | :--- | :--- |
    | **Service Name** | `RDP` (or any name) |
    | **External Port** | `3389` |
    | **Internal Port** | `3389` |
    | **Internal IP Address** | The static IP you reserved (e.g., `192.168.0.50`) |
    | **Protocol** | `TCP` |
3.  **Save** or **Apply** the settings.

## Step 5: Find Your Public IP

1.  On the same PC, open a web browser and go to **[https://whatismyip.com](https://whatismyip.com)**.
2.  **Note down** the public IP address shown (e.g., `103.145.159.126`). This is your internet address.

## Step 6: Know Your Username

1.  Press `Win + R`, type `cmd`, press `Enter`.
2.  In the command prompt, type `echo %username%` and press `Enter`.
3.  **Note down** the username that appears. This is the exact username for login.

## Step 7: Connect from Your Remote Device (e.g., Android)

1.  On your remote device (phone, another PC), download the official **Microsoft Remote Desktop** app.
2.  Open the app and create a new connection.
3.  In the **"PC Name"** field: Enter the **Public IP** address you noted down.
4.  For credentials:
    -   **Username:** Enter the username from `echo %username%`.
    -   **Password:** Enter your Windows login password.
    -   **Domain:** (Leave this field blank).
5.  Save the connection and tap to connect.

`netsh advfirewall firewall set rule group="remote desktop" new enable=Yes`


---
**Note:** If your public IP changes (it often does for home connections), you will need to check it again on the host PC before connecting. For a permanent solution, look into **Dynamic DNS (DDNS)** services.
