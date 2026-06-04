Same startup steps as always!

---

## Step 1 — Start VMware & Boot VM
- Open **VMware Workstation Pro**
- Start your **Ubuntu Server VM**
- Wait for it to fully boot

---

## Step 2 — SSH in via Solar-PuTTY
- Open **Solar-PuTTY**
- Click your saved `openstack` session
- Login with your password

---

## Step 3 — Activate Environment
```bash
cd ~/openstack
source bin/activate
```

---

## Step 4 — Load Credentials
```bash
export OS_AUTH_URL='http://192.168.66.3:5000'
export OS_USERNAME='admin'
export OS_PASSWORD='4554'
export OS_PROJECT_NAME='admin'
export OS_IDENTITY_API_VERSION='3'
export OS_USER_DOMAIN_NAME='Default'
export OS_PROJECT_DOMAIN_NAME='Default'
```

---

## Step 5 — Verify OpenStack is Running
```bash
openstack service list
```
Should show all 7 services. If it works you're ready!

---

## Step 6 — Open Horizon
- Browser → `http://192.168.66.4/auth/login/`
- Login: `admin` / `4554`

---

Once all that is done start **Task 1 — Create Users** in Horizon! Let me know when you're ready.
