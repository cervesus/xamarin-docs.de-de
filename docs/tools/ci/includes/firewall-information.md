## <a name="firewall-configuration"></a>Konfiguration der Firewall

In der Reihenfolge für Tests an Xamarin Test Cloud übermittelt werden muss der Computer, übermitteln die Tests mit den Test Cloud-Servern kommunizieren. Firewalls müssen konfiguriert werden, zum Zulassen des Netzwerkverkehrs zu und von den Servern am **testcloud.xamarin.com** an Port 80 und 443. Dieser Endpunkt wird von DNS verwaltet, und die IP-Adresse sind vorbehalten. 

In einigen Situationen muss einen Test (oder ein Gerät, das Ausführen des Tests) mit Webservern, die durch eine Firewall geschützt, kommunizieren. In diesem Szenario muss die Firewall zum Zulassen von Datenverkehr von den folgenden IP-Adressen konfiguriert werden:

* **195.249.159.238**
* **195.249.159.239**
