# SSL VPN Configuration Documentation

## SSL VPN Settings

Navigate to:

`VPN → SSL-VPN Settings`

### Configuration

| Setting | Value |
|---|---|
| Listen on Interface | `INTERNET (wan1)` |
| Listen on Port | `4433` |
| Server Certificate | `fortinet-factory` (Built-in Certificate) |

### Authentication / Portal Mapping

1. Click **Create New**
2. Configure the following:
   - **User/Group:** `LOCAL/GUEST GROUP`
   - **Portal:** `All Other Users/Groups`
3. Click **Apply**

![SSL VPN Settings](https://github.com/user-attachments/assets/02f42262-7890-4d91-9cb7-1f020dd27294)

---

# User & Authentication Configuration

Navigate to:

`User & Authentication → User Definition`

1. Select the user: `Guest`
2. Click **Edit**

## User Configuration

| Setting | Value |
|---|---|
| Username | `guest` |
| Password | `C1sc0123` |
| User Group | `Guest-Group` |

![User Definition](https://github.com/user-attachments/assets/0b5ef7ec-b5ee-4761-b51d-e721e5e717ab)

![Guest Group](https://github.com/user-attachments/assets/075b2898-3aff-466b-b987-01515b227cb4)

---
# Create a firewall policy for WIFI_SSL_VPN_LAN:

| Setting | Value |
|---|---|
| Name | WIFI_VPN_POLICY |
| Incoming Interface | SSL-VPN tunnel interface (ssl.root) |
| Outgoing Interface | LAN |
| Source | VPN User Group |
| Destination | Internal Subnet |
| Schedule | always |
| Service | ALL |
| Action | ACCEPT |
| NAT | OFF |

<img width="1917" height="687" alt="{993B844A-46E8-499C-A551-6D6ED3F2BBFA}" src="https://github.com/user-attachments/assets/5413549c-f7f7-455d-a516-48d5e2ba99ca" />

# Why NAT Should Be Disabled
For internal LAN access, NAT should typically be disabled.

# Reason
Disabling NAT allows internal devices to see the actual VPN client IP address instead of the firewall WAN IP.

---
# Create a firewall policy for WIFI_SSL_VPN_WAN:

| Setting | Value |
|---|---|
| Name | WIFI_VPN_SSL_WAN |
| Incoming Interface | SSL-VPN tunnel interface (ssl.root) |
| Outgoing Interface | INTERNET (wan1) |
| Source | TEST GROUP |
| Destination | all |
| Schedule | always |
| Service | ALL |
| Action | ACCEPT |
| NAT | ON |

1st option:
<img width="1920" height="924" alt="{9C32BC26-EE59-4E6B-AB9A-A3D5819ECDAA}" src="https://github.com/user-attachments/assets/500fc9aa-65c4-40a4-b2c1-b338d831e647" />

2nd option:
<img width="1370" height="913" alt="{79E7C78E-C775-4F8F-BD66-5A4B05706B62}" src="https://github.com/user-attachments/assets/1cc6830f-a6a6-4b29-a479-fff56e2e2523" />

---

# FortiClient VPN Setup

## Creating a New VPN Profile

1. Open the **FortiClient VPN App**
2. Click **New VPN**
3. Select:
   - **VPN Name**
   - **SSL-VPN**
4. Click **Create**

## VPN Configuration Values

| Setting | Value |
|---|---|
| Server | `200.0.0.61` |
| Port | `4433` |
| Username | `guest` |
| Certificate | `Built-in` |
| Single Sign-On (SSO) | `Disabled` |
| Prompt User Credentials | `Enabled` |

> **Note:**  
> The default SSL-VPN port is `443`, but `4433` was used to avoid port conflicts/overlap.

![FortiClient VPN Setup](https://github.com/user-attachments/assets/f8ffad4e-8de3-4c41-9c16-ddb59d13c3de)


## Expected Output:

<img width="380" height="842" alt="{EAB39904-CAE5-408C-AFCB-0CE2C06E9358}" src="https://github.com/user-attachments/assets/23001283-d892-4e0a-890d-076e401884c9" />





---
