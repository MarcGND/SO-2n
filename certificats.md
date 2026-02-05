---
layout: default
title: "Ubuntu AD + Certificats"
---

# Unir ubuntu a AD amb un script

En primer lloc es important tenir la màquina ubuntu dins del rang de xarxa del nostre servidor Windows. Amb el següent script he conseguit unir el client ubuntu al domini marc.sabate de Windows. (Script complet al final de la pagina)

<img width="1832" height="748" alt="1" src="https://github.com/user-attachments/assets/fa574a25-fc4c-4089-b943-df4944d0b09e" />

Aquesta primera part consisteix en definir les variables, de quina es l'adreça IP del servidor quin es l'usuari i definir també el nom de l'equip Ubuntu, després es passen a fer les configuracions de xarxa i també es sincronitza l'hora (error habitual)

<img width="1723" height="582" alt="2" src="https://github.com/user-attachments/assets/77fb1fcb-88b6-4d99-92c8-eeb3dcd49fba" />

Seguidament la segona part intenta instal·lar tots els paquets necessaris i intenta unir l'equip al domini.

<img width="1853" height="807" alt="3" src="https://github.com/user-attachments/assets/1c27521d-7ee0-419b-bf75-e7f132514313" />

Com podem veure a la imatge surt que ens em unit correctament al domini i ens configura l'sssd, els inicis de sessió amb home i per últim el kerberos.

<img width="1855" height="808" alt="4" src="https://github.com/user-attachments/assets/fa7c0e13-1e44-4eea-bbc5-2192486c6d4d" />


Per ultim fa comprovacions per veure que realment estem units al domini, mostrant el tiquet de kerberos. També executo la comanda realm list per veure si el domini hi es.

<img width="602" height="328" alt="5" src="https://github.com/user-attachments/assets/bc469e21-e636-4886-bccd-b9989e058ea8" />

Desrpés si reiniciem el client podem accedir amb qualsevol usuari de l'AD, com es el cas de Gina, si volem accedir amb un altre podem fer-ho clicant a no esteu llistat.

<img width="758" height="285" alt="6" src="https://github.com/user-attachments/assets/1e24a333-7cfc-42fb-97e3-7f14374dd9ea" />

D'altra banda a l'AD ens apareixerà a computers el nostre equip.

<img width="684" height="666" alt="7" src="https://github.com/user-attachments/assets/4b2a8a4f-d6b2-4d21-941d-079c648a8996" />

Com podem veure hi es l'usuari Gina.

<img width="1025" height="546" alt="10" src="https://github.com/user-attachments/assets/6ba7be53-26c0-46b8-acd7-0246dd6d65fc" />


Si entrem podrem veure la seva informació i com aquest usuari pertany al domini.

# Certificat IIS

Per instal·lar un certificat d'AD i configurar-lo necessitarem els següents rols, també cal afegir que haurem de crear una web.

<img width="786" height="558" alt="1" src="https://github.com/user-attachments/assets/29e46027-fde3-44b4-a7f9-5254c8884020" />

<img width="1910" height="340" alt="2" src="https://github.com/user-attachments/assets/f11b8757-5f9c-41d9-81af-48cf9ca93781" />

Un cop tenim els rols instal·lats, crearem la web, primer una carpeta on guardarem l'index, el certificat i tots els arxius.

<img width="1118" height="590" alt="3" src="https://github.com/user-attachments/assets/bab2c53f-f947-4d26-ac39-9dfeba43b381" />


Crearem una cosa bàscia per fer les proves. Amb un index html.

<img width="575" height="188" alt="4" src="https://github.com/user-attachments/assets/271b9b9e-6841-4c7b-b925-d1d6b590a515" />

A part de l'html necessitarem els arxius de configuració.

<img width="772" height="178" alt="5" src="https://github.com/user-attachments/assets/fcc5bc49-6e30-4d15-b59d-4ac6cc4f5cc5" />

Abans de seguir hem de crear als DNS el nom de la nostra web, jo he escollit marc.cat.

<img width="934" height="211" alt="dns" src="https://github.com/user-attachments/assets/0884d590-5c5d-4b97-9bd5-b251efc7411b" />



Per crear el certificat he executat les següents comandes per PoweShell.

