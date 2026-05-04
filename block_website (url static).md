# FortiGate 60F – Website Blocking (Without License)

## 📌 Overview
This guide explains how to block specific websites (Facebook, YouTube, and Monkeytype) using Static URL Filtering on a FortiGate firewall without requiring a FortiGuard subscription.

Tested on: FortiGate 60F

---

## ⚙️ Requirements
- Admin access to FortiGate
- Basic firewall policy (LAN → WAN)
- Web browser for GUI access

---

## 🧠 Concept
Since no FortiGuard license is used:
- We rely on **Static URL Filtering**
- We manually define domains to block
- SSL Inspection is recommended for HTTPS enforcement

---

## 🚫 Websites to Block
- facebook.com
- youtube.com
- monkeytype.com

---

## 🔧 Configuration Steps (GUI)

### 1. Create Web Filter Profile
- Go to: Security Profiles → Web Filter
- Click: Create New
- Name: `Block_Sites`
- Enable: Static URL Filter

---

### 2. Add URL Filters

| URL                | Type     | Action |
|--------------------|----------|--------|
| facebook.com       | Wildcard | Block  |
| *.facebook.com     | Wildcard | Block  |
| youtube.com        | Wildcard | Block  |
| *.youtube.com      | Wildcard | Block  |
| monkeytype.com     | Wildcard | Block  |
| *.monkeytype.com   | Wildcard | Block  |

---

### 3. Apply to Firewall Policy
- Go to: Policy & Objects → Firewall Policy
- Edit: LAN → WAN policy
- Enable: Security Profiles
- Enable: Web Filter → Select `Block_Sites`

---

### 4. Enable SSL Inspection (Important)
- Go to: Security Profiles → SSL/SSH Inspection
- Use:
  - certificate-inspection (basic)
  - OR deep-inspection (recommended)
- Apply it to the same firewall policy

---

## 🧪 Testing
1. Open browser on client device
2. Try accessing:
   - facebook.com
   - youtube.com
   - monkeytype.com
3. Verify they are blocked

<img width="1917" height="985" alt="{9D1C5670-EEB9-4F92-B710-62AE121F560D}" src="https://github.com/user-attachments/assets/d4e53c13-15d5-417d-9efa-87e3c51341cf" />
<img width="1920" height="980" alt="{3D049E24-54A2-4E47-878E-E2B1FA39DC3D}" src="https://github.com/user-attachments/assets/01f5e77f-19df-410e-8d2c-d96185d48b4d" />
<img width="1920" height="987" alt="{BC067EE9-C2A7-4B65-B311-E5887AD2E71D}" src="https://github.com/user-attachments/assets/822d7557-124b-467e-8ea0-43c7b07a1574" />

### Additional Website (NSFW):

<img width="1920" height="986" alt="{68987CB6-0923-4CDD-9237-6637A1AA763C}" src="https://github.com/user-attachments/assets/fab78388-c297-48b5-9c80-d9326208324d" />

---

## ⚠️ Limitations
- No category-based filtering
- No real-time updates
- Some apps may bypass using CDNs or IPs
- HTTPS may bypass filtering without SSL inspection

---

## 🖥️ CLI Alternative (Optional)

```bash
config webfilter urlfilter
    edit 1
        set name "Block_Sites"
        config entries
            edit 1
                set url "facebook.com"
                set type wildcard
                set action block
            next
            edit 2
                set url "*.facebook.com"
                set type wildcard
                set action block
            next
        end
    next
end


GUI fortigate 60F:
creating default gateway:
Network --> static routes --> create new
destination: 0.0.0.0/0
gateway IP: 0.0.0.0

create policy to ping from PC to FORTINET:
policy & Objects --> Firewall Policy --> Create New
incoming: internal1
outgoing: wan1
source: all
destination: all
service: all

creating web filter:
Security Profiles --> Web filter
Name: block_site

