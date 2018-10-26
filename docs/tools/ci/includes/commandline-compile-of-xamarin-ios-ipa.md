
Die folgende Befehlszeile zum Angeben eines Releasebuilds der Lösung **SOLUTION_FILE.sln** für das iPhone. Der Speicherort der IPA kann festgelegt werden, durch Angabe der `IpaPackageDir` Eigenschaft in der Befehlszeile:

 - Auf dem Mac mit **Xbuild**:

        xbuild /p:Configuration="Release" \ 
           /p:Platform="iPhone" \ 
           /p:IpaPackageDir="$HOME/Builds" \
           /t:Build MyProject.sln

Die **Xbuild** Befehl befindet sich in der Regel im Verzeichnis **/Library/Frameworks/Mono.framework/Commands**.

 - Auf Windows, die mit **Msbuild**:

        msbuild /p:Configuration="Release" 
            /p:Platform="iPhone" 
            /p:IpaPackageDir="%USERPROFILE%\Builds" 
            /p:ServerAddress="192.168.1.3" /p:ServerUser="macuser"  
            /t:Build MyProject.sln


**MSBuild** wird nicht automatisch erweitert `$( )` Ausdrücke mithilfe der Befehlszeile übergeben. Aus diesem Grund ist es wird empfohlen, einen vollständigen Pfad verwenden, beim Festlegen der `IpaPackageDir` an der Befehlszeile eingeben.


Finden Sie unter den [Anmerkungen zu dieser Version für iOS 9.8](https://developer.xamarin.com/releases/ios/xamarin.ios_9/xamarin.ios_9.8/#New_MSBuild_property_IpaPackageDir_to_customize_.ipa_output_location) um mehr über die `IpaPackageDir` Eigenschaft.
