
Die folgende Befehlszeile, geben Sie einen Releasebuild der Lösung **SOLUTION_FILE.sln** für das iPhone. Der Speicherort der IPA kann festgelegt werden, indem die die `IpaPackageDir` Eigenschaft in der Befehlszeile:

 - Auf dem Mac verwenden **Xbuild**:

        xbuild /p:Configuration="Release" \ 
           /p:Platform="iPhone" \ 
           /p:IpaPackageDir="$HOME/Builds" \
           /t:Build MyProject.sln

Die **Xbuild** Befehl befindet sich in der Regel im Verzeichnis **/Library/Frameworks/Mono.framework/Commands**.

 - Bei Verwendung von Windows **Msbuild**:

        msbuild /p:Configuration="Release" 
            /p:Platform="iPhone" 
            /p:IpaPackageDir="%USERPROFILE%\Builds" 
            /p:ServerAddress="192.168.1.3" /p:ServerUser="macuser"  
            /t:Build MyProject.sln


**MSBuild** wird nicht automatisch erweitert `$( )` von der Befehlszeile übergebenen Ausdrücken. Aus diesem Grund ist es wird empfohlen, einen vollständigen Pfad verwenden, der zum Einstellen der `IpaPackageDir` in der Befehlszeile.


Finden Sie unter der [Anmerkungen zu dieser Version für iOS 9.8](https://developer.xamarin.com/releases/ios/xamarin.ios_9/xamarin.ios_9.8/#New_MSBuild_property_IpaPackageDir_to_customize_.ipa_output_location) Weitere Details zu den `IpaPackageDir` Eigenschaft.
