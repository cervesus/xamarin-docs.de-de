## <a name="firewall-configuration"></a>Konfiguration der Firewall

Damit für Tests zur Xamarin Test Cloud übermittelt werden kann muss der Computer, senden die Tests mit den Test-Cloud-Servern kommunizieren. Firewalls müssen konfiguriert werden, um das Zulassen von Netzwerkdatenverkehr zu und von den Servern am **testcloud.xamarin.com** an den Ports 80 und 443. Dieser Endpunkt wird vom DNS verwaltet, und die IP-Adresse unterliegt. 

In einigen Situationen muss einen Test (oder ein Gerät, indem Sie den Test) mit Web-Server durch eine Firewall geschützt kommunizieren. In diesem Szenario muss die Firewall zum Zulassen von Datenverkehr von den folgenden IP-Adressen konfiguriert werden:

* **195.249.159.238**
* **195.249.159.239**