```
New-SelfSignedCertificate -DnsName "web.marc.cat" -CertStoreLocation "Cert:\LocalMachine\My"
```
```
makecert -r -pe -n "CN=web.marc.cat" -ss my -sr localmachine -sky exchange -sp "Microsoft RSA SChannel Cryptographic Provider" -sy 12
```
<img width="1212" height="475" alt="comandescert" src="https://github.com/user-attachments/assets/018972a7-e8d1-4243-a7c1-2ed30420b0ea" />

Per assignar-lo hem d'anar a la configuració IIS, clicar al lloc web que em creat, i als enllaços al del port 443 (https) entrem i li assignem el certificat SSL que hem creat.

<img width="1911" height="695" alt="6" src="https://github.com/user-attachments/assets/02912174-0256-4730-b61c-5457bb21db1b" />

Per últim, comprovarem que s'hagi assignat entrant a l'url amb el nom del DNS, veurem que la web es segura i si pitjem al candau de dalt a l'essquerra i anem a certificats ens apareixerà el certificat que hem assignat.

<img width="1480" height="762" alt="final" src="https://github.com/user-attachments/assets/e290cc58-58b4-419f-a43b-509c9f2d52ae" />




Script d'unió ubuntu a AD

```
#!/bin/bash

##############################################################################
# SCRIPT DEFINITIU - Ubuntu + Active Directory (VERSIÓ FINAL CORREGIDA)
# Incorpora totes les solucions que han funcionat als companys
##############################################################################

set -e

# ==================== CONFIGURACIÓ ====================
DOMAIN="marc.sabate"
REALM="MARC.SABATE"
AD_SERVER="192.168.2.101"
AD_USER="Administrador"  # O "Administrator" si no funciona

# IP d'aquesta màquina
CLIENT_IP="192.168.2.104"
CLIENT_HOSTNAME="marcsr"

# Interfícies de xarxa
INTERFACE_INTERNAL="enp0s8"  # Xarxa amb AD
INTERFACE_BRIDGE="enp0s3"    # Xarxa per Internet (NAT)

# ======================================================

# Colors
GREEN='\033[0;32m'
RED='\033[0;31m'
YELLOW='\033[1;33m'
BLUE='\033[0;34m'
CYAN='\033[0;36m'
NC='\033[0m'

print_header() {
    echo -e "${CYAN}==========================================${NC}"
    echo -e "${CYAN}$1${NC}"
    echo -e "${CYAN}==========================================${NC}"
    echo ""
}

print_step() {
    echo -e "${BLUE}[$1] $2${NC}"
}

check_result() {
    if [ $1 -eq 0 ]; then
        echo -e "${GREEN}✓ $2${NC}"
        return 0
    else
        echo -e "${RED}✗ $2${NC}"
        return 1
    fi
}

error_exit() {
    echo -e "${RED}ERROR: $1${NC}"
    exit 1
}

if [ "$EUID" -ne 0 ]; then
    error_exit "Aquest script s'ha d'executar com a root. Usa: sudo $0"
fi

clear
print_header "  UNIÓ AL DOMINI ACTIVE DIRECTORY - VERSIÓ FINAL"
echo "Domini: $DOMAIN"
echo "Servidor AD: $AD_SERVER"
echo "IP Client: $CLIENT_IP"
echo "Hostname: $CLIENT_HOSTNAME"
echo "Usuari AD: $AD_USER"
echo ""

# Demanar contrasenya
echo -e "${YELLOW}Introdueix la contrasenya de l'usuari $AD_USER del domini:${NC}"
read -s AD_PASSWORD
echo ""

if [ -z "$AD_PASSWORD" ]; then
    error_exit "La contrasenya no pot estar buida"
fi

##############################################################################
# 1. CONFIGURAR HOSTNAME I HOSTS
##############################################################################
print_step "1/12" "Configurant hostname i hosts..."

hostnamectl set-hostname "${CLIENT_HOSTNAME}.${DOMAIN}"

# Configurar hosts correctament
cat > /etc/hosts << EOF
127.0.0.1       localhost
127.0.1.1       ${CLIENT_HOSTNAME}.${DOMAIN} ${CLIENT_HOSTNAME}
${CLIENT_IP}    ${CLIENT_HOSTNAME}.${DOMAIN} ${CLIENT_HOSTNAME}
${AD_SERVER}    vpn.${DOMAIN} vpn
EOF

check_result 0 "Hostname configurat: $(hostname -f)"
echo ""

##############################################################################
# 2. ARREGLAR PROBLEMA .local (SI ÉS EL CAS)
##############################################################################
print_step "2/12" "Arreglant problema .local..."

# Si el domini acaba en .local, cal arreglar nsswitch.conf
if [[ "$DOMAIN" == *.local ]]; then
    sed -i 's/mdns4_minimal \[NOTFOUND=return\]/dns/g' /etc/nsswitch.conf
    echo "Arreglat nsswitch.conf per domini .local"
fi

echo ""

##############################################################################
# 3. CONFIGURAR XARXA I DNS
##############################################################################
print_step "3/12" "Configurant xarxa i DNS..."

# Crear configuració netplan
cat > /etc/netplan/01-ad-config.yaml << EOF
network:
  version: 2
  renderer: networkd
  ethernets:
    ${INTERFACE_BRIDGE}:
      dhcp4: true
    ${INTERFACE_INTERNAL}:
      dhcp4: no
      addresses: [${CLIENT_IP}/24]
      nameservers:
        addresses: [${AD_SERVER}, 8.8.8.8]
        search: [${DOMAIN}]
      routes:
        - to: 192.168.2.0/24
          via: ${AD_SERVER}
EOF

chmod 600 /etc/netplan/01-ad-config.yaml
netplan apply
sleep 3

# Configurar DNS temporalment
rm -f /etc/resolv.conf
cat > /etc/resolv.conf << EOF
nameserver ${AD_SERVER}
nameserver 8.8.8.8
EOF

check_result 0 "Xarxa i DNS configurats"
echo ""

##############################################################################
# 4. SINCRONITZAR HORA (CRÍTIC PER A KERBEROS)
##############################################################################
print_step "4/12" "Sincronitzant hora amb servidor AD..."

apt install -y ntpdate 2>/dev/null || true
ntpdate -u ${AD_SERVER} 2>/dev/null || echo "  ⚠ No s'ha pogut sincronitzar hora"

check_result 0 "Hora sincronitzada (aproximadament)"
echo ""

##############################################################################
# 5. INSTAL·LAR PAQUETS NECESSARIS
##############################################################################
print_step "5/12" "Instal·lant paquets..."

DEBIAN_FRONTEND=noninteractive apt update -qq
DEBIAN_FRONTEND=noninteractive apt install -y \
    krb5-user samba sssd sssd-tools \
    libnss-sss libpam-sss adcli realmd \
    packagekit policykit-1 smbclient \
    > /dev/null 2>&1

check_result $? "Paquets instal·lats"
echo ""

##############################################################################
# 6. UNIR AL DOMINI AMB SAMBA (LA CLAU!)
##############################################################################
print_step "6/12" "Unint al domini (mode Samba)..."

# Aturar serveis i netejar
systemctl stop sssd 2>/dev/null || true
rm -f /etc/krb5.keytab 2>/dev/null || true
realm leave 2>/dev/null || true

echo ""
echo -e "${YELLOW}======================================================${NC}"
echo -e "${YELLOW}UNINT AL DOMINI ${DOMAIN} amb usuari ${AD_USER}${NC}"
echo -e "${YELLOW}======================================================${NC}"
echo ""

# Unir amb Samba (evita problemes de GPO i permisos)
if echo "$AD_PASSWORD" | realm join -v -U "$AD_USER" "$DOMAIN" --membership-software=samba 2>&1 | grep -v "password"; then
    echo -e "${GREEN}✅ DOMINI UNIT CORRECTAMENT!${NC}"
else
    echo "⚠ Provant amb usuari 'Administrator'..."
    AD_USER="Administrator"
    if echo "$AD_PASSWORD" | realm join -v -U "$AD_USER" "$DOMAIN" --membership-software=samba 2>&1 | grep -v "password"; then
        echo -e "${GREEN}✅ DOMINI UNIT amb usuari Administrator!${NC}"
    else
        error_exit "No s'ha pogut unir al domini amb cap usuari"
    fi
fi

echo ""

##############################################################################
# 7. CONFIGURAR SSSD (CONFIGURACIÓ PERMISSIVA)
##############################################################################
print_step "7/12" "Configurant SSSD (mode permissiu)..."

cat > /etc/sssd/sssd.conf << EOF
[sssd]
domains = $DOMAIN
config_file_version = 2
services = nss, pam

[domain/$DOMAIN]
default_shell = /bin/bash
krb5_store_password_if_offline = True
cache_credentials = True
krb5_realm = $REALM
id_provider = ad
fallback_homedir = /home/%u
ad_domain = $DOMAIN
use_fully_qualified_names = False
ldap_id_mapping = True

# AQUESTA ÉS LA CLAU - PERMET TOT PER EVITAR PROBLEMES DE GPO
access_provider = permit
EOF

chmod 600 /etc/sssd/sssd.conf
chown root:root /etc/sssd/sssd.conf

# Netejar caches velles
rm -rf /var/lib/sssd/db/* 2>/dev/null || true

check_result 0 "SSSD configurat (mode permissiu)"
echo ""

##############################################################################
# 8. ACTIVAR CARPETA HOME AUTOMÀTICA
##############################################################################
print_step "8/12" "Activant creació automàtica de home..."

pam-auth-update --enable mkhomedir --force > /dev/null 2>&1

echo "session required pam_mkhomedir.so skel=/etc/skel umask=0022" >> /etc/pam.d/common-session 2>/dev/null || true

check_result 0 "Home automàtic activat"
echo ""

##############################################################################
# 9. INICIAR I ACTIVAR SSSD
##############################################################################
print_step "9/12" "Iniciant serveis..."

systemctl restart sssd
systemctl enable sssd > /dev/null 2>&1

check_result 0 "SSSD iniciat i activat"
echo ""

##############################################################################
# 10. CONFIGURAR KERBEROS (OPCIONAL)
##############################################################################
print_step "10/12" "Configurant Kerberos..."

cat > /etc/krb5.conf << EOF
[libdefaults]
    default_realm = $REALM
    dns_lookup_realm = false
    dns_lookup_kdc = false
    ticket_lifetime = 24h
    renew_lifetime = 7d
    forwardable = true

[realms]
    $REALM = {
        kdc = $AD_SERVER
        admin_server = $AD_SERVER
    }

[domain_realm]
    .$DOMAIN = $REALM
    $DOMAIN = $REALM
EOF

check_result 0 "Kerberos configurat"
echo ""

##############################################################################
# 11. ESPERAR I VERIFICAR
##############################################################################
print_step "11/12" "Esperant sincronització..."

echo "Esperant 15 segons per sincronització SSSD..."
sleep 15

# Forçar actualització de cache
sss_cache -E 2>/dev/null || true
systemctl restart sssd
sleep 5

echo ""

##############################################################################
# 12. VERIFICACIÓ FINAL
##############################################################################
print_step "12/12" "Verificació final..."

echo ""
echo -e "${CYAN}══════════════════════════════════════════════════${NC}"
echo -e "${CYAN}                 VERIFICACIÓ FINAL                 ${NC}"
echo -e "${CYAN}══════════════════════════════════════════════════${NC}"
echo ""

echo "1. Estat del domini:"
realm list 2>/dev/null | grep -E "(realm|configured|login)" || echo "  ⚠ No s'ha trobat informació"
echo ""

echo "2. Usuaris del domini detectats:"
for user in "$AD_USER" "Administrador" "Administrator" "administrador" "administrator"; do
    if id "$user" 2>/dev/null; then
        echo -e "  ✅ ${GREEN}Trobat: $user${NC}"
        break
    fi
done
echo ""

echo "3. Estat de SSSD:"
systemctl status sssd --no-pager | grep -A2 "Active:" || echo "  ⚡ SSSD no està actiu"
echo ""

echo "4. Ticket Kerberos:"
klist 2>/dev/null && echo "  ✅ Ticket actiu" || echo "  ⚠ No hi ha ticket actiu"
echo ""

echo -e "${CYAN}══════════════════════════════════════════════════${NC}"
echo ""
echo -e "${GREEN}🎉 PROCÉS COMPLETAT!${NC}"
echo ""
echo -e "${YELLOW}PASOS SEGÜENTS:${NC}"
echo "1. Reinicia la màquina: ${GREEN}sudo reboot${NC}"
echo "2. Després del reinici, prova:"
echo "   - ${CYAN}id $AD_USER${NC}"
echo "   - ${CYAN}su - $AD_USER${NC}"
echo "   - ${CYAN}getent passwd${NC} | grep $DOMAIN"
echo ""
echo "Si tens problemes, executa:"
echo "  sudo systemctl restart sssd"
echo "  sudo sss_cache -E"
echo ""

```


[← Torna a l'índex](index.html)
